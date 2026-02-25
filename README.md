# UnityBasicFiles

為 **Unity 6.3 (6000.3.x+) URP** 跨平台遊戲開發所設計的基底儲存庫（Base Repository），內含統一的程式碼規範、Claude Code 自動套用規則，以及專為 Render Graph、HLSL 著色器最佳化與效能調校而設計的 AI 輔助技能。

將這些檔案放入 Unity 專案根目錄，即可讓團隊與 Claude Code 共享一致的開發標準。

---

## 這個儲存庫的目的

在商業級遊戲開發中，隨著程式碼規模增長、功能規格反覆修訂、團隊成員更迭，如何維持程式碼品質與一致性是長期開發的核心挑戰。本儲存庫提供：

1. **統一的程式碼規範** — 讓所有 C# 腳本與 HLSL 著色器遵循相同的命名、結構與效能準則
2. **自動化規則執行** — 透過 Claude Code 的 Rules 機制，在編輯特定檔案時自動套用對應規則，無需手動提醒
3. **AI 輔助開發技能** — 提供專為 Unity 6.3 URP 設計的 Claude Code Skill，涵蓋 Render Graph 編寫、跨平台著色器最佳化與效能調校
4. **團隊知識共享** — 內附完整的 C# 風格指南與編碼標準參考文件，降低新成員上手門檻

---

## 目標技術規格

| 項目 | 規格 |
|------|------|
| 引擎版本 | Unity 6.3 (6000.3.x+) |
| 渲染管線 | Universal Render Pipeline (URP) |
| 渲染技術 | Render Graph + GPU Resident Drawer |
| 腳本語言 | C# 12 / .NET 8 |
| 目標平台 | PC (Windows/Mac/Linux)、Android、iOS、Console（可選） |

---

## 檔案結構

```
.
├── claude.md                       # 專案主規範（Claude Code 每次對話自動載入）
├── .gitignore                      # 針對 Unity 6.3 URP 跨平台開發的 gitignore
│
└── .claude/
    ├── settings.json               # Claude Code 權限設定
    ├── settings.local.json         # 本機權限覆寫（不提交至版控）
    │
    ├── rules/                      # 自動套用規則（依檔案路徑觸發）
    │   ├── csharp-unity.md         # 編輯 Assets/Scripts/**/*.cs 時自動套用
    │   └── hlsl-urp.md             # 編輯 Assets/Shaders/**/*.hlsl|.shader 時自動套用
    │
    └── skills/                     # Claude Code 技能
        └── unity-6-urp/
            ├── SKILL.md            # URP 開發專用技能定義
            └── references/         # 參考文件
                ├── Unity_C_Sharp_Style_Guide.md
                └── Unity_Cowork_and_Coding_Standard.md
```

---

## 各檔案說明

### claude.md（專案主規範）

每次 Claude Code 對話啟動時自動載入，定義整個專案的開發框架：

- **技術規範** — Render Graph、Shader 撰寫、跨平台相容性、輸入系統
- **效能預算** — 各平台的 Draw Call、三角面數、FPS 目標上限
- **代碼風格** — 命名規範、結構規範、禁止模式
- **資源管線** — Addressables 載入、貼圖格式規範、音訊設定
- **URP Asset 分層設定** — PC 高畫質 / PC 低配 / Mobile 三組設定

### 自動套用規則（Rules）

這些規則會根據編輯的檔案路徑**自動啟用**，無需手動觸發。

#### `csharp-unity.md` — C# 腳本規則
- **觸發路徑**：`Assets/Scripts/**/*.cs`
- **命名**：`m_camelCase`（私有成員）、`s_camelCase`（靜態）、`k_PascalCase`（常數）、`PascalCase`（公開）
- **禁止模式**：`GetComponent` 於 Update 中呼叫、`Camera.main`、`Resources.Load`、`Coroutine`、每幀 `new` 配置
- **必要模式**：`async/await` + UniTask、Addressables 資源載入、`sealed` 修飾非繼承類別

#### `hlsl-urp.md` — HLSL / Shader 規則
- **觸發路徑**：`Assets/Shaders/**/*.hlsl` 及 `*.shader`
- **SRP Batcher 合規**：Material 屬性必須在 `CBUFFER_START/CBUFFER_END` 內宣告
- **貼圖巨集**：僅使用 `TEXTURE2D` / `SAMPLER`，禁止 `sampler2D`
- **平台精度**：Mobile 使用 `half` 計算顏色與光照、UV 保持 `float2`
- **Mobile 限制**：禁止在不透明物件使用 `discard` / `clip()`
- **Render Graph 整合**：使用 `SetGlobalTextureAfterPass` 傳遞 TextureHandle

### Unity 6 URP 開發技能（Skill）

當使用者要求撰寫新功能、最佳化程式碼、編寫 Shader 或調校 URP 效能時啟動，提供：

- **Render Graph Pass 編寫模式** — `IRenderGraphRecorder`、`TextureHandle`、資源宣告
- **跨平台著色器最佳實踐** — 精度控制、平台分支、編譯器陷阱迴避
- **效能最佳化工作流程** — GPU Resident Drawer、Forward+、LOD、Occlusion Culling
- **偵錯工具指引** — Frame Debugger、RenderDoc、各平台專用分析工具
- **思考路徑驗證** — 版本檢查、Render Graph 資源檢查、跨平台校驗、API 溯源

### 參考文件（References）

兩份完整的風格指南，作為技能的上下文知識來源：

- **Unity C# 風格指南** — 命名規範、格式規範、註解規範、範例程式碼
- **Clean Coding and Coding Style Guide** — 團隊協作標準、SOLID/DRY/KISS 原則、Code Smell 檢測、UI Toolkit 命名規範 (BEM)

---

## 效能預算

| 平台 | 目標幀率 | Draw Call | 三角面數/幀 | 頂點/物件 |
|------|---------|-----------|-----------|---------|
| PC High | 60 fps | ≤ 500 | ≤ 2M | 不限 |
| PC Low | 60 fps | ≤ 200 | ≤ 500K | ≤ 50K |
| Android | 30 fps | ≤ 100 | ≤ 200K | ≤ 10K |
| iOS | 60 fps | ≤ 150 | ≤ 300K | ≤ 15K |

---

## 命名規範速查表

| 類型 | 規範 | 範例 |
|------|------|------|
| 類別、方法、屬性 | `PascalCase` | `TowerController` |
| 私有成員欄位 | `m_camelCase` | `m_currentHealth` |
| 序列化欄位 | `[SerializeField] private` + `m_camelCase` | `m_moveSpeed` |
| 常數 | `k_PascalCase` | `k_MaxTowerCount` |
| 靜態欄位 | `s_camelCase` | `s_instanceCount` |
| 介面 | `I` 前綴 | `IDamageable` |
| Shader 屬性 | `_PascalCase` | `_BaseColor` |

---

## 主要禁止模式

| 禁止使用 | 替代方案 |
|---------|---------|
| `GetComponent<T>()` 於 `Update()` / `FixedUpdate()` | 在 `Awake()` 或 `Start()` 中快取引用 |
| `Camera.main` | 快取攝影機引用或使用 `[CameraTag]` 方式 |
| `Resources.Load()` | `Addressables.LoadAssetAsync<T>()` |
| `Coroutine` | `async/await`（UniTask 或 .NET Task） |
| `Graphics.Blit()` | `Blitter.BlitCameraTexture()` |
| `ScriptableRenderPass.Execute` | `IRenderGraphRecorder.RecordRenderGraph` |
| 舊版 `Input.GetKeyDown` | Input System Package (`InputActionAsset`) |
| `sampler2D`（Shader 中） | `TEXTURE2D` / `SAMPLER` 巨集 |
| `discard` / `clip()`（Mobile 不透明物件） | 遮罩乘法控制輸出 |

---

## 使用方式

1. 將本儲存庫的所有檔案複製到你的 Unity 6.3 URP 專案根目錄
2. 使用 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 開啟專案
3. 開始開發 — 編輯 C# 或 Shader 檔案時規則會自動套用，URP 相關任務會啟動專用技能

---

## URP Asset 分層設定

專案須維護三份 URP Asset，放於 `Assets/Settings/`：

| Asset 名稱 | 用途 | 關鍵設定 |
|------------|------|---------|
| `URP_PC_High.asset` | PC 高畫質 | Forward+、SSAO、Shadows 150m、4 cascades |
| `URP_PC_Low.asset` | PC 低配 / 內顯 | Forward、無 SSAO、Shadows 50m、2 cascades |
| `URP_Mobile.asset` | Android / iOS | Forward、無 SSAO、Shadows 30m、1 cascade、HDR off |

透過 `QualitySettings` API 或 Quality Level 在執行時切換對應的 URP Asset。

---

## 專案關鍵路徑

| 用途 | 路徑 |
|------|------|
| 腳本 | `Assets/Scripts/` |
| 著色器 | `Assets/Shaders/` |
| 渲染設定 | `Assets/Settings/` |
| 預製件 | `Assets/Prefabs/` |
| 美術資源 | `Assets/Art/` |

---

## 授權

本儲存庫提供程式碼規範與設定檔範本，請依團隊需求自行調整使用。
