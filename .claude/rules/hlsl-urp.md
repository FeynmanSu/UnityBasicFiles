---
paths:
  - "Assets/Shaders/**/*.hlsl"
  - "Assets/Shaders/**/*.shader"
---

# URP HLSL / Shader 編寫規則

編輯 Shader 或 HLSL 檔案時，**自動套用**以下規則，無需額外提示。

## 必要引用
```hlsl
#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"
```

## SRP Batcher 合規
- 所有 Material 屬性**必須**在 `CBUFFER_START(UnityPerMaterial) ... CBUFFER_END` 中宣告
- 禁止在 `CBUFFER` 外宣告 Material 屬性
- 包含 `_BaseMap_ST` 等標準變量以確保合批

## 貼圖與取樣
- 使用 `TEXTURE2D` / `SAMPLER` 巨集，禁止使用 `sampler2D`
- Mobile 每 Pass 取樣貼圖數 ≤ 4 張，PC ≤ 8 張

## Blit / 後製
- 後製 Blit 使用 `Blitter.BlitCameraTexture()`，禁止使用 `Graphics.Blit()`

## 平台精度
- Mobile 顏色與光照計算：使用 `half`
- UV 座標保持 `float2`，**禁止**降為 `half2`（精度失真）
- PC / Console 可全部使用 `float`

## 平台分支
- 跨平台邏輯透過 `#if SHADER_API_MOBILE` 分支
- 使用 `#pragma multi_compile` 定義功能變體，並在 URP Asset Shader Stripping 中剔除不需要的組合
- 禁止超過 4 個 dynamic branch，複雜邏輯優先用 `static const` 查表

## Mobile 限制
- 禁止在 Mobile 不透明物件中使用 `discard` / `clip()`（破壞 tile-based GPU Early-Z）

## HLSL 編譯器陷阱
- 迴圈中避免提前 `return`（FXC 報「potentially uninitialized variable」）
- 改用遮罩方式控制輸出：`result *= saturate(condition * 1e6)`
- 對各品質層級使用 macro 展開獨立函數，不在函數內用條件提前返回

## Render Graph 整合
- RG 分配的 `TextureHandle` 傳給 Shader：在 pass `using` 區塊內使用 `builder.SetGlobalTextureAfterPass(handle, id)`
- 禁止 `Shader.SetGlobalTexture(id, textureHandle)`（TextureHandle ≠ Texture，靜默失敗）
- 持久化 RT（如 temporal history）使用普通 `RenderTexture`，可直接 `Shader.SetGlobalTexture(id, rt)`
