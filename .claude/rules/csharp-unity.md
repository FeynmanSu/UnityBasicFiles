---
paths:
  - "Assets/Scripts/**/*.cs"
---

# Unity C# 編寫規則

編輯 Unity C# 腳本時，**自動套用**以下規則，無需額外提示。

## 命名
- 私有欄位：`m_camelCase`（成員）、`s_camelCase`（靜態）、`k_PascalCase`（常數）
- 方法 / 類別 / 屬性：`PascalCase`
- 布林值前綴動詞：`isDead`、`isWalking`、`hasDamageMultiplier`
- 介面前綴 `I`：`IDamageable`
- Shader property：`_PascalCase`

## 禁止模式
- 禁止在 `Update()` / `FixedUpdate()` 中呼叫 `GetComponent<T>()`、`FindObjectOfType<T>()`、`GameObject.Find()`
- 禁止使用 `Camera.main`（每次呼叫會 `FindObjectOfType`，請快取引用）
- 禁止使用 `Resources.Load()`（改用 Addressables）
- 禁止使用 `Coroutine`（改用 `async/await` 與 UniTask）
- 禁止每幀 `new` 配置（List、array、string concat），改用 `ObjectPool<T>` 或預先分配
- 禁止使用舊版 `Input.GetKeyDown`（改用 Input System Package）

## 結構
- Manager 類別使用 `[RuntimeInitializeOnLoadMethod]` 或 Scene 注入，不使用 `DontDestroyOnLoad` Prefab Singleton
- 資料與邏輯分離：純資料用 `[Serializable] struct` 或 `ScriptableObject`
- 不打算繼承的類別加上 `sealed`
- 序列化欄位：`[SerializeField] private` + `_camelCase`，`[SerializeField]` 與 `[Range]` 合併寫：`[SerializeField, Range(0f, 1f)]`

## 非同步
- 優先 `async/await`（UniTask 或 .NET Task）
- 資源載入：`await Addressables.LoadAssetAsync<T>(key)`
- 資源釋放：明確呼叫 `Addressables.Release(handle)`

## 效能
- `NativeArray<T>` / `NativeList<T>` 使用完必須 `Dispose()`
- `UnityEngine.Object` null 比較用 C# `is null`，只在需 Unity 生命週期判斷時用 `== null`
- 字串格式化用 `StringBuilder` 或插值字串，不在 Update 中用 `string.Format`
