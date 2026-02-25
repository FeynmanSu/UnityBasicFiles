---
paths:
  - "Assets/Scripts/**/*.cs"
---

# Unity C# Coding Rules

When editing Unity C# scripts, the following rules are **automatically applied** without additional prompting.

## Naming
- Private fields: `m_camelCase` (member), `s_camelCase` (static), `k_PascalCase` (constant)
- Methods / Classes / Properties: `PascalCase`
- Booleans prefixed with verbs: `isDead`, `isWalking`, `hasDamageMultiplier`
- Interfaces prefixed with `I`: `IDamageable`
- Shader properties: `_PascalCase`

## Prohibited Patterns
- Do not call `GetComponent<T>()`, `FindObjectOfType<T>()`, or `GameObject.Find()` in `Update()` / `FixedUpdate()`
- Do not use `Camera.main` (calls `FindObjectOfType` internally; cache the reference)
- Do not use `Resources.Load()` (use Addressables instead)
- Do not use `Coroutine` (use `async/await` with UniTask instead)
- Do not use per-frame `new` allocations (List, array, string concat); use `ObjectPool<T>` or pre-allocate
- Do not use the legacy `Input.GetKeyDown` (use Input System Package instead)

## Structure
- Manager classes use `[RuntimeInitializeOnLoadMethod]` or Scene injection. Do not use `DontDestroyOnLoad` Prefab Singleton
- Separate data from logic: use `[Serializable] struct` or `ScriptableObject` for pure data
- Mark classes not intended for inheritance with `sealed`
- Serialized fields: `[SerializeField] private` + `m_camelCase`. Combine `[SerializeField]` with `[Range]`: `[SerializeField, Range(0f, 1f)]`

## Async
- Prefer `async/await` (UniTask or .NET Task)
- Asset loading: `await Addressables.LoadAssetAsync<T>(key)`
- Asset release: explicitly call `Addressables.Release(handle)`

## Performance
- `NativeArray<T>` / `NativeList<T>` must be `Dispose()`d after use
- For `UnityEngine.Object` null checks, use C# `is null`. Only use `== null` when Unity lifecycle checks are needed
- Use `StringBuilder` or string interpolation for string formatting. Do not use `string.Format` in Update
