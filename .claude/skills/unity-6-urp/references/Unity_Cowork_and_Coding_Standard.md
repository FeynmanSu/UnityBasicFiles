# Clean Coding and Coding Style Guide
```
Any fool can write code that a computer can understand. 
Good programmers write code that humans can understand.
                - Martin Fowler, author of Refactoring
```

這份指南並非死板的規則手冊，若有任何建議，都應提出建議並讓團隊討論之。
> **最重要的原則：保持一致性。** 請盡量遵循此指南的建議並持續實行。

---
### 目的
設計並規定團隊使用的 Coding standard / Coding Style，並不是為了製造麻煩或者展現團隊風格。就算在 Vibe Coding 當道的現在，開發一款商業品質的軟體，也不會是幾天內幾行 code 就可以完成的。當程式碼數量逐漸增加，功能規格修修改改，人員來來去去後，開發者必然需接手他人程式碼。在這樣的情境下，如何能快速有效且輕易的閱讀和理解他人的程式碼，是長時開發專案維持品質的重要關鍵。  

**這份文件的內容應該隨著團隊的變動與成長不斷修訂，這裡的原則並非不可變，如何協調出團隊都能接受的習慣與形式並讓成員願意持續執行，才是最重要的準則。**

好的設計規範以及願意努力實踐的團隊，可以帶來多方面的好處 :  

1. **提升可讀性與可維護性 (Readability & Maintainability)**  
   在大型專案中，開發者常需要閱讀他人編寫的程式碼。統一的縮排、命名慣例與結構，能讓程式碼「看起來像是由一人撰寫」，可以有效減少閱讀與理解的時間。當需要修復 Bug 或加入新功能時，清晰的結構更能讓維護者快速上手。 
2. **促進團隊協作與知識共享 (Collaboration & Knowledge Sharing)**  
   降低新成員磨合期： 新人能透過規範快速了解專案邏輯，減少因風格不一產生的疑惑。
   優化 Code Review 流程： 有了規範，審查者可以專注於程式邏輯與架構，而非糾結於括號位置或空格等瑣碎細節。 
3. **提高安全性與減少錯誤 (Security & Error Prevention)**  
   許多規範的目的在強制執行最佳實踐，避免使用已知有風險的函式或寫法，從而降低安全性漏洞與效能問題等風險。 
4. **確保長期可擴展性 (Scalability)**  
   大型專案往往會持續數年以上。隨著程式碼庫（Codebase）增長，混亂的風格會導致技術債不斷累積。標準化能確保程式碼在規模擴大後仍具備良好架構，且易於進行自動化測試與持續整合。 
5. **減少認知負荷 (Cognitive Load)**  
   當專案都遵循相同規範時，工程師在不同程式檔案、任務以及專案間切換時，不需要因為不同的程式碼寫法，而有不必要的腦力消耗與多餘的認知時間花費。

---
### 原則 (Principles)

#### 核心原則 (Key Principles)
  * **易讀性勝過簡短**：清晰度比省略幾個音節所節省的時間更重要 。
  * **明確一致的目標**：目標是讓程式碼更易閱讀、維護且保有一致性。
  * **從頭做起**：從一開始就正確命名，可以節省往後除錯與功能擴充的時間 。
  * **務實至上**：雖然應盡量遵循業界標準，但也要考量團隊規模。2 人的團隊與 100 人的團隊需求是不同的 。
  * **動態更新**：風格標準是一份動態文件，應隨專案演進與團隊成長而更新 。
  * **先學後破**：作為初學者，在打破規則前先學會規則（守破離精神）。初學者應保持指南精簡，最重要的是先寫出能運行的程式碼，再考慮「整潔」。
  * **自然發音**：偏好能自然發音且符合英文閱讀習慣的名稱。例如：`HorizontalAlignment` 優於 `AlignmentHorizontal` 。

#### Basic coding rules
  * DRY (Don't repeat yourself), don't WET (We Enjoy Typing/Write Everything Twice)
  * KISS (Keep it simple, stupid)
  * YAGNI (You aren't gonna need it)
  * SOLID for OOP (SRP、OCP、LSP、ISP、DIP)
  * PIE for FP (Purity, Immutability, Effects management)

---
### 命名規範 (Naming)
  * **變數命名**：應具備描述性、清晰且無歧義。除了布林值外，應使用**名詞**命名 。
  * **布林值 (Booleans)**：名稱應以**動詞**開頭（如 `isDead`, `isWalking`, `hasDamageMultiplier`），使其意義更明顯 。
  * **禁止縮寫**：除非是數學運算或通用的縮寫，否則變數名應完整揭示其意圖 。
  * **避免常用詞**：為了避免在命名空間或問題領域中產生歧義，應避開通用詞彙，或加上前綴/後綴（例如使用 `PhysicsSolver` 而非 `Solver`） 。
  * 完整的 C# 程式碼標準（Coding Standard）與風格（Coding Style）以及命名慣例（Naming Convention）可參考以下文件
    * [Microsoft C# Coding Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
    * [C# at Google Style Guide](https://google.github.io/styleguide/csharp-style.html)
    * [Unity 提供的 Style Example 原始 C# 內容](#styleexample)

#### 大小寫與前綴 (Casing and Prefixes)
  * **Pascal Case**：用於類別名、公共欄位、方法（如 `ExamplePlayerController`, `MaxHealth`）。
  * **Camel Case**：用於區域變數、私有變數與參數（如 `examplePlayerController`, `maxHealth`）。
  * **避免使用**：蛇形命名法 (snake_case)、短橫線命名法 (kebab-case) 或匈牙利命名法。

#### Naming Convention
  * 重點如下
    * **Identifier names**
        * Camel case (camelCase) : 區域變數和方法參數使用駝峰式命名。
        * Pascal case (PascalCase) : 在 Unity 開發中，用於類別、public fields和方法名稱。

    * **Fields and variables**
        * 變數名稱使用名詞
          (✓) movementSpeed
          (✗) getMovementSpeed
        * 布林值前加上動詞
          (✓) isDead
          (✗) dead
        * 使用有意義的名稱，不要縮寫（除非是數學）  
          (✓) elapsedTimeInDays　、　(✗) d  
          (✓) healthPoints　　　　、　(✗) hp 
        * Public fields使用 Pascal Case (MyPropertyName)。
        * Private 變數使用 Camel Case (myPrivateVariable)。
        * 考慮使用前綴或特殊編碼 : private member (m_ 或 \_), static (s_)
        * 將常數變數命名為以 k_ 為前綴並使用 PascalCase : public const int k_MaxItems = 100;
        * 欄位會自動初始化其預設值 : 不要 assign 預設值，如0給int。
        * 指定存取層級修飾符，分區塊放置 Public, protected, private 變數。
        * 偏好可讀性而非簡潔性，簡短的文字或縮寫在現代IDE輔助下，沒有意義。
        * 每行只有一個變數宣告。
        * 避免冗餘的名稱 : class Player 內的變數，不要再加 Player  
          (✗) PlayerHealthPoint。
        * 避免使用玩笑、雙關語、髒話等。
        * 在避免模糊並始終尋找提升可讀性方式的同時，若類型 (type) 可從上下文中清楚得知，才使用 var  
            <font color="green">EXAMPLE: good use of var</font>
            ```
            var powerUps = new List<PowerUps>();
            var dictionary = new Dictionary<string, List<GameObject>>();
            ```
            <font color="red">AVOID: potential ambiguity</font>
            ```
            var powerUps = PowerUpManager.GetPowerUps();
            ```

    * **Enums**
        * Enum 使用 Pascal Case，盡量使用單一名詞且不要有前後綴 (prefix or suffix)。
        * Bitwise 的 Enums 加上 \[Flags] 作為強調  
          加上 \[Flags] 可用位元運算（Bitwise Operations）來組合它們
        ```
        [Flags]
        public enum ElementType
        {
            None = 0,
            Fire = 1,   // 000001
            Water = 2,  // 000010
            Earth = 4,  // 000100
        }

        ElementType magicAttackType = ElementType.Fire | ElementType.Water;
        ```

    * **Classes and interfaces**
        * 對類別名稱使用 Pascal 命名法的名詞或名詞短語。
        * 如果檔案中有 Monobehaviour，檔案名稱應該要一樣。
        * 在介面名稱前加上大寫字母 I 作為前綴。

    * **Methods**
        * 使用動詞或動詞片語作為名稱開頭 : GetDirection, FindTarget。
        * 參數使用 Camel Case : public bool IsNewPosition(Vector3 currentPosition)。
        * 回傳布林值的方法，命名看起來應該像是在問問題 : IsGameOver, HasStartedTurn。

    * **Events and event handlers**
        * 使用動詞片語來命名事件 : OpeningDoor, DoorOpened
        * 對於事件使用 System.Action delegate
          ```
          // using System.Action delegate
          public event Action OpeningDoor; // event before
          public event Action DoorOpened; // event after
          public event Action<int> PointsScored;
          public event Action<CustomEventArgs> ThingHappened;
          ```
        * 事件觸發的方法加上 “On” 前綴 : OnOpeningDoor, OnDoorOpened
        * （在觀察者中）可考慮在事件處理方法前加上主體名稱並用底線（\_）分隔 :   
          例如一個命名為 "PlayerFiredEvent" 的主體，相關事件可以命名為 PlayerFiredEvent_OpeningDoor, PlayerFiredEvent_DoorOpened
        * 只有真的必要時才創建自訂的 EventArgs
          ```
          // EventArgs 是一個類別 (Class)，屬於引用類型（Reference Type），
          // 會在 Heap 上分配記憶體，引起 GC
          public event EventHandler<DamageEventArgs> OnDamage; 

          // 推薦：直接傳遞數值
          public event Action<float, Vector3> OnDamage;

          // 或透過結構 (會在 Stack 上分配記憶體，不產生 GC)
          public struct DamageData {
              public float amount;
              public Entity source;
          }
          public event Action<DamageData> OnDamage;
          ```

    * **Namespaces**
        * 使用 Pascal Case 命名法，不要使用特殊符號或底線。
        * 在檔案頭使用 using 指令，下方程式碼避免輸入命名空間的前綴。
        * 遇到相同名稱的方法時，可以加上命名空間來區別。
        * 建立子命名空間，將腳本組織成層級結構。例如 MyApplication.GameFlow、MyApplication.UI。
        * 有些人喜歡用與專案資料夾結構相符的命名空間，這樣可以更容易的了解程式碼庫 (codebase) 結構。

---
### 格式與間距 (Formatting & Spacing)
  * **大括號風格**：選擇 Allman (括號換行) 風格
    * 就算只有一行，也加上大括號，且括號換行
  * **行寬限制**：盡量保持每行程式碼簡短以不超過一個畫面最佳，但也不要過度換行。
  * **空白的使用**：
    * 在逗號之後加空白。
      * CollectItem(myObject, 0, 1);
    * 在 `if` 或 `while` 等流程控制條件前加上一個空格。
      * for (int i = 0; i < 100; i++)
    * 括號內不留空格，函式名與括號間不留空格。
      * DropPowerUp(myPrefab, 0, 1);
  * **變數聲明**：多數情況下每行僅聲明一個變數，以提高易讀性。

#### 格式（Formatting）
  * Properties
    * 對於單行唯讀屬性，盡量使用運算式主體屬性 (expression-bodied properties)（=>）：
    * 其他所有情況都使用舊式 { get; set; } 語法：
    * 也可以使用函式。簡單的 get/set 使用 Property 方式，複雜的邏輯或計算，使用函式
      ```
      // EXAMPLE: expression bodied properties
      public class PlayerHealth
      {
          // the private backing field
          private int m_maxHealth;
          private int m_maxMana;

          // read-only, returns backing field
          public int MaxHealth => m_maxHealth;

          // equivalent to:
          // public int MaxHealth { get; private set; }

          // explicit full-form getter (與 => 寫法等效)
          public int MaxHealthExpanded
          {
              get { return m_maxHealth; }
          }

          // explicitly implementing getter and setter
          // 注意：property 名稱必須與 backing field 不同，否則會造成無限遞迴
          public int MaxMana
          {
              get => m_maxMana;
              set => m_maxMana = value;
          }

          // write-only (not using backing field)
          public int Mana { private get; set; }

          // write-only, without an explicit setter
          public void SetMaxMana(int newMaxValue) => m_maxMana = newMaxValue;
      }
      ```

  * Serialization
    * 使用 \[SerializeField] 特性（Attribute）。
    * 使用 Range 特性設定最小值和最大值。
    * 利用可序列化的類別或結構讓 Inspector 的顯示更簡潔。
    * \[SerializeField] 不要單獨使用一行
      ```
      // EXAMPLE: a serializable class for PlayerStats
      using System;
      using UnityEngine;
  
      public class Player : MonoBehaviour
      {
          [Serializable] public struct PlayerStats
          {
              public int MovementSpeed;
              public int HitPoints;
              public bool HasHealthPotion;
          }

          // EXAMPLE: The private field is visible in the Inspector
          [SerializeField] private PlayerStats m_stats;
      } 
      ```

  * 括號與縮排風格（brace and indentation style）
    * 使用 Allman 風格，將左大括號放在新的一行，也稱為 BSD 風格（源自 BSD Unix）。
    * 不使用 K&R style（也稱 one true brace style）  
        <font color="blue">EXAMPLE: Allman or BSD style puts opening brace on a new line.</font>
        ```
        // 很容易看出對齊區塊，不需要動眼去尋找左括號位置
        void DisplayMouseCursor(bool showMouse)
        {
            if (!showMouse)
            {
                Cursor.lockState = CursorLockMode.Locked;
                Cursor.visible = false;
            }
            else
            {
                Cursor.lockState = CursorLockMode.None;
                Cursor.visible = true;
            }
        }
        ```  
        <font color="red">EXAMPLE: K&R style puts opening brace on the previous line.</font>
        ```
        // 不使用這種方法，現代螢幕夠大，沒有需要節省空間。
        void DisplayMouseCursor(bool showMouse){
            if (!showMouse) {
                Cursor.lockState = CursorLockMode.Locked;
                Cursor.visible = false;
            }
            else {
                Cursor.lockState = CursorLockMode.None;
                Cursor.visible = true;
            }
        }
        ```
    * 縮排規則 : 使用 4 個 space (不使用 tab, 現代 IDE 都可以在 preferences/settings 裡面設定)。
    * 不省略大括號，即使是單行語句也一樣，且不要撰寫於同一行  
        <font color="blue">使用這種括號方式</font>
        ```
        for (int i = 0; i < 100; i++)
        {
            DoSomething(i);
        }
        ```
        <font color="red">不使用以下形式</font>
        ```
        // AVOID: 就算加上括號，也不要寫在同一行
        for (int i = 0; i < 100; i++) { DoSomething(i); }
  
        // AVOID: 沒有括號
        for (int i = 0; i < 100; i++) DoSomething(i);
        for (int i = 0; i < 100; i++)
            DoSomething(i);
        ```
    * 超過 3 層的 if-else 改用 switch。
    * switch 每層都要縮排，case 後可以加大括號也可以不加。
    * 盡量寫上 default
      ```
      // EXAMPLE: 每一層都縮排，不加大括號
      switch (someExpression)
      {
          case 0:
              DoSomething();
              break;
          case 1:
              DoSomethingElse();
              break;
          case 2:
              int n = 1;
              DoAnotherThing(n);
              break;
          default:
              // Handle unexpected or default case
              break;
      }

      // 加上大括號的方式
      switch (someExpression)
      {
          case 0:
          {
              DoSomething();
              break;
          }
          case 1:
          {
              DoSomethingElse();
              break;
          }
          case 2:
          {
              int n = 1;
              DoAnotherThing(n);
              break;
          }
          default:
          {
              // Handle unexpected or default case
              break;
          }
      } 
      ```
    * 空格的使用（Horizontal spacing）
      * 加入空格以降低程式碼的擁擠感
        ```          
        // EXAMPLE: add spaces to make the code easier to read
        for (int i = 0; i < 100; i++) 

        // AVOID: no spaces
        for(inti=0;i<100;i++)
        ```
      * 函式的參數之間用逗號隔開，逗號後面使用一個空格。
        ```
        // EXAMPLE: 參數間，在逗號後加一格空白
        CollectItem(myObject, 0, 1);

        // AVOID: 太擠了
        CollectItem(myObject,0,1); 
        ```
      * 函式、小括號以及參數之間都不加空格
        ```
        // EXAMPLE: 在函式名稱，小括號與參數之間不加空格
        DropPowerUp(myPrefab, 0, 1);

        //AVOID:
        DropPowerUp( myPrefab, 0, 1 );
        ```
      * 函式名和小括號之間不要使用空格
        ```
        // EXAMPLE: 在函式名和小括號之間不加空格
        DoSomething();

        // AVOID
        DoSomething (); 
        ```
      * 避免在中括號/方括號內使用空格
        ```
        // EXAMPLE:中括號前後省略空白
        x = dataArray[index];

        // AVOID
        x = dataArray[ index ];
        ```
      * 在流程控制的條件前使用一個空格
        ```
        // EXAMPLE: 條件之前添加一個空白; 利用空白隔開小括號
        while (x == y)

        // AVOID
        while(x==y)
        while(x == y)
        ```
      * 在比較運算子的前後各加一個空格
        ```
        // EXAMPLE: 運算子前後添加一格空白
        if (x == y)

        // AVOID
        if (x==y)
        ```
      * 不要讓一行程式碼過長。考慮適度的換行，但也不要過度換行。
      * 維持縮排/層級的結構。
      * 除非為了提高可讀性，否則不要使用列對齊(對 IDE 和版控不友善)。
        ```
        // EXAMPLE: 在型別和變數名稱之間只需要一格空白
        public float Speed = 12f;
        public Transform GroundCheck;
        public double GroundDistance = 0.4f;
        public GUILayerMask GUIGroundMask;

        // AVOID: 盡量避免使用列對齊
        public float        Speed = 12f;
        public Transform    GroundCheck;
        public double       GroundDistance = 0.4f;
        public GUILayerMask GUIGroundMask;
        ```
    * 換行的使用（Vertical spacing）
      * 將相關或相似的方法歸為一組。
      * 利用換行分隔類別不同的部分  
        例如變數和方法，類別和介面之間，可以透過兩行空白來隔開。
    * 程式碼區塊（Regions）
      * 類別的大小應控制在適度的範圍內，若類別過大，應考慮拆分。
      * 盡量不使用程式碼區塊來隱藏過長程式碼。
      * 函式可以被摺疊，透過設計，將過長程式碼拆解為多個函式。

---
### 命名與前綴 (Naming and Prefixes)
  * **成員變數前綴**：
    * 私有成員變數：`m_` 或底線 `_`。
    * 常數 (Constants)：`k_`。
    * 靜態變數 (Static)：`s_`。
  * **簡化初始化**：省略冗餘的初始化（如 `int` 預設值為 `0`，引用類型預設值為 `null`）。
  * **存取修飾符**：明確標記 `public`、`protected`、`private` 等修飾符以避免歧義。
  * **消除冗餘**：如果類別名為 `Player`，變數名稱用 `Score` 不要用 `PlayerScore`。

#### 類別（Classes）
  * Robert C. Martin 在 Clean Code 一書中說 : 
  > The first rule of classes is that they should be small.  
    The second rule is they should be even smaller than that.

  * 類別內的成員應分門別類，盡量將使用者需要知道或者會用到的成員往上放；不需要知道或用不到的成員，則移往越下方（以下為舉例，注意不要因此變得混亂，盡量做好分類，適度加上簡單註解）  
  　　_Public_ :
    * Fields
    * Properties
    _Protected_ :
    * Fields
    _Private_ :
    * Fields
    _Public_ :
    * Methods
    * Events / Delegates
    * Monobehaviour Methods (Awake, Start, OnEnable, OnDestroy, etc.)
    Private:
    * Methods

#### 方法（Methods）
  * 減少參數數量。
  * 避免過度重載。
  * 避免副作用 : 函式名稱應該只做如他的名稱所表示的事情。小心傳入以及回傳的 reference 物件。
  * 不要傳遞旗標 (flag)，而是創建一個新方法 : 不要讓一個函式透過旗標而有不同的用途
    例如 : 不要建立根據旗標設定返回角度或弧度的 GetAngle 方法。直接創建 GetAngleInDegrees 和 GetAngleInRadians 兩個方法。
    > 在 Unity 的 C# 世界裏，會習慣把在 class 或 object 內的 function 都稱為 method
      因為 function 通常還包含其他的意涵，例如一段執行特定任務的程式碼區塊。
  * 可以考慮使用 Extension methods 來擴展 UnityEngine API 的功能。

---
### 註解與組織 (Comments & Organization)
  * **為何而寫**：註解應說明「為什麼 (Why)」，而非僅僅是「什麼 (What)」或「如何做 (How)」。如果程式碼過於複雜需要註解說明，應先考慮重構 。
  * **自帶說明**：優先使用良好的命名來取代註解 。
  * **Inspector 工具**：對於序列化欄位，使用 `[Tooltip]` 代替註解，以便在 Unity 編輯器中查看 。
  * **類別排序**：建議順序為：公有 -> 欄位 (Fields) -> 屬性 (Properties) -> 私有 -> 欄位 -> 事件 (Events) -> MonoBehaviour 方法 (Awake, Start 等) 。
  * **禁止事項**：避免使用 `#region`（這通常代表類別、方法過於龐大）、  
  避免標註作者（應使用版本控制系統追蹤變更）。

#### 註解（Comments）
```
Code is like humor, If you have to explain it, it's bad.
             - Cory House, software architect and author
```
  * 註解的目的是要解釋核心邏輯或說明流程、功能作用、演算法等。
  * 有用的註解會更具焦在程式碼無法輕易說明的資訊上，例如設計上的 Why。
  * 註解應適度就好，透過良好的設計及命名並遵循KISS原則，程式碼本身應能自我解釋。
  * 不要用註解取代糟糕的程式碼 : 如果邏輯錯綜複雜需要解釋，考慮重構程式碼。
  * 正確命名的類別、變數及方法可以取代註解  
    <font color="red">AVOID: noisy, redundant comments</font>
    ```
    // the target to shoot
    Transform targetToShoot; 
    ```
  * 盡可能將註解放在單獨的一行上，不要放在該行程式碼的後面 (視使用情況)
  * 大多數情況下盡可能使用雙斜線 (//) 來註解
  * 在 Unity 中，對於 serialized fields，多使用 tooltip 來代替註解
  * 可以在 public 方法或函式前使用 XML summary tag
    ```
    // summary tag 可以讓 IDE 顯示該註解
    // 部分插件可以以此產生技術文檔或形成欄位提示資訊

    /// <summary>
    /// Controls the weapon system including firing, reloading, and dealing damage
    /// </summary>
    public void Fire()
    {
        ...
    }
    ```
  * 在註解分隔符號 (//) 和註解文字之間插入一個空格。
  * 加上法律免責聲明 : 例如版權宣告等資訊 (license or copyright)；  
    過長的完整版，可選擇用 link 的方式。
  * 固定註解樣式 : 例如盡量使用英文，維持特定格式。
  * 不要在註解周圍建立星號或特殊字元的格式化區塊
  * 刪除註解掉的程式碼 : 註解掉的程式碼幾乎都不會再次使用，若真有需要，善用 source control
    > Commented-out code is a sign of a lack of confidence in your version control system.
  * 保持 TODO 註解的更新 : 完成功能後，記得清理 TODO 註解；  
    如果你沒有打算實作，就不要留下 TODO。請記得 YANGI 原則。
  * 避免當成開發日誌 : 不要在註解內留下你的開發足跡，善用 source control。
  * 避免署名 : 不要在註解內留下使用者名稱，source control 可以做的更好。

---
#### UI Toolkit 的 Naming Convention
  * 由於 UI 的元件往往數量龐大，且不同 UI 間的元件很相似，良好的命名能讓開發者易於了解結構、目的並管理之。
  * 在 Unity 上，採用 Block Element Modifier (BEM) 的 naming convention 風格
  * BEM 格式 : block-name__element-name--modifier-name。例如 :
    * menu__home-button
    * menu__shop-button
    * navbar-menu__shop-button--small
  * 保持名稱簡短清晰（不要有模擬兩可的名稱）。
  * 避免在 BEM selectors 中使用類型名稱（例如 Button、Label）或元素名稱（例如 #my-button）。 BEM 名稱應該代表其角色和狀態，而不是類型。
  * 避免使用可能更改的名稱/修飾符（例如，當配色方案尚未最終確定時，使用“button-quit”而不是“button-red”）。
  * 將這些命名規則擴展到美術 assets，例如與 UI Toolkit interface 相關的 sprites 和 textures。
  * 如果會在其他專案中重複使用該元素，請考慮為您的類別（Classes）加上前綴（Prefixing），以避免與使用者現有的類別名稱發生衝突。透過命名空間（Namespacing）或增加前綴的方式，可以防止在整合至其他專案或函式庫時產生命名衝突。
  * 在建構函式（Constructor）中使用 AddToClassList()，將相關的 USS 類別加入到您的元素實例 (element instances) 中。此方法能確保在元素實例化的同時套用正確的樣式，進而保持 UI 程式碼的一致性與清晰度

---
#### 針對可能有問題的程式碼 (code smell) 的通用建議
  * 晦澀難懂的命名 (Enigmatic naming)：類別、方法和變數需要直觀、不廢話的名稱。可參閱上方的命名章節。
  * 沒必要的複雜性 (Needless complexity)： 當你試圖預測一個類別所有可能的開發需求時，就會發生「過度設計」。將大型的類別拆解成較小、專門的類別，讓每個部分各司其職。
  * 僵化性 (Inflexibility)： 注意是否維持 SRP 原則。一個微小的變動不應該要求你在其他地方也做出多處修改。如果你更新了一個只做一件事的方法，且更新後的邏輯運行正常，你會預期程式碼的其他部分在修改後也能繼續正常運作。
  * 脆弱性 (Fragility)： 如果你只做了一個微小的改動，結果所有功能都停止運作，這通常意味著架構出了問題。
  * 固定性 (Immobility)： 你通常會編寫在不同情境下可重複使用的程式碼。如果將代碼部署到其他地方需要依賴過多項目，那麼請將邏輯運作方式進行「解耦（Decouple）」。
  * 重複代碼 (Duplicate code)： 如果明顯看得出程式碼是剪貼過來的，那就是時候進行「重構（Refactor）」了。
  * 過多註解 (Excessive commentary)： 註解有助於解釋不夠直觀的程式碼，但開發者有時會過度使用。較短的程式碼自然就不需要過多解釋。

---
### 整合範例程式碼 (Example)
```csharp
// 移除沒有用到的using
using System.Collections;
using System.Collections.Generic;
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
        // 十進位                          // 二進位
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
    //      MonoBehaviour的類別，應讓檔案維持一樣的名稱
    public class StyleExample : MonoBehaviour
    {
        // 欄位：避免特殊字元，布林值以動詞開頭

        // 靜態欄位使用 s_ 前綴
        static int s_sharedCount;

        // 常數使用 k_ 前綴
        const int k_maxCount = 100;


        // 使用 [SerializeField] 在 Inspector 中顯示私有變數
        [SerializeField] bool m_isPlayerDead;

        // 使用 Range 屬性限制範圍，可限制 Inspector 中能輸入的範圍
        [SerializeField, Range(0f, 1f)] float m_rangedStat;

        // Tooltip 可同時作為註解與編輯器提示
        [Tooltip("這是給玩家的另一項統計數據。")]
        [SerializeField] float m_anotherStat;
        

        // 唯讀屬性，使用運算式主體 (expression-bodied)
        public int MaxHealthReadOnly => m_maxHealth;

        // 自動實作屬性
        public string DescriptionName { get; set; } = "Fireball";

        // 事件：以動詞詞組命名。現在分詞代表「之前」，過去分詞代表「之後」
        public event Action OpeningDoor; // 開門中 (之前)
        public event Action DoorOpened;  // 門已開 (之後)


        // 私有欄位建議使用 m_ 前綴以區分區域變數
        private int m_maxHealth;
        private int m_elapsedTimeInSeconds;


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

        private void FormatExamples(int someExpression)
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

        private void DoSomething(int x)
        {
            // ..
        }
    }

    // 其他結構：用於分組資料
    [Serializable] public struct PlayerStats
    {
        public int MovementSpeed;
        public int HitPoints;
        public bool HasHealthPotion;
    }
}
```


---
#### StyleExample
<details>
  <summary>點擊展開</summary>

```
// **UNITY C# 風格指南：**
// | '// |' 風格的註解表示文件標記。 
// | 這是用於您的 Unity 專案的範例風格指南，靈感來自微軟（Microsoft）。 
// | 微軟的架構設計指南請見： [https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/) 
// | Google 的風格指南也維護於此： [https://google.github.io/styleguide/csharp-style.html](https://google.github.io/styleguide/csharp-style.html) 
// | 本指南與任何其他風格指南一樣，並非規則手冊。 
// | 它是您團隊討論並商定共同風格的一個起點。 
// | 沒有一種萬用的風格指南。請挑選最適合您團隊和專案的內容。 
// | 雖然沒有所謂正確的解決方案，本指南提供了一些您可能想要考慮的規範靈感。 
// | 最重要的事情是保持一致。 如果您有團隊，請商定一套風格指南並堅持執行。 
// | 風格指南的目標是消除編碼慣例中的猜測成分 
// | 以及格式化問題，讓您和您的團隊可以專注於解決方案。 
// | 因此，排除這些因素後，請參考本指南、微軟或 Google 的指南作為靈感 
// | 甚至作為建立您自己風格指南的起點。 
// | 略過、增加或自訂這些規則來建立您自己的團隊風格指南，然後一致地應用。 

// **核心原則 (KEY PRINCIPLES)**
// | 易讀性勝過簡短。清晰度比省略幾個母音所節省的任何時間都重要。 
// | 目標是讓您的程式碼更具易讀性、可維護性且一致。 
// | 從一開始就正確命名這類事情，將為您以後節省時間和精力。 
// | 特別是在除錯和擴充功能時。 
// | 盡可能遵守業界標準和慣例。 然而，要務實。 
// | 100 人以上團隊的需求與 2 人團隊的需求是不同的。 
// | 承接上述，程式碼風格標準是一份動態文件。 
// | 它應該隨著專案的演進和團隊的成長而更新。 
// | 作為初學者，在打破規則之前先學習規則是很重要的。 
// | 隨著您獲得經驗，您可以對於何時偏離規則做出明智的決定。 
// | 然而，作為初學者，請保持指南簡潔。 
// | 最重要的事情是在讓程式碼變得「整潔」之前，先寫出能運行的程式碼。 
// | 程式碼風格是個人喜好。最重要的事情是保持一致。 
// | 如果您有團隊，請商定一套風格指南並堅持執行。 
// | 偏好能自然發音且具易讀性的名稱 
// | 例如：HorizontalAlignment 而非 AlignmentHorizontal（更符合英文閱讀習慣） 

// **命名 (NAMING)**
// | 這點值得重複：易讀性勝過簡短。 
// | 清晰度比省略幾個母音所節省的任何時間都重要。 
// | 從一開始就挑選有意義的名稱，以減少以後的重構。 
// | 變數名稱應該具有描述性、清晰且無歧義，因為它們代表一個事物或狀態。 
// | 命名時使用名詞，除非變數類型是布林值 (bool)。 
// | 在布林值前加上動詞，使其含義更明顯。例如：isDead, isWalking, hasDamageMultiplier。 
// | 使用有意義的名稱。 不要縮寫（除非是數學或通用的縮寫）。您的變數名稱應該揭示其意圖。 
// | 選擇易於閱讀的標識符名稱。 
// | 例如，名為 HorizontalAlignment 的屬性比 AlignmentHorizontal 更具易讀性。 
// | 通過避免使用通用術語或增加前綴/後綴，使類型名稱在跨命名空間和問題領域時無歧義。 
// | （例如：使用 'PhysicsSolver'，而非 'Solver'） 

// **大小寫與前綴 (CASING AND PREFIXES):**
// | 使用 Pascal case（例如：ExamplePlayerController, MaxHealth 等），除非另有說明。 
// | 對於區域/私有變數和參數，使用 camel case（例如：examplePlayerController, maxHealth 等）。 
// | 避免 snake case, kebab case, 匈牙利命名法。 
// | 如果文件中有 MonoBehaviour，源文件名稱必須匹配。 
// | 考慮為私有成員變數 (m_)、常數 (k_) 或靜態變數 (s_) 使用前綴， 
// | 這樣名稱一眼就能揭示更多關於變數的信息。 
// | 或者，有些指南建議在私有成員變數前 
// | 加上底線 (_) 以將其與區域變數區分開。 
// | 捨棄冗餘的初始化（即：整數不要寫 '= 0'，引用類型不要寫 '= null' 等） 
// | 因為它們預設就會被初始化為 0 或 null。 
// | 一致地指定（或省略）存取級別修飾符。 
// | 我們建議明確指定 private，使存取級別清晰並避免任何歧義。 
// | 避免冗餘的名稱：如果您的類別名為 Player，您不需要創建名為 PlayerScore 或 PlayerTarget 的成員變數。 
// | 將它們簡化為 Score 或 Target。 

// **格式化 (FORMATTING):**
// | 選擇 K&R（左大括號在同一行）或 Allman（左大括號在下一行）風格的大括號。 
// | 易讀性是關鍵。盡量保持行短。考慮水平空白。 
// | 您也可以在風格指南中定義標準的最大行寬（有些人偏好少於 120 個字元）。 
// | 將長行拆分為較短的語句，而不是讓它溢出。 
// | 在流程控制條件前使用單個空格，例如：while (x == y)。 
// | 括號內避免空格，例如： x = dataArray[index]。 

// 間距 (SPACING):
// | 在函式引數間的逗號後使用單個空格，例如：CollectItem(myObject, 0, 1); 
// | 不要在括號和函式引數後添加空格，例如：CollectItem(myObject, 0, 1); 
// | 函式名稱和括號之間不要使用空格，例如：DropPowerUp(myPrefab, 0, 1); 
// | 使用垂直間距（額外的空行）進行視覺分隔，例如：
```
  for (int i = 0; i < 100; i++)
  {
      DoSomething(i);
  } 
```
在大多數情況下，每行使用一個變數聲明。雖然較不緊湊，但能增強易讀性。 
// | 函式名稱和括號之間不要使用空格，例如：DropPowerUp(myPrefab, 0, 1); 
// | 在流程控制條件前使用單個空格，並在比較運算符前後各使用一個空格，例如： if (x == y)。 

// **註解 (COMMENTS):**
// | 必要時加註解。 也就是當程式碼本身無法解釋，且除了揭示意圖的良好命名之外還需要進一步說明時。 
// | 然而，如果您需要添加註解來解釋一段糾結複雜的邏輯，請考慮重構您的程式碼使其更明顯。 
// | 良好的命名可以消除猜測。在註解之前請考慮重新命名。 
// | 註解不應只是回答「是什麼」或「如何做」，而是填補空白並告訴我們「為什麼」。 
// | 使用 // 註解將解釋保持在邏輯旁邊。 
// | 如果您的 Inspector 中的欄位需要解釋，請對序列化欄位使用 Tooltip 而非註解。 
// | 與其使用 Region，不如將其視為一種「程式碼異味 (code smell)」，這表示您的類別太大需要重構。 
// | 對於法律信息或許可證，請使用外部參考鏈接以節省空間。 
// | 在公共方法或函式前使用 XML 摘要標籤（summary），用於輸出文件/智慧感知 (Intellisense)。 
// | 避免標註貢獻者，例如： // Created by, // Modified by 等。使用版本控制來追蹤變更。 
// | 記錄「為什麼」遠比記錄「是什麼」或「如何做」重要得多。 

// **類別組織 (CLASS ORGANIZATION):**
// | 按以下順序組織您的類別：欄位 (Fields)、屬性 (Properties)、事件 (Events)、 
// | MonoBehaviour 方法 (Awake, Start, OnEnable, OnDisable, OnDestroy 等)、公共方法、私有方法、其他類別。 
// | 通常不鼓勵使用 #region，因為它會隱藏複雜性並增加程式碼閱讀難度。 

---
## 範例程式碼 (Code Example)
```csharp
// **引用行 (USING LINES):**
// | 將引用行保持在文件頂部。 
// | 刪除未使用的行。 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

// 命名空間 (NAMESPACES):
// | 使用命名空間以確保您的類別、介面、列舉等 
// | 不會與來自其他命名空間或全局命名空間的現有內容發生衝突。 
// | 使用 Pascal case，不帶特殊符號或底線。 
// | 在頂部添加引用行，以避免重複輸入命名空間。 
// | 使用點 (.) 運算符創建子命名空間，例如：MyApplication.GameFlow, MyApplication.AI 等。 
// | 有些人建議命名空間應反映專案的資料夾結構，以便進行邏輯分組。 
// | 刪除未使用的 'usings'，除了「最小必要集」之外。 

namespace UnityCSharpStyleSheetExample
{
    // 列舉 (ENUMS):
    // | 當一個物件或動作一次只能有一個值時，使用列舉。 
    // | 列舉名稱和值使用 Pascal case。 
    // | 列舉名稱使用單數名詞，因為它代表一組可能值中的單個值。 
    // | 它們不應有前綴或後綴。 
    // | 您可以將公共列舉放在類別之外，使其成為全局性的。 
    public enum Direction
    {
        North,
        South,
        East,
        West,
    }

    // 旗標列舉 (FLAG ENUM):
    // | 當可以同時選擇多個值時，使用旗標列舉來代表選項組合，並啟用位元運算。 
    // | 使用複數名詞來指示多重選擇的可能性（例如：AttackModes）。 
    // | 無前綴或後綴。 
    // | 對二進制值使用列對齊。 
    // | 或者，考慮使用 1 << bitnum 風格。 
    [Flags]
    public enum AttackModes
    {
        // 十進制                           // 二進制
        None = 0,                          // 000000
        Melee = 1,                         // 000001
        Ranged = 2,                        // 000010
        Special = 4,                       // 000100
        MeleeAndSpecial = Melee | Special  // 000101
    }

    // 介面 (INTERFACES):
    // | 當不相關的類別需要共享共同功能但實現方式不同時，介面允許您定義共同契約。 
    // | 介面名稱前綴大寫的 I 
    // | 接著使用描述功能的形容詞詞組來命名介面。 
    public interface IDamageable
    {
        string DamageTypeName { get; }
        float DamageValue { get; }

        bool ApplyDamage(string description, float damage, int numberOfHits);
    }

    public interface IDamageable<T>
    {
        void Damage(T damageTaken);
    }


    // 類別或結構 (CLASSES or STRUCTS):
    // | 使用 Pascal case 
    // | 用名詞或名詞詞組命名。 這將類型名稱與方法區分開來，後者以動詞詞組命名。 
    // | 避免前綴。 
    // | 每個文件一個 MonoBehaviour。如果您文件中有 MonoBehaviour，源文件名稱必須匹配。 
    public class StyleExample : MonoBehaviour
    {
        // 欄位 (FIELDS): 
        // | 避免特殊字元（反斜線、符號、Unicode 字元）；這些會干擾命令行工具。 
        // | 名稱使用名詞，但布林值前綴動詞。 
        // | 使用有意義的名稱。使名稱可搜索且可發音。 不要縮寫（除非是數學）。 
        // | 公共欄位使用 Pascal case。私有變數使用 camel case。 
        // | 指定（或省略）預設的私有存取修飾符，但要保持一致。 
        // | 如果您認同這不會對歧義或易讀性產生負面影響，我們建議省去隱含且冗餘的部分（例如 private）以保持簡潔。 
        // | 關於前綴的使用有很多意見。 
        // | 挑選最適合您和團隊的方式，並與您的風格指南保持一致。 
        // | 您可以考慮在私有欄位前加上底線 (_) 以與區域變數區分。 
        // | 您也可以使用更明確的前綴：m_ = 成員變數，s_ = 靜態，k_ = 常數 
        // | 以易讀性勝過簡短作為指導原則。 

        // 有些人偏好不對私有欄位使用任何前綴，並如下指定存取修飾符
        private int elapsedTimeInHours;

        // 有些人偏好對私有欄位使用底線前綴。
        int _elapsedTimeInDays;

        // 我們推薦使用明確的前綴：m* = 成員變數，這有利於明確性和
        // 易讀性，勝過節省幾次按鍵。然而，我們建議省略 "private"，因為它為了簡潔而隱含存在。 
        // 無論您選擇哪種風格，請保持一致。
        int m_elapsedTimeInSeconds;

        // 如果您想在 Inspector 中顯示私有欄位，請使用 [SerializeField] 屬性。
        // 布林值提出一個可以用真或假回答的問題。
        [SerializeField] bool m_isPlayerDead;

        // 靜態欄位前綴為 s_
        static int s_sharedCount;

        // 常數前綴為 k_
        const int k_maxCount = 100;

        // 這會在 Inspector 中將來自自定義 PlayerStats 類別的數據進行分組。
        [Serializable] PlayerStats m_stats;

        // 使用 Range 屬性設置最小值和最大值。 
        // 這將值限制在一個範圍內，並在 Inspector 中創建一個滑桿。
        [Range(0f, 1f)] [SerializeField] float m_rangedStat;

        // Tooltip 可以取代序列化欄位上的註解，並起到雙重作用。
        [Tooltip("這是玩家的另一項統計數據。")]
        [SerializeField] float m_anotherStat;

        // 屬性 (PROPERTIES):
        // | 優於公共欄位。 
        // | Pascal case，不帶特殊字元。 
        // | 使用運算式主體屬性（expression-bodied properties）來縮短，但選擇您偏好的格式。 
        // | 例如：對唯讀屬性使用運算式主體，但對其他屬性使用 { get; set; }。 
        // | 對沒有後備欄位的公共屬性使用自動實作屬性（Auto-Implemented Property）。 
        // | 雖然您也可以使用函式來公開私有數據，但通常建議使用屬性。 
        // | 對於涉及複雜邏輯或運算的 get 或 set 操作，請使用方法而非屬性。 

        // 私有後備欄位
        int m_maxHealth;

        // 唯讀，回傳後備欄位
        public int MaxHealthReadOnly => m_maxHealth;

        // 等同於：
        // public int MaxHealth { get; private set; }

        // 明確地實現 getter 和 setter
        public int MaxHealth
        {
            get => m_maxHealth;
            set => m_maxHealth = value;
        }

        // 唯寫（不使用後備欄位）
        public int Health { private get; set; }

        // 唯寫，不帶明確的 setter
        public void SetMaxHealth(int newMaxValue) => m_maxHealth = newMaxValue;

        // 自動實作屬性，無後備欄位
        public string DescriptionName { get; set; } = "Fireball";

        // 事件 (EVENTS):
        // | 使用動詞詞組命名。
        // | 現在分詞表示「之前」，過去分詞表示「之後」。 
        // | 大多數事件使用 System.Action 委派（可以接受 0 到 16 個參數）。 
        // | 僅在必要時定義自定義 EventArg（使用 System.EventArgs 或自定義結構）。 
        // | 或者，也可以使用 System.EventHandler； 選擇一個並一致應用。 
        // | 為事件、事件處理方法（訂閱者/觀察者）選擇命名方案， 
        // | 以及事件引發方法（發布者/主體） 
        // | 例如： event/action = "OpeningDoor", 事件引發方法 = "OnDoorOpened", 事件處理方法 = MySubject_DoorOpened" 

        // 之前的事件
        public event Action OpeningDoor;

        // 之後的事件
        public event Action DoorOpened;

        // 帶整數參數的事件
        public event Action<int> PointsScored;

        // 帶自定義 EventArgs 的自定義事件
        public event Action<CustomEventArgs> ThingHappened;

        // 這些是事件引發方法，例如 OnDoorOpened, OnPointsScored 等。
        // 在（主體中的）事件引發方法前綴加上 “On”。 
        //  或者，事件處理方法例如 MySubject_DoorOpened()。 

        public void OnDoorOpened()
        {
            DoorOpened?.Invoke();
        }

        public void OnPointsScored(int points)
        {
            PointsScored?.Invoke(points);
        }

        // 這是由結構組成的自定義 EventArg。
        public struct CustomEventArgs
        {
            public int ObjectID { get; }
            public Color Color { get; }

            public CustomEventArgs(int objectId, Color color)
            {
                this.ObjectID = objectId;
                this.Color = color;
            }
        }

        // 方法 (METHODS):
        // | 雖然「函式 (function)」和「方法 (method)」常被交替使用，但在 Unity 開發中「方法」是正確術語 
        // | 因為在 C# 中您無法在不將其併入類別的情況下編寫函式。 
        // | 以動詞或動詞詞組開始方法名稱以顯示動作。必要時增加上下文。例如： GetDirection, FindTarget 等。 
        // | 返回 bool 的方法應該提出問題：就像布林變數本身一樣。 
        // | 如果方法返回真假條件，則在方法前加前綴動詞。 
        // | 這將它們以問題的形式表達，例如： IsGameOver, HasStartedTurn 
        // | 參數使用 camel case。 格式化傳入方法的參數要像區域變數一樣。 

        // | 一些關於方法的一般提示： 
        // | 避免長方法。 如果一個方法太長，考慮將其拆分為較小的方法。 
        // | 避免參數過多的方法。 如果一個方法有超過三個參數， 
        // | 考慮使用類別或結構來對它們進行分組。 
        // | 避免過度重載：您可以生成無窮無盡的方法重載排列。 
        // | 避免副作用：一個方法只需要做它名稱所標榜的事。 
        // | 方法的好名稱反映了它所做的事情。 
        // | 避免根據旗標將方法設置為在多種不同模式下工作。 
        // | 改為建立兩個具有不同名稱的方法，例如 GetAngleInDegrees 和 GetAngleInRadians。 

        // 方法以動詞開始。
        public void SetInitialPosition(float x, float y, float z)
        {
            transform.position = new Vector3(x, y, z);
        }

        // 當方法返回 bool 時提出問題。
        public bool IsNewPosition(Vector3 newPosition)
        {
            return (transform.position == newPosition);
        }

        void FormatExamples(int someExpression)
        {
            // VAR:
            // | 在避免歧義並始終尋求改進易讀性的同時， 
            // | 您可以在類型從上下文中清晰可見時使用 var， 
            // | 有了良好的命名，歧義就不再是問題，因為變數名稱已經傳達了意圖。 
            // | 使用 var 重構更簡單，因為它抽象化了具體類型， 
            // | 減少了當類型改變時需要更新代碼的地方。 
            // | 在 foreach 循環中，var 確保迭代變數與列舉器提供的類型匹配。 
            // | 如果您明確聲明了不匹配的類型，編譯器可能會允許它，從而導致運行時錯誤。 

            var powerUps = new List<PlayerStats>();
            var dict = new Dictionary<string, List<GameObject>>();

            // SWITCH 語句:
            // | 通常建議用 switch 語句取代較長的 if-else 鏈以獲得更好的易讀性。 
            // | 格式可以有所不同。為您的風格指南選擇一種並遵循它。 
            // | 這個範例縮進了每個 case 及其下方的 break。 
            switch (someExpression)
            {
                case 0:
                    // ..
                    break;
                case 1:
                    // ..
                    break;
                case 2:
                    // ..
                    break;
            }


            // 大括號 (BRACES): 
            // | 在可能的情況下，不要省略大括號，即使是單行語句。 
            // | 或者完全避免單行語句以利於除錯。 
            // | 在巢狀多行語句中保持大括號。 

            // 這個單行語句保留了括號...
            for (int i = 0; i < 100; i++) { DoSomething(i); }

            // ... 但這更具易讀性，且通常更利於除錯。
            for (int i = 0; i < 100; i++)
            {
                DoSomething(i);
            }

            // 為了易讀性分離語句。
            for (int i = 0; i < 10; i++)
            {
                for (int j = 0; j < 10; j++)
                {
                    DoSomething(j);
                }
            }
        }

        void DoSomething(int x)
        {
            // .. 
        }
    }

    // 其他類別 (OTHER CLASSES):
    // | 根據需要在文件中定義任意數量的其他輔助類別/非 MonoBehaviour 類別。 
    // | 這是一個可序列化的類別，在 Inspector 中對欄位進行分組。 
    [Serializable]
    public struct PlayerStats
    {
        public int MovementSpeed;
        public int HitPoints;
        public bool HasHealthPotion;
    }
}
```
</details>