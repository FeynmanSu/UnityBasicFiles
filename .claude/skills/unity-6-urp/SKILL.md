---
name: unity-6-urp
description: "Specialized for Unity 6000.3.x (6.3) URP development, Render Graph authoring, HLSL shader optimization, and performance tuning. Activate when the user requests creating new Unity scenes, writing new features, optimizing Unity projects or code, writing shaders, or optimizing URP performance."
allowed-tools: "Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch"
---

# Unity 6000.3.x (6.3) URP Development Guide

You are an expert Unity 6000.3.x (6.3) developer, well-versed in the latest **Render Graph system**, **GPU Resident Drawer**, and **Forward+** rendering mode.

---

## 1. Code Standards and Architecture

- **API Guidelines**: Prefer Unity 6000.3.x (6.3)+ exclusive APIs.
  - Using the legacy `Execute` / `cmd.DrawRenderer` in `ScriptableRenderPass` is prohibited. All drawing must be done within `RecordRenderGraph` via `RenderGraphContext`.
  - Use `RTHandles` and `TextureHandle` instead of legacy `RenderTexture`. For full-screen post-processing or Copy Color, prefer the `Blitter` API (`Blitter.BlitCameraTexture()`). Do not use `Graphics.Blit()`.
- **Namespaces**: Default to `UnityEngine`, `UnityEngine.Rendering`, `UnityEngine.Rendering.Universal`.
- **Naming Conventions**: Classes/Methods `PascalCase`; private fields `m_camelCase`; serialized fields `[SerializeField] private`; Shader properties `_PascalCase`.
- **Async**: Prefer `async/await` (UniTask or .NET Task). Avoid `Coroutine`.
- **Comments**: Complex logic should include Unity 6 official documentation links (see Section 4).

---

## 2. URP Shader (HLSL) Standards

- **Core Library**: Must `#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"`.
- **SRP Batcher**: All material properties must be declared within `CBUFFER_START(UnityPerMaterial) ... CBUFFER_END`, including standard variables like `_BaseMap_ST` to ensure batching.
- **Texture Declaration**: Use `TEXTURE2D` / `SAMPLER` macros. Do not use `sampler2D`.
- **Platform Precision**:
  - Mobile: Color and lighting calculations must use `half`. UV coordinates must remain `float2` (to avoid precision artifacts). **Do not** downgrade to `half2`.
  - PC / Console: May use `float` throughout, but explicit precision declarations help maintain cross-platform Shader consistency.
- **Platform Branching**: Use `#if SHADER_API_MOBILE` to separate Mobile optimization paths. Use `#pragma multi_compile` to define feature variants and strip unnecessary combinations via URP Asset Shader Stripping.
- **`discard` / `clip()` prohibited** on Mobile opaque objects (breaks tile-based GPU Early-Z).
- **Loops and Early Return**: Avoid early `return` in functions with fixed loops (FXC may report "potentially uninitialized variable"). Use mask multiplication (`result *= saturate(condition * 1e6)`) to control output, or use macros to expand into separate functions for each quality tier.

---

## 3. Render Graph Core Patterns

### Resource Declaration
```csharp
// Read:
builder.UseTexture(handle, AccessFlags.Read);
// Write (Color Attachment):
builder.UseTextureFragment(handle, 0, IBaseRenderGraphBuilder.AccessFlags.Write);
// Depth Attachment:
builder.UseTextureFragmentDepth(depthHandle, IBaseRenderGraphBuilder.AccessFlags.Write);
```

### Global Texture Passing (Critical Pitfall)
```csharp
// ❌ Wrong: TextureHandle is not a Texture, fails silently
Shader.SetGlobalTexture(_id, textureHandle);

// ✅ Correct: Declare within the pass's using block
using (var builder = renderGraph.AddRasterRenderPass<PassData>("MyPass", out var passData))
{
    builder.SetGlobalTextureAfterPass(textureHandle, _id);
}
```

### Persistent RTs (Temporal History, etc.)
- RTs that need to persist across frames (e.g., temporal accumulation) are created and managed by `ScriptableRendererFeature` as `RenderTexture`. They are **not allocated** through Render Graph (RG-allocated resources are only valid for the current frame).
- Persistent RTs are passed to Shaders via `Shader.SetGlobalTexture(id, renderTexture)` (regular `RenderTexture` can be set directly; `SetGlobalTextureAfterPass` is not needed).

### Pass Type Selection
| Scenario | API to Use |
|----------|-----------|
| Full-screen Post-processing / Blit | `AddRasterRenderPass` + `Blitter` |
| Custom Geometry Drawing | `AddRasterRenderPass` + `RendererList` |
| Compute Shader | `AddComputePass` |
| Pure CPU Logic (no GPU resources) | `AddUnsafePass` (use sparingly) |

---

## 4. Performance Optimization

- **GPU Resident Drawer**: Enable for scenes with many repeated static objects. Ensure objects' Mesh and Material support GPU instancing (add `#pragma instancing_options` to Shader).
- **Forward+** (new in Unity 6 URP): Suitable for scenes with many lights (>8 dynamic lights). Switch to Forward+ in the URP Asset's Rendering Path to bypass the legacy Forward lighting limit.
- **LOD**: All objects exceeding 500 tris must have a `LODGroup` with at least 3 LOD levels.
- **Occlusion Culling**: Scenes must have baked Occlusion Data. Mark large opaque objects with `Occluder Static`.
- **Shadow**: Configure different Shadow Distance and Cascade counts per platform in each URP Asset (Mobile ≤ 1 cascade, PC High ≤ 4 cascades).
- **Adaptive Probe Volumes (APV)**: The recommended global illumination solution for Unity 6 URP, replacing the legacy Light Probe Group. Supports streaming and memory budget control.
- **Zero GC Allocation Principle**: Per-frame `new` allocations are prohibited. Use `ObjectPool<T>`, `NativeArray<T>`, or pre-allocated buffers.

---

## 5. Debugging Tool Workflow

| Tool | Purpose | Platform |
|------|---------|----------|
| **Frame Debugger** (`Window > Analysis > Frame Debugger`) | Per-pass Draw Call tracking, Render Graph resource flow | All Platforms |
| **RenderDoc** | GPU-level Shader debugging, texture content inspection | PC |
| **Unity Profiler** | CPU/GPU timeline analysis, GC detection | All Platforms |
| **Xcode Instruments / GPU Frame Capture** | iOS Metal-level analysis | iOS |
| **Android GPU Inspector** | Android Vulkan / GLES analysis | Android |

Render Graph passes appear as individual nodes in the Frame Debugger, allowing direct observation of resource `Create / Read / Write / Release` lifecycles.

---

## 6. Reference Documentation

- **Unity 6 Manual**: `https://docs.unity3d.com/6000.3/Documentation/Manual/`
- **Unity 6 Scripting API**: `https://docs.unity3d.com/6000.3/Documentation/ScriptReference/{ClassName}.html`
- **URP Package Documentation**: `https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@17.0/`
- **Coding Standard Documents**: See `Unity_C_Sharp_Style_Guide.md` and `Unity_Cowork_and_Coding_Standard.md` in the `references/` directory
- **Enforcement**: API reference URLs must use version `6000.3` or above. Using APIs deprecated before 6000.3 is strictly prohibited. If a feature is deprecated, the currently recommended alternative must be used.

---

## 7. Chain of Thought

Before answering, perform the following self-checks in order:

1. **Version Check**: Does this feature have a dedicated Render Graph simplified path in Unity 6000.3 (e.g., `UniversalRenderPipelineUtils`)? Are any deprecated APIs being used?
2. **Render Graph Resource Check**: Is `TextureHandle` correctly passed via `builder.UseTexture` / `SetGlobalTextureAfterPass`? Is there any mixing of `RenderTexture` and `TextureHandle`?
3. **Cross-Platform Shader Validation**: Are there branches for `SHADER_API_MOBILE`? Is `discard` used on opaque Mobile objects? Are UVs kept as `float2`? Are color/lighting calculations using `half`?
4. **HLSL Compiler Pitfalls**: Are there early `return` statements in loops that could trigger the FXC "potentially uninitialized variable" warning?
5. **Performance Budget Verification**: Do Draw Call / tri counts meet the target platform budget? Are GC allocations zero?
6. **API Source Verification**: Consult `https://docs.unity3d.com/6000.3/Documentation/ScriptReference/{ClassName}.html` to confirm method signatures are correct.

---

> **Compatibility**: Unity 6000.3.x (6.3) · Universal Render Pipeline (URP)
> **License**: All Rights Reserved. Copyright (c) 2026 [Feynman]. This work may not be used, copied, modified, or distributed in any form without explicit written permission.
