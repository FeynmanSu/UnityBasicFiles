# Unity C# 風格指南 (Unity C# Style Guide)
專案風格指南 。這份指南並非死板的規則手冊，若有任何建議，都可以提出建議並討論。

> **最重要的原則：保持一致性。** 請盡量遵循此指南的建議並持續實行。

---
## 核心原則 (Key Principles)
  * **易讀性勝過簡短**：清晰度比省略幾個母音所節省的時間更重要 。
  * **明確一致的目標**：目標是讓程式碼更易讀、易維護且有一致性。
  * **從頭做起**：從一開始就正確命名，可以節省往後除錯與功能擴充的時間 。
  * **務實至上**：雖然應盡量遵循業界標準，但也要考量團隊規模。100 人的團隊與 2 人的團隊需求是不同的 。
  * **動態更新**：風格標準是一份動態文件，應隨專案演進與團隊成長而更新 。
  * **先學後破**：作為初學者，在打破規則前先學會規則。初學者應保持指南精簡，最重要的是先寫出能運行的程式碼，再考慮「整潔」。
    * 守破離精神
  * **自然發音**：偏好能自然發音且符合英文閱讀習慣的名稱。例如：`HorizontalAlignment` 優於 `AlignmentHorizontal` 。

---
## 命名規範 (Naming)
  * **變數命名**：應具備描述性、清晰且無歧義。除了布林值外，應使用**名詞**命名 。
  * **布林值 (Booleans)**：名稱應以**動詞**開頭（如 `isDead`, `isWalking`, `hasDamageMultiplier`），使其意義更明顯 。
  * **禁止縮寫**：除非是數學運算或通用的縮寫，否則變數名應揭示其意圖 。
  * **避免常用詞**：為了避免在命名空間或問題領域中產生歧義，應避開通用詞彙，或加上前綴/後綴（例如使用 `PhysicsSolver` 而非 `Solver`） 。

---
## 大小寫與前綴 (Casing and Prefixes)
  * **Pascal Case**：用於類別名、公共欄位、方法（如 `ExamplePlayerController`, `MaxHealth`）。
  * **Camel Case**：用於區域變數、私有變數與參數（如 `examplePlayerController`, `maxHealth`）。
  * **避免使用**：蛇形命名法 (snake_case)、短橫線命名法 (kebab-case) 或匈牙利命名法。
  * **成員變數前綴**：
    * 私有成員變數：`m_` 或底線 `_`。
    * 常數 (Constants)：`k_`。
    * 靜態變數 (Static)：`s_`。
  * **簡化初始化**：省略冗餘的初始化（如 `int` 默認為 `0`，引用類型默認為 `null`）。
  * **存取修飾符**：建議明確標記 `private` 以避免歧義，或者根據團隊共識統一省略。
  * **消除冗餘**：如果類別名為 `Player`，變數名應為 `Score` 而非 `PlayerScore`。

---
## 格式與間距 (Formatting & Spacing)
  * **大括號風格**：選擇 Allman (括號換行) 風格
    * 就算只有一行，也加上大括號，且括號換行
  * **行寬限制**：盡量保持每行程式碼簡短，但也不要過度換行。
  * **空白的使用**：
    * 在逗號之後加空白。
      * CollectItem(myObject, 0, 1);
    * 在 `if` 或 `while` 等流程控制條件前加上一個空格。
      * for (int i = 0; i < 100; i++)
    * 括號內不留空格，函式名與括號間不留空格。
      * DropPowerUp(myPrefab, 0, 1);
  * **變數聲明**：多數情況下每行僅聲明一個變數，以提高易讀性。

---
## 註解與組織 (Comments & Organization)
  * **為何而寫**：註解應說明「為什麼 (Why)」，而非僅僅是「什麼 (What)」或「如何做 (How)」。如果程式碼過於複雜需要註解說明，應先考慮重構 。
  * **自帶說明**：優先使用良好的命名來取代註解 。
  * **Inspector 工具**：對於序列化欄位，使用 `[Tooltip]` 代替註解，以便在 Unity 編輯器中查看 。
  * **類別排序**：建議順序為：欄位 (Fields) -> 屬性 (Properties) -> 事件 (Events) -> MonoBehaviour 方法 (Awake, Start 等) -> 公共方法 -> 私有方法 -> 巢狀類別 。
  * **禁止事項**：避免使用 `#region`（這通常代表類別過於龐大） ，以及避免標註作者（應使用版本控制系統追蹤變更） 。

---
## 範例程式碼 (Code Example)
```csharp
// 移除沒有使用到的using
using UnityEngine;
using System;

// 盡量使用namespace來限制你的class, interfacem, enums等的存取範圍
// 命名空間：使用 Pascal 命名法，盡量反映出資料夾結構
namespace UnityCSharpStyleSheetExample
{
    // 列舉：當物件一次只能有一個值時使用
    public enum Direction
    {
        North,
        South,
        East,
        West,
    }

    // 旗標列舉：當可以同時選擇多個值時使用，使用複數名詞
    [Flags]
    public enum AttackModes
    {
        // 十進位                         // 二進位
        None = 0,                          // 000000
        Melee = 1,                         // 000001
        Ranged = 2,                        // 000010
        Special = 4,                       // 000100
        MeleeAndSpecial = Melee | Special  // 000101
    }

    // 介面：以大寫 I 開頭，後接描述功能的形容詞詞組
    public interface IDamageable
    {
        string DamageTypeName { get; }
        float DamageValue { get; }
        bool ApplyDamage(string description, float damage, int numberOfHits);
    }

    // 類別：使用 Pascal 命名法，名詞為主
    public class StyleExample : MonoBehaviour
    {
        // 欄位：避免特殊字元，布林值以動詞開頭

        // 私有欄位建議使用 m_ 前綴以區分區域變數
        int m_elapsedTimeInSeconds;

        // 使用 [SerializeField] 在 Inspector 中顯示私有變數
        [SerializeField] bool m_isPlayerDead;

        // 靜態欄位使用 s_ 前綴
        static int s_sharedCount;

        // 常數使用 k_ 前綴
        const int k_maxCount = 100;

        // 使用 Range 屬性限制範圍，可限制 Inspector 中能輸入的範圍
        [SerializeField, Range(0f, 1f)] float m_rangedStat;

        // Tooltip 可同時作為註解與編輯器提示
        [Tooltip("這是給玩家的另一項統計數據。")]
        [SerializeField] float m_anotherStat;

        // 屬性：優先於公共欄位使用

        // 私有 backing field
        int m_maxHealth;

        // 唯讀屬性，使用運算式主體 (expression-bodied)
        public int MaxHealthReadOnly => m_maxHealth;

        // 自動實作屬性
        public string DescriptionName { get; set; } = "Fireball";

        // 事件：以動詞詞組命名。現在分詞代表「之前」，過去分詞代表「之後」
        public event Action OpeningDoor; // 開門中 (之前)
        public event Action DoorOpened;  // 門已開 (之後)

        // 引發事件的方法前綴加上 "On"
        public void OnDoorOpened()
        {
            DoorOpened?.Invoke();
        }

        // 方法：以動詞開頭。回傳布林值的方法應像提問一樣
        public void SetInitialPosition(float x, float y, float z)
        {
            transform.position = new Vector3(x, y, z);
        }

        public bool IsNewPosition(Vector3 newPosition)
        {
            return (transform.position == newPosition);
        }

        void FormatExamples(int someExpression)
        {
            // VAR：當類型在上下文中清晰時可以使用
            var powerUps = new List<PlayerStats>();
            
            // Switch 語句：取代超過3層以上的 if-else
            // 最後永遠加上 default (可透過IDE自行產生)
            switch (someExpression)
            {
                case 0:
                    break;
                case 1:
                    break;
                case 2:
                    break;
                default:
                    break;
            }

            // 括號：即使是單行語句要使用括號以利除錯
            for (int i = 0; i < 100; i++)
            {
                DoSomething(i);
            }
        }

        void DoSomething(int x)
        {
            // ..
        }
    }

    // 其他結構：用於分組資料
    [Serializable]
    public struct PlayerStats
    {
        public int MovementSpeed;
        public int HitPoints;
        public bool HasHealthPotion;
    }
}

```