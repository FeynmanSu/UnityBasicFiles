---
description:
alwaysApply: true
---

# Claude Unity 6.3 URP Cross-Platform Game Development Guidelines

## Project Core Context
- **Engine Version**: Unity 6.3 (6000.3.x+)
- **Render Pipeline**: Universal Render Pipeline (URP)
- **Rendering Technology**: **Render Graph** and **GPU Resident Drawer** enabled by default
- **Scripting Language**: C# 12 / .NET 8, using Unity 6.3+ exclusive APIs
- **Target Platforms**: PC (Windows/Mac/Linux), Mobile (Android/iOS), Console (optional)
- **Key Paths**:
    - Scripts: `Assets/Scripts/`
    - Shaders: `Assets/Shaders/`
    - Render Settings: `Assets/Settings/`
    - Prefabs: `Assets/Prefabs/`
    - Art Assets: `Assets/Art/`

---

## Technical Specifications (Unity 6 Specific)

### Render Graph
- Custom passes must implement the `IRenderGraphRecorder` interface (`RecordRenderGraph` method). Using the legacy `ScriptableRenderPass.Execute` is **prohibited**.
- Pass-to-pass resource transfer uses `TextureHandle`. Persistent RTs (e.g., temporal history) are managed by `ScriptableRendererFeature` directly, not allocated through Render Graph.
- When a Render Graph-allocated `TextureHandle` needs to be passed to a Shader as a global texture, call `builder.SetGlobalTextureAfterPass(handle, id)` within the pass's `using` block. **Do not** use `Shader.SetGlobalTexture(id, textureHandle)` (fails silently).
- Use `builder.UseTexture()` / `builder.UseTextureFragment()` to declare read/write dependencies, letting Render Graph handle resource barriers automatically.

### Shader Authoring
- Use HLSL only. Must `#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"`.
- All material properties must be wrapped in `CBUFFER_START(UnityPerMaterial) ... CBUFFER_END` to support SRP Batcher.
- Use `TEXTURE2D` / `SAMPLER` macros. Do not use `sampler2D`.
- Post-processing blit must use `Blitter.BlitCameraTexture()` API. Do not use `Graphics.Blit()`.
- Avoid early `return` in HLSL (FXC may report "potentially uninitialized variable"). Use mask multiplication to control output instead.

### Cross-Platform Shader Compatibility
- Use `#pragma multi_compile` to define platform branches, and strip unnecessary variants via Shader Stripping in the URP Asset.
- Avoid more than 4 dynamic branches. Prefer `static const` lookup tables or per-quality macro expansion for complex logic.
- Texture sample count: Mobile ≤ 4 per pass, PC ≤ 8 per pass.
- Do not use `discard` / `clip()` on opaque objects for Mobile (causes performance degradation on tile-based GPUs).

### Input System
- Use **Input System Package (New)** and define `InputActionAsset`. Using the legacy `Input.GetKeyDown` is **prohibited**.
- Cross-platform input mapping: PC uses keyboard/mouse/gamepad, Mobile uses `Touchscreen`, switching via Action Maps. Do not write `#if UNITY_ANDROID` branches.

---

## Cross-Platform Performance Specifications

### Target Performance Budget

| Platform  | Target FPS | Draw Calls | Tri/Frame | Verts/Object |
|-----------|-----------|-----------|-----------|------------|
| PC High   | 60 fps    | ≤ 500     | ≤ 2M      | Unlimited  |
| PC Low    | 60 fps    | ≤ 200     | ≤ 500K    | ≤ 50K     |
| Android   | 30 fps    | ≤ 100     | ≤ 200K    | ≤ 10K     |
| iOS       | 60 fps    | ≤ 150     | ≤ 300K    | ≤ 15K     |

### Rendering Optimization Principles
- **GPU Resident Drawer**: Must be enabled for static objects (scene geometry, buildings). Enable for dynamic objects as needed.
- **LOD**: All objects exceeding 500 tris must have a `LODGroup` with at least 3 LOD levels (100%/30%/10%).
- **Occlusion Culling**: Scenes must have baked Occlusion Data. Mark large opaque objects with `Occluder Static`.
- **Batching**: Use `Static Batching` for static objects. Use `GPU Instancing` for small dynamic objects (add `#pragma instancing_options` to Shader).
- **Shadow**: Shadow Distance is configured per platform in tiered URP Assets. Mobile must not use more than 1 real-time shadow light.

### Memory and GC Management
- Per-frame `new` allocations are prohibited (List, array, string concat). Use `ObjectPool<T>` or pre-allocate.
- Use `StringBuilder` or string interpolation for string formatting. Avoid `string.Format` in Update.
- Use `NativeArray<T>` / `NativeList<T>` for large data structures (Burst-compatible). Must call `Dispose()` properly.
- `UnityEngine.Object` null comparisons are expensive. After caching, use C# `is null`. Only use `== null` when Unity lifecycle checks are needed.

---

## Code Style Guidelines

### Naming Conventions
| Type                          | Convention        | Example                  |
|-------------------------------|------------------|--------------------------|
| Classes, Methods, Properties  | `PascalCase`     | `TowerController`        |
| Private Fields                | `m_camelCase`    | `m_currentHealth`        |
| Serialized Fields             | `[SerializeField] private` + `m_camelCase` | `m_moveSpeed` |
| Constants                     | `k_PascalCase`   | `k_MaxTowerCount`        |
| Interfaces                    | `I` prefix       | `IDamageable`            |
| Shader Properties             | `_PascalCase`    | `_BaseColor`             |

### Structural Guidelines
- Prefer `async/await` (`UniTask` or `.NET Task`). Avoid `Coroutine` (GC overhead, difficult to debug).
- Manager classes use the singleton pattern via `[RuntimeInitializeOnLoadMethod]` or Scene injection. **Do not use** `DontDestroyOnLoad` Prefab Singleton (difficult to test).
- Separate data from logic: use `[Serializable] struct` or `ScriptableObject` for pure data; behavior logic belongs in MonoBehaviour/System.
- Mark classes not intended for inheritance with `sealed` to enable JIT devirtualization.

### Prohibited Patterns
- Do not call `GetComponent<T>()`, `FindObjectOfType<T>()`, or `GameObject.Find()` in `Update()` / `FixedUpdate()`.
- Do not declare material properties outside the Shader's `CBUFFER`.
- Do not use `Resources.Load()` (use Addressables instead).
- Do not use `Camera.main` (calls `FindObjectOfType` internally; cache the reference or use a `[CameraTag]` approach).

---

## Asset Pipeline Guidelines

### Asset Loading
- Use the **Addressable Asset System** for all runtime asset loading. Do not use `Resources/`.
- Async loading: `Addressables.LoadAssetAsync<T>()`, used with `await`.
- Asset release: explicitly call `Addressables.Release()` to avoid memory leaks.

### Texture Specifications

| Usage     | Format (Android) | Format (iOS) | Format (PC) | Max Size  |
|-----------|-----------------|-------------|------------|-----------|
| Albedo    | ASTC 6x6        | ASTC 6x6    | DXT5       | 2048      |
| Normal    | ASTC 6x6        | ASTC 6x6    | BC5        | 2048      |
| UI Atlas  | ASTC 4x4        | ASTC 4x4    | DXT5       | 4096      |
| HDR / RT  | RGBAHalf        | RGBAHalf    | RGBAHalf   | As needed |

- Enable Mipmaps (except for UI textures).
- Use **Texture Streaming** (`streamingMipmaps = true`) for large scene textures.

### Audio
- Background music: `Streaming`, `Vorbis`, Mono.
- Sound effects: `Decompress on Load` (short clips) / `Compressed in Memory` (long clips), `ADPCM`.

---

## URP Asset Tiered Configuration

The project must maintain three URP Assets (located in `Assets/Settings/`):

| Asset Name               | Purpose                     | Key Settings                                     |
|--------------------------|-----------------------------|--------------------------------------------------|
| `URP_PC_High.asset`     | PC High Quality             | Forward+, SSAO, Shadows 150m, 4 shadow cascades |
| `URP_PC_Low.asset`      | PC Low-end / Integrated GPU | Forward, No SSAO, Shadows 50m, 2 cascades       |
| `URP_Mobile.asset`      | Android / iOS               | Forward, No SSAO, Shadows 30m, 1 cascade, HDR off |

Switch the corresponding URP Asset via the `QualitySettings` API or Quality Levels.

---

## Response Principles
1. **Reject Deprecated Syntax**: Never provide `ScriptableRenderPass.Execute`, `Camera.main` global lookup, `Resources.Load`, or `Coroutine` as primary solutions in new code.
2. **Terminology Consistency**: Maintain English technical terms (Render Graph, TextureHandle, URP Asset, etc.) and use English for all explanations.
3. **Platform Awareness**: When providing code, if behavior differs across platforms, always note platform-specific differences and recommended settings.
4. **Performance First**: All recommendations must consider Mobile performance baseline first, then scale up to PC/Console.
5. **No API Guessing**: If unsure whether a specific Unity 6.3 API exists, explicitly state that official documentation should be consulted. Do not fabricate method signatures.

---

## Common Commands
- **Build**: Triggered via `UnityEditor.BuildPlayerWindow`, or via CLI `Unity.exe -batchmode -buildTarget <platform>`.
- **Test**: `Unity Test Framework`. `EditMode` tests for pure logic, `PlayMode` tests for scene interactions.
- **Profiling**: Use **Unity Profiler** + **Frame Debugger** + **RenderDoc** (PC) / **Xcode Instruments** (iOS) / **Android GPU Inspector** (Android).
- **Addressables Build**: `AddressableAssetSettings.BuildPlayerContent()` must be executed before the Player Build.
