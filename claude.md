---
description:
alwaysApply: true
---

# Claude Unity 6.3 URP 跨平台遊戲開發規範

## 專案核心上下文
- **引擎版本**: Unity 6.3 (6000.3.x+)
- **渲染管線**: Universal Render Pipeline (URP)
- **渲染技術**: 預設啟用 **Render Graph** 與 **GPU Resident Drawer**
- **腳本語言**: C# 12 / .NET 8，使用 Unity 6.3+ 專屬 API
- **目標平台**: PC (Windows/Mac/Linux)、Mobile (Android/iOS)、Console (可選)
- **關鍵路徑**:
    - 腳本: `Assets/Scripts/`
    - 著色器: `Assets/Shaders/`
    - 渲染設置: `Assets/Settings/`
    - 預製件: `Assets/Prefabs/`
    - 資源: `Assets/Art/`

---

## 技術規範 (Unity 6 特化)

### Render Graph
- 自定義 Pass 必須實作 `IRenderGraphRecorder` 介面（`RecordRenderGraph` 方法），**禁止**使用舊版 `ScriptableRenderPass.Execute`。
- Pass 間資源傳遞使用 `TextureHandle`；持久化 RT（如 temporal history）由 `ScriptableRendererFeature` 自行管理，不透過 RG 分配。
- RG 分配的 `TextureHandle` 若需作為 global texture 傳給 Shader，必須在 pass 的 `using` 區塊內呼叫 `builder.SetGlobalTextureAfterPass(handle, id)`，**不可**用 `Shader.SetGlobalTexture(id, textureHandle)`（會靜默失敗）。
- 使用 `builder.UseTexture()` / `builder.UseTextureFragment()` 宣告讀寫依賴，讓 RG 自動處理資源屏障。

### Shader 撰寫
- 僅使用 HLSL。必須 `#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"`。
- 所有 Material 屬性必須包裹在 `CBUFFER_START(UnityPerMaterial) ... CBUFFER_END` 中以支援 SRP Batcher。
- 使用 `TEXTURE2D` / `SAMPLER` 巨集，不使用 `sampler2D`。
- 後製 Blit 使用 `Blitter.BlitCameraTexture()` API，不使用 `Graphics.Blit()`。
- HLSL 中避免提前 `return`（FXC 可能報「potentially uninitialized variable」），改用乘以遮罩的方式控制輸出。

### 跨平台 Shader 相容性
- 使用 `#pragma multi_compile` 定義平台分支，並在 URP Asset 的 Shader Stripping 中剔除不需要的變體。
- 避免超過 4 個 dynamic branch；複雜邏輯優先用 `static const` 查表或 per-quality macro 展開。
- 貼圖取樣數量：Mobile ≤ 4 張/Pass，PC ≤ 8 張/Pass。
- 不使用 `discard` / `clip()` 於 Mobile 的不透明物件（tile-based GPU 性能劣化）。

### 輸入系統
- 使用 **Input System Package (New)**，定義 `InputActionAsset`，**禁止**使用舊版 `Input.GetKeyDown`。
- 跨平台輸入映射：PC 用鍵鼠/Gamepad，Mobile 用 `Touchscreen`，透過 Action Map 切換，不寫 `#if UNITY_ANDROID` 分支。

---

## 跨平台性能規範

### 目標性能預算

| 平台        | 目標幀率 | Draw Call | Tri/Frame  | 頂點/物件  |
|-----------|---------|-----------|------------|----------|
| PC High   | 60 fps  | ≤ 500     | ≤ 2M       | 不限      |
| PC Low    | 60 fps  | ≤ 200     | ≤ 500K     | ≤ 50K    |
| Android   | 30 fps  | ≤ 100     | ≤ 200K     | ≤ 10K    |
| iOS       | 60 fps  | ≤ 150     | ≤ 300K     | ≤ 15K    |

### 渲染優化原則
- **GPU Resident Drawer**: 靜態物件（場景幾何、建築）必須啟用，動態物件視情況開啟。
- **LOD**: 所有超過 500 tri 的物件必須設定 `LODGroup`，至少 3 個 LOD 級別（100%/30%/10%）。
- **Occlusion Culling**: 場景必須烘焙 Occlusion Data，使用 `Occluder Static` 標記大型不透明物件。
- **Batching**: 靜態物件用 `Static Batching`；動態小物件用 `GPU Instancing`（Shader 加 `#pragma instancing_options`）。
- **Shadow**: 陰影 Distance 依平台在 URP Asset 分層設定；Mobile 不使用超過 1 個即時陰影燈光。

### 記憶體與 GC 管理
- 每幀禁止 `new` 配置（List、array、string concat）；使用 `ObjectPool<T>` 或預先分配。
- 字串格式化使用 `StringBuilder` 或插值字串，避免 `string.Format` 於 Update 中。
- 大型資料結構使用 `NativeArray<T>` / `NativeList<T>`（Burst 相容），需正確 `Dispose()`。
- `UnityEngine.Object` 的 Null 比較很耗效能，快取後用 C# `is null` 判斷；只在需要 Unity 生命週期時用 `== null`。

---

## 代碼風格規範

### 命名規範
| 類型               | 規範             | 範例                       |
|------------------|-----------------|--------------------------|
| 類別、方法、屬性       | `PascalCase`    | `TowerController`        |
| 私有欄位             | `_camelCase`    | `_currentHealth`         |
| 序列化欄位            | `[SerializeField] private` + `_camelCase` | `_moveSpeed` |
| 常數               | `UPPER_SNAKE`   | `MAX_TOWER_COUNT`        |
| 介面               | `I` 前綴         | `IDamageable`            |
| Shader property  | `_PascalCase`   | `_BaseColor`             |

### 結構規範
- 優先使用 `async/await` (`UniTask` 或 `.NET Task`)，避免 `Coroutine`（GC、Debug 困難）。
- Manager 類別使用單例模式，以 `[RuntimeInitializeOnLoadMethod]` 或 Scene 注入，**不使用** `DontDestroyOnLoad` 的 Prefab Singleton（難以測試）。
- 資料與邏輯分離：純資料用 `[Serializable] struct` 或 `ScriptableObject`；行為邏輯在 MonoBehaviour/System 中。
- 使用 `sealed` 修飾不打算繼承的類別，利於 JIT 去虛化。

### 禁止模式
- 禁止在 `Update()`/`FixedUpdate()` 中呼叫 `GetComponent<T>()`、`FindObjectOfType<T>()`、`GameObject.Find()`。
- 禁止在 Shader 的 `CBUFFER` 外宣告 Material 屬性。
- 禁止使用 `Resources.Load()`（改用 Addressables）。
- 禁止使用 `Camera.main`（每次呼叫會 `FindObjectOfType`；快取引用或用 `[CameraTag]` 方式）。

---

## 資源管線規範

### 資源載入
- 使用 **Addressable Asset System** 進行所有執行時資源載入，不使用 `Resources/`。
- 非同步載入：`Addressables.LoadAssetAsync<T>()`，配合 `await` 使用。
- 資源釋放：明確呼叫 `Addressables.Release()`，避免記憶體洩漏。

### 貼圖規範

| 用途        | Format (Android) | Format (iOS) | Format (PC) | Max Size   |
|-----------|-----------------|-------------|------------|-----------|
| Albedo    | ASTC 6x6        | ASTC 6x6    | DXT5       | 2048      |
| Normal    | ASTC 6x6        | ASTC 6x6    | BC5        | 2048      |
| UI Atlas  | ASTC 4x4        | ASTC 4x4    | DXT5       | 4096      |
| HDR / RT  | RGBAHalf        | RGBAHalf    | RGBAHalf   | 依需求     |

- 啟用 Mipmap（UI 貼圖除外）。
- 大型場景貼圖使用 **Texture Streaming**（`streamingMipmaps = true`）。

### Audio
- 背景音樂：`Streaming`，`Vorbis`，Mono。
- 音效：`Decompress on Load`（短音效） / `Compressed in Memory`（長音效），`ADPCM`。

---

## URP Asset 分層設定

專案必須維護三份 URP Asset（放於 `Assets/Settings/`）：

| Asset 名稱               | 用途               | 關鍵設定                              |
|-------------------------|------------------|-------------------------------------|
| `URP_PC_High.asset`     | PC 高品質           | Forward+、SSAO、Shadows 150m、4 shadow cascades |
| `URP_PC_Low.asset`      | PC 低配/整合顯卡      | Forward、無 SSAO、Shadows 50m、2 cascades |
| `URP_Mobile.asset`      | Android / iOS     | Forward、無 SSAO、Shadows 30m、1 cascade、HDR off |

透過 `QualitySettings` API 或 Quality Level 切換對應 URP Asset。

---

## 回答原則
1. **拒絕過時語法**: 嚴禁提供 `ScriptableRenderPass.Execute`、`Camera.main` 全域查找、`Resources.Load`、`Coroutine` 作為新程式碼的主要方案。
2. **術語一致性**: 保持英文技術名詞（Render Graph、TextureHandle、URP Asset…），但使用繁體中文解釋邏輯。
3. **平台意識**: 提供程式碼時，若行為因平台而異，必須說明各平台的差異與建議設定。
4. **性能優先**: 所有建議預設考量 Mobile 效能底線，再向上擴充至 PC/Console。
5. **不猜測 API**: 若不確定特定 Unity 6.3 API 是否存在，明確說明需查閱官方文檔，不捏造方法簽名。

---

## 常規指令
- **Build**: `UnityEditor.BuildPlayerWindow` 觸發，或透過 CLI `Unity.exe -batchmode -buildTarget <platform>`。
- **Test**: `Unity Test Framework`，`EditMode` 測試純邏輯，`PlayMode` 測試場景互動。
- **Profiling**: 使用 **Unity Profiler** + **Frame Debugger** + **RenderDoc**（PC）/ **Xcode Instruments**（iOS）/ **Android GPU Inspector**（Android）。
- **Addressables Build**: `AddressableAssetSettings.BuildPlayerContent()` 需在 Player Build 前執行。
