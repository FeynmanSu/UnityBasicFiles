---
paths:
  - "Assets/Shaders/**/*.hlsl"
  - "Assets/Shaders/**/*.shader"
---

# URP HLSL / Shader Coding Rules

When editing Shader or HLSL files, the following rules are **automatically applied** without additional prompting.

## Required Include
```hlsl
#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"
```

## SRP Batcher Compliance
- All material properties **must** be declared within `CBUFFER_START(UnityPerMaterial) ... CBUFFER_END`
- Do not declare material properties outside the `CBUFFER`
- Include standard variables like `_BaseMap_ST` to ensure batching

## Textures and Sampling
- Use `TEXTURE2D` / `SAMPLER` macros. Using `sampler2D` is prohibited
- Mobile texture samples per pass ≤ 4, PC ≤ 8

## Blit / Post-Processing
- Post-processing blit must use `Blitter.BlitCameraTexture()`. Using `Graphics.Blit()` is prohibited

## Platform Precision
- Mobile color and lighting calculations: use `half`
- UV coordinates must remain `float2`. **Do not** downgrade to `half2` (causes precision artifacts)
- PC / Console may use `float` throughout

## Platform Branching
- Cross-platform logic uses `#if SHADER_API_MOBILE` branches
- Use `#pragma multi_compile` to define feature variants, and strip unnecessary combinations via URP Asset Shader Stripping
- Do not exceed 4 dynamic branches. Prefer `static const` lookup tables for complex logic

## Mobile Restrictions
- Using `discard` / `clip()` on Mobile opaque objects is prohibited (breaks tile-based GPU Early-Z)

## HLSL Compiler Pitfalls
- Avoid early `return` in loops (FXC reports "potentially uninitialized variable")
- Use mask multiplication to control output instead: `result *= saturate(condition * 1e6)`
- Use macro expansion to generate separate functions per quality tier. Do not use conditional early returns within functions

## Render Graph Integration
- To pass RG-allocated `TextureHandle` to Shaders: use `builder.SetGlobalTextureAfterPass(handle, id)` within the pass's `using` block
- `Shader.SetGlobalTexture(id, textureHandle)` is prohibited (TextureHandle ≠ Texture, fails silently)
- Persistent RTs (e.g., temporal history) use regular `RenderTexture` and can be set directly via `Shader.SetGlobalTexture(id, rt)`
