# Clean Coding and Coding Style Guide
```
Any fool can write code that a computer can understand.
Good programmers write code that humans can understand.
                - Martin Fowler, author of Refactoring
```

This guide is not a rigid rulebook — suggestions should be raised and discussed with the team.
> **The most important principle: be consistent.** Follow this guide's recommendations and apply them consistently.

---
### Purpose
The purpose of designing and establishing a team Coding Standard / Coding Style is not to create hassle or showcase team flair. Even in the era of Vibe Coding, developing commercial-quality software is never a matter of writing a few lines of code in a few days. As the codebase grows, feature specs get revised, and team members come and go, developers inevitably need to take over others' code. In this context, the ability to quickly, effectively, and easily read and understand someone else's code is a critical key to maintaining quality in long-term development projects.

**The content of this document should be continuously revised as the team changes and grows. The principles here are not immutable — the most important guideline is to negotiate conventions and styles that the whole team can accept and is willing to consistently follow.**

A well-designed set of standards combined with a team willing to put in the effort brings many benefits:

1. **Readability and Maintainability**
   In large projects, developers frequently need to read code written by others. Unified indentation, naming conventions, and structure make the code "look like it was written by one person," effectively reducing the time needed to read and understand it. When fixing bugs or adding new features, a clear structure helps maintainers get up to speed quickly.
2. **Collaboration and Knowledge Sharing**
   Reduces new member onboarding friction: Newcomers can quickly understand project logic through the standards, reducing confusion caused by inconsistent styles.
   Optimizes Code Review workflows: With standards in place, reviewers can focus on program logic and architecture rather than debating brace placement or spacing details.
3. **Security and Error Prevention**
   Many standards enforce best practices, avoiding known risky functions or patterns, thereby reducing risks of security vulnerabilities and performance issues.
4. **Scalability**
   Large projects often span several years or more. As the codebase grows, chaotic styles lead to accumulating technical debt. Standardization ensures the code maintains good architecture at scale and remains amenable to automated testing and continuous integration.
5. **Reduced Cognitive Load**
   When all projects follow the same standards, engineers don't waste mental energy or extra cognitive time adjusting to different coding styles when switching between files, tasks, or projects.

---
### Principles

#### Key Principles
  * **Readability over brevity**: Clarity is more important than the time saved by omitting a few syllables.
  * **Clear and consistent goals**: The goal is to make code easier to read, maintain, and keep consistent.
  * **Start right from the beginning**: Naming things correctly from the start saves time debugging and extending features later.
  * **Be pragmatic**: While you should follow industry standards as much as possible, consider your team size. A 2-person team and a 100-person team have different needs.
  * **Living document**: Style standards are living documents that should evolve as the project progresses and the team grows.
  * **Learn before you break**: As a beginner, learn the rules before breaking them (Shu-Ha-Ri philosophy). Keep the guide concise — the most important thing is to write working code first, then worry about "clean" code.
  * **Natural pronunciation**: Prefer names that read naturally in English. For example: `HorizontalAlignment` is better than `AlignmentHorizontal`.

#### Basic coding rules
  * DRY (Don't repeat yourself), don't WET (We Enjoy Typing/Write Everything Twice)
  * KISS (Keep it simple, stupid)
  * YAGNI (You aren't gonna need it)
  * SOLID for OOP (SRP, OCP, LSP, ISP, DIP)
  * PIE for FP (Purity, Immutability, Effects management)

---
### Naming
  * **Variable naming**: Should be descriptive, clear, and unambiguous. Except for booleans, use **nouns**.
  * **Booleans**: Names should start with a **verb** (e.g., `isDead`, `isWalking`, `hasDamageMultiplier`) to make their meaning more obvious.
  * **No abbreviations**: Unless it's a math operation or a universally recognized abbreviation, variable names should fully reveal their intent.
  * **Avoid generic words**: To prevent ambiguity across namespaces or problem domains, avoid generic terms or add prefixes/suffixes (e.g., use `PhysicsSolver` instead of `Solver`).
  * For the complete C# Coding Standard, Coding Style, and Naming Convention, refer to the following documents:
    * [Microsoft C# Coding Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
    * [C# at Google Style Guide](https://google.github.io/styleguide/csharp-style.html)
    * [Unity-provided Style Example C# source](#styleexample)

#### Casing and Prefixes
  * **Pascal Case**: Used for class names, public fields, and methods (e.g., `ExamplePlayerController`, `MaxHealth`).
  * **Camel Case**: Used for local variables, private variables, and parameters (e.g., `examplePlayerController`, `maxHealth`).
  * **Avoid**: snake_case, kebab-case, or Hungarian notation.

#### Naming Convention
  * Key points:
    * **Identifier names**
        * Camel case (camelCase): Used for local variables and method parameters.
        * Pascal case (PascalCase): In Unity development, used for classes, public fields, and method names.

    * **Fields and variables**
        * Variable names use nouns
          (✓) movementSpeed
          (✗) getMovementSpeed
        * Prefix booleans with verbs
          (✓) isDead
          (✗) dead
        * Use meaningful names; do not abbreviate (unless it's math)
          (✓) elapsedTimeInDays  ,  (✗) d
          (✓) healthPoints       ,  (✗) hp
        * Public fields use Pascal Case (MyPropertyName).
        * Private variables use Camel Case (myPrivateVariable).
        * Consider using prefixes or special encoding: private member (m_ or \_), static (s_)
        * Name constant variables with the k_ prefix and PascalCase: public const int k_MaxItems = 100;
        * Fields are automatically initialized to their defaults: do not assign default values like 0 to int.
        * Specify access level modifiers; group Public, Protected, and Private variables into separate sections.
        * Prefer readability over brevity — short text or abbreviations provide no benefit with modern IDE assistance.
        * Declare only one variable per line.
        * Avoid redundant names: variables inside class Player should not repeat Player
          (✗) PlayerHealthPoint.
        * Avoid jokes, puns, profanity, etc.
        * While avoiding ambiguity and always seeking to improve readability, only use var when the type is clearly apparent from context
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
        * Enums use Pascal Case. Use singular nouns without prefixes or suffixes.
        * Add \[Flags] attribute for bitwise enums as emphasis
          Adding \[Flags] allows combining values using bitwise operations
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
        * Use Pascal case noun or noun phrases for class names.
        * If the file contains a MonoBehaviour, the file name should match.
        * Prefix interface names with uppercase letter I.

    * **Methods**
        * Start names with a verb or verb phrase: GetDirection, FindTarget.
        * Parameters use Camel Case: public bool IsNewPosition(Vector3 currentPosition).
        * Methods returning bool should read like a question: IsGameOver, HasStartedTurn.

    * **Events and event handlers**
        * Use verb phrases for event names: OpeningDoor, DoorOpened
        * Use System.Action delegate for events
          ```
          // using System.Action delegate
          public event Action OpeningDoor; // event before
          public event Action DoorOpened; // event after
          public event Action<int> PointsScored;
          public event Action<CustomEventArgs> ThingHappened;
          ```
        * Prefix event-raising methods with "On": OnOpeningDoor, OnDoorOpened
        * (In observers) consider prefixing event handler methods with the subject name separated by underscore (\_):
          For example, a subject named "PlayerFiredEvent" could have handlers: PlayerFiredEvent_OpeningDoor, PlayerFiredEvent_DoorOpened
        * Only create custom EventArgs when truly necessary
          ```
          // EventArgs is a class (reference type),
          // allocated on the Heap, causing GC
          public event EventHandler<DamageEventArgs> OnDamage;

          // Recommended: pass values directly
          public event Action<float, Vector3> OnDamage;

          // Or use a struct (allocated on the Stack, no GC)
          public struct DamageData {
              public float amount;
              public Entity source;
          }
          public event Action<DamageData> OnDamage;
          ```

    * **Namespaces**
        * Use Pascal Case. Do not use special characters or underscores.
        * Use using directives at the top of the file to avoid typing namespace prefixes in the code below.
        * When methods have identical names, use namespaces to disambiguate.
        * Create sub-namespaces to organize scripts into a hierarchical structure. For example: MyApplication.GameFlow, MyApplication.UI.
        * Some prefer namespaces that mirror the project folder structure, making it easier to understand the codebase layout.

---
### Formatting and Spacing
  * **Brace style**: Use Allman (opening brace on a new line) style.
    * Always use braces even for single-line statements, with the opening brace on a new line.
  * **Line width limit**: Keep lines of code short enough to fit on one screen when possible, but don't over-wrap.
  * **Whitespace usage**:
    * Add a space after commas.
      * `CollectItem(myObject, 0, 1);`
    * Add a space before flow control conditions like `if` or `while`.
      * `for (int i = 0; i < 100; i++)`
    * No spaces inside parentheses; no space between function name and parentheses.
      * `DropPowerUp(myPrefab, 0, 1);`
  * **Variable declarations**: In most cases, declare only one variable per line for readability.

#### Formatting
  * Properties
    * For single-line read-only properties, prefer expression-bodied properties (=>):
    * For all other cases, use the traditional { get; set; } syntax:
    * You can also use methods. Use properties for simple get/set; use methods for complex logic or calculations
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

          // explicit full-form getter (equivalent to => syntax)
          public int MaxHealthExpanded
          {
              get { return m_maxHealth; }
          }

          // explicitly implementing getter and setter
          // Note: property name must differ from backing field to avoid infinite recursion
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
    * Use the \[SerializeField] attribute.
    * Use the Range attribute to set minimum and maximum values.
    * Use serializable classes or structs to keep the Inspector display clean.
    * \[SerializeField] should not be on a separate line
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

  * Brace and indentation style
    * Use Allman style: place the opening brace on a new line (also known as BSD style, originating from BSD Unix).
    * Do not use K&R style (also called "one true brace style")
        <font color="blue">EXAMPLE: Allman or BSD style puts opening brace on a new line.</font>
        ```
        // Easy to see aligned blocks without searching for the opening brace
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
        // Do not use this style; modern screens are large enough, no need to save vertical space.
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
    * Indentation rule: use 4 spaces (not tabs; modern IDEs can be configured in preferences/settings).
    * Do not omit braces, even for single-line statements, and do not place them on the same line
        <font color="blue">Use this brace style</font>
        ```
        for (int i = 0; i < 100; i++)
        {
            DoSomething(i);
        }
        ```
        <font color="red">Do not use these forms</font>
        ```
        // AVOID: even with braces, do not write on the same line
        for (int i = 0; i < 100; i++) { DoSomething(i); }

        // AVOID: no braces
        for (int i = 0; i < 100; i++) DoSomething(i);
        for (int i = 0; i < 100; i++)
            DoSomething(i);
        ```
    * Replace if-else chains deeper than 3 levels with switch.
    * Indent each level of a switch statement. Cases may or may not have braces.
    * Always include a default case
      ```
      // EXAMPLE: each level indented, no braces on cases
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

      // With braces on cases
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
    * Horizontal spacing
      * Add spaces to reduce code density
        ```
        // EXAMPLE: add spaces to make the code easier to read
        for (int i = 0; i < 100; i++)

        // AVOID: no spaces
        for(inti=0;i<100;i++)
        ```
      * Separate function arguments with commas, with a space after each comma.
        ```
        // EXAMPLE: space after commas between arguments
        CollectItem(myObject, 0, 1);

        // AVOID: too cramped
        CollectItem(myObject,0,1);
        ```
      * No spaces between function name, parentheses, and arguments
        ```
        // EXAMPLE: no spaces between function name, parentheses, and arguments
        DropPowerUp(myPrefab, 0, 1);

        //AVOID:
        DropPowerUp( myPrefab, 0, 1 );
        ```
      * No space between function name and opening parenthesis
        ```
        // EXAMPLE: no space between function name and opening parenthesis
        DoSomething();

        // AVOID
        DoSomething ();
        ```
      * Avoid spaces inside square brackets
        ```
        // EXAMPLE: no spaces inside square brackets
        x = dataArray[index];

        // AVOID
        x = dataArray[ index ];
        ```
      * Use a single space before flow control conditions
        ```
        // EXAMPLE: add a space before the condition; use spaces to separate parentheses
        while (x == y)

        // AVOID
        while(x==y)
        while(x == y)
        ```
      * Use a space on each side of comparison operators
        ```
        // EXAMPLE: space on each side of operators
        if (x == y)

        // AVOID
        if (x==y)
        ```
      * Don't let a line of code get too long. Consider wrapping at appropriate points, but don't over-wrap.
      * Maintain indentation/hierarchy structure.
      * Unless it improves readability, do not use column alignment (unfriendly to IDEs and version control).
        ```
        // EXAMPLE: only one space between type and variable name
        public float Speed = 12f;
        public Transform GroundCheck;
        public double GroundDistance = 0.4f;
        public GUILayerMask GUIGroundMask;

        // AVOID: avoid column alignment
        public float        Speed = 12f;
        public Transform    GroundCheck;
        public double       GroundDistance = 0.4f;
        public GUILayerMask GUIGroundMask;
        ```
    * Vertical spacing
      * Group related or similar methods together.
      * Use blank lines to separate different parts of a class.
        For example, separate variables from methods, or classes from interfaces, with two blank lines.
    * Code regions
      * Keep class size moderate; if a class is too large, consider splitting it.
      * Avoid using code regions to hide long code.
      * Functions can be collapsed; through design, break long code into multiple functions.

---
### Naming and Prefixes
  * **Member variable prefixes**:
    * Private member variables: `m_` or underscore `_`.
    * Constants: `k_`.
    * Static variables: `s_`.
  * **Simplify initialization**: Omit redundant initialization (e.g., `int` defaults to `0`, reference types default to `null`).
  * **Access modifiers**: Explicitly mark `public`, `protected`, `private`, etc. to avoid ambiguity.
  * **Eliminate redundancy**: If the class is named `Player`, use `Score` instead of `PlayerScore`.

#### Classes
  * Robert C. Martin says in Clean Code:
  > The first rule of classes is that they should be small.
    The second rule is they should be even smaller than that.

  * Class members should be categorized. Place members that users need to know about or use toward the top; members they don't need to know about or won't use go toward the bottom (the following is an example — be careful not to create confusion; categorize well and add brief comments as needed):
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
    * MonoBehaviour Methods (Awake, Start, OnEnable, OnDestroy, etc.)
    Private:
    * Methods

#### Methods
  * Reduce the number of parameters.
  * Avoid excessive overloading.
  * Avoid side effects: a function's name should describe exactly what it does. Be careful with reference objects passed in or returned.
  * Don't pass flags — create a new method instead: don't let a single function serve different purposes via flags.
    For example: don't create a GetAngle method that returns degrees or radians based on a flag. Create GetAngleInDegrees and GetAngleInRadians as two separate methods.
    > In Unity's C# world, functions inside a class or object are typically called "methods"
      because "function" usually carries additional meanings, such as a standalone block of code that performs a specific task.
  * Consider using Extension methods to extend UnityEngine API functionality.

---
### Comments and Organization
  * **Explain "why"**: Comments should explain "why", not just "what" or "how". If the code is too complex and needs a comment to explain, consider refactoring first.
  * **Self-documenting**: Prefer good naming over comments.
  * **Inspector tooltips**: For serialized fields, use `[Tooltip]` instead of comments so they're visible in the Unity Editor.
  * **Class ordering**: Recommended order: Public -> Fields -> Properties -> Private -> Fields -> Events -> MonoBehaviour methods (Awake, Start, etc.).
  * **Prohibited practices**: Avoid `#region` (this usually indicates the class or method is too large).
  Avoid author annotations (use version control to track changes).

#### Comments
```
Code is like humor, If you have to explain it, it's bad.
             - Cory House, software architect and author
```
  * The purpose of comments is to explain core logic or describe workflows, feature behavior, algorithms, etc.
  * Useful comments focus on information that the code cannot easily convey, such as design decisions ("Why").
  * Comments should be used sparingly. Through good design, naming, and adherence to the KISS principle, code should be self-explanatory.
  * Don't use comments as a substitute for bad code: if the logic is tangled and needs explanation, consider refactoring.
  * Properly named classes, variables, and methods can replace comments
    <font color="red">AVOID: noisy, redundant comments</font>
    ```
    // the target to shoot
    Transform targetToShoot;
    ```
  * Place comments on a separate line whenever possible, not at the end of a code line (use judgment)
  * In most cases, use double slashes (//) for comments
  * In Unity, for serialized fields, prefer tooltips over comments
  * Consider using XML summary tags before public methods or functions
    ```
    // Summary tags allow IDEs to display the comment
    // Some plugins can generate technical documentation or field hints from these

    /// <summary>
    /// Controls the weapon system including firing, reloading, and dealing damage
    /// </summary>
    public void Fire()
    {
        ...
    }
    ```
  * Insert a space between the comment delimiter (//) and the comment text.
  * Add legal disclaimers: such as copyright notices (license or copyright);
    for lengthy full versions, consider using a link.
  * Maintain a consistent comment style: e.g., use English consistently, maintain a specific format.
  * Don't create formatted blocks of asterisks or special characters around comments
  * Delete commented-out code: commented-out code is almost never used again. If truly needed, use source control
    > Commented-out code is a sign of a lack of confidence in your version control system.
  * Keep TODO comments up to date: after completing a feature, clean up TODO comments;
    if you don't intend to implement something, don't leave a TODO. Remember the YAGNI principle.
  * Avoid using comments as a development log: don't leave your development trail in comments; use source control.
  * Avoid attribution: don't leave usernames in comments; source control does this better.

---
#### UI Toolkit Naming Convention
  * Since UI elements are often numerous and similar across different UIs, good naming helps developers easily understand the structure, purpose, and manage them.
  * In Unity, adopt the Block Element Modifier (BEM) naming convention style
  * BEM format: block-name__element-name--modifier-name. Examples:
    * menu__home-button
    * menu__shop-button
    * navbar-menu__shop-button--small
  * Keep names short and clear (no ambiguous names).
  * Avoid using type names (e.g., Button, Label) or element names (e.g., #my-button) in BEM selectors. BEM names should represent roles and states, not types.
  * Avoid names/modifiers that might change (e.g., use "button-quit" instead of "button-red" when the color scheme isn't finalized).
  * Extend these naming rules to art assets, such as sprites and textures related to UI Toolkit interfaces.
  * If the element will be reused across other projects, consider prefixing your classes to avoid conflicts with existing class names. Using namespacing or adding prefixes prevents naming collisions when integrating into other projects or libraries.
  * Use AddToClassList() in constructors to add relevant USS classes to your element instances. This ensures correct styles are applied at instantiation, maintaining consistency and clarity in UI code.

---
#### General Advice for Code Smells
  * **Enigmatic naming**: Classes, methods, and variables need intuitive, no-nonsense names. Refer to the naming section above.
  * **Needless complexity**: "Over-engineering" happens when you try to anticipate every possible requirement for a class. Break large classes into smaller, specialized classes where each part has a single responsibility.
  * **Inflexibility**: Check whether SRP is being maintained. A small change should not require multiple modifications elsewhere. If you update a single-responsibility method and the updated logic works correctly, the rest of the codebase should continue working after the change.
  * **Fragility**: If a single small change causes everything to break, this usually indicates an architectural problem.
  * **Immobility**: You typically write code that can be reused in different contexts. If deploying code elsewhere requires too many dependencies, decouple the logic.
  * **Duplicate code**: If code is clearly copy-pasted, it's time to refactor.
  * **Excessive commentary**: Comments help explain non-intuitive code, but developers sometimes overuse them. Shorter code naturally needs less explanation.

---
### Integrated Code Example
```csharp
// Remove unused usings
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

// Use namespaces to scope your classes, interfaces, enums, etc.
// Namespaces: use Pascal case, ideally reflecting the folder structure
namespace UnityCSharpStyleSheetExample
{
    // Enum: use when an object can only have one value at a time
    public enum Direction
    {
        North,
        South,
        East,
        West,
    }

    // Flags enum: use when multiple values can be selected simultaneously, use plural nouns
    [Flags]
    public enum AttackModes
    {
        // Decimal                             // Binary
        None = 0,                          // 000000
        Melee = 1,                         // 000001
        Ranged = 2,                        // 000010
        Special = 4,                       // 000100
        MeleeAndSpecial = Melee | Special  // 000101
    }

    // Interface: prefix with uppercase I, followed by an adjective phrase describing the capability
    public interface IDamageable
    {
        string DamageTypeName { get; }
        float DamageValue { get; }
        bool ApplyDamage(string description, float damage, int numberOfHits);
    }

    // Class: use Pascal case, primarily nouns
    //        MonoBehaviour classes should have a matching file name
    public class StyleExample : MonoBehaviour
    {
        // Fields: avoid special characters; prefix booleans with verbs

        // Static fields: use s_ prefix
        static int s_sharedCount;

        // Constants: use k_ prefix
        const int k_maxCount = 100;


        // Use [SerializeField] to expose private variables in the Inspector
        [SerializeField] bool m_isPlayerDead;

        // Use Range attribute to constrain values in the Inspector
        [SerializeField, Range(0f, 1f)] float m_rangedStat;

        // Tooltip serves as both a comment and an editor hint
        [Tooltip("Another stat for the player.")]
        [SerializeField] float m_anotherStat;


        // Read-only property using expression-bodied syntax
        public int MaxHealthReadOnly => m_maxHealth;

        // Auto-implemented property
        public string DescriptionName { get; set; } = "Fireball";

        // Events: name with verb phrases. Present participle = "before", past participle = "after"
        public event Action OpeningDoor; // Opening (before)
        public event Action DoorOpened;  // Opened (after)


        // Private fields: recommend m_ prefix to distinguish from local variables
        private int m_maxHealth;
        private int m_elapsedTimeInSeconds;


        // Event-raising methods are prefixed with "On"
        public void OnDoorOpened()
        {
            DoorOpened?.Invoke();
        }

        // Methods: start with a verb. Methods returning bool should read like a question
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
            // VAR: use when the type is clear from context
            var powerUps = new List<PlayerStats>();

            // Switch statement: replace deeply nested (3+) if-else chains
            // Always include a default case (can be auto-generated by IDE)
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

            // Braces: always use braces even for single-line statements to aid debugging
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

    // Other structures: used for grouping data
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
  <summary>Click to expand</summary>

```
// **UNITY C# STYLE GUIDE:**
// | '// |' style comments represent document annotations.
// | This is a sample style guide for your Unity project, inspired by Microsoft.
// | Microsoft's Framework Design Guidelines can be found at: [https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/)
// | Google's style guide is also maintained at: [https://google.github.io/styleguide/csharp-style.html](https://google.github.io/styleguide/csharp-style.html)
// | This guide, like any other style guide, is not a rulebook.
// | It is a starting point for your team to discuss and agree upon a common style.
// | There is no one-size-fits-all style guide. Pick what works best for your team and project.
// | While there is no single correct solution, this guide provides some conventions you may want to consider.
// | The most important thing is to be consistent. If you have a team, agree on a style guide and stick to it.
// | The goal of a style guide is to eliminate guesswork from coding conventions
// | and formatting issues, so you and your team can focus on solutions.
// | With that in mind, use this guide, Microsoft's, or Google's as inspiration
// | or even as a starting point for building your own style guide.
// | Skip, add, or customize these rules to create your own team style guide, then apply it consistently.

// **KEY PRINCIPLES**
// | Readability over brevity. Clarity is more important than the time saved by omitting a few vowels.
// | The goal is to make your code more readable, maintainable, and consistent.
// | Getting things like naming right from the start will save you time and effort later.
// | Especially when debugging and extending features.
// | Follow industry standards and conventions as much as possible. However, be pragmatic.
// | The needs of a 100+ person team are different from a 2-person team.
// | Following from the above, coding style standards are a living document.
// | It should evolve as the project progresses and the team grows.
// | As a beginner, it's important to learn the rules before breaking them.
// | As you gain experience, you can make informed decisions about when to deviate.
// | However, as a beginner, keep the guide concise.
// | The most important thing is to write working code before making it "clean".
// | Code style is personal preference. The most important thing is to be consistent.
// | If you have a team, agree on a style guide and stick to it.
// | Prefer names that read naturally and are readable.
// | For example: HorizontalAlignment rather than AlignmentHorizontal (reads more naturally in English).

// **NAMING**
// | This is worth repeating: readability over brevity.
// | Clarity is more important than the time saved by omitting a few vowels.
// | Pick meaningful names from the start to reduce refactoring later.
// | Variable names should be descriptive, clear, and unambiguous, as they represent a thing or state.
// | Use nouns when naming, unless the variable type is bool.
// | Prefix booleans with verbs to make their meaning more obvious. e.g., isDead, isWalking, hasDamageMultiplier.
// | Use meaningful names. Don't abbreviate (unless it's math or a universally recognized abbreviation). Your variable names should reveal their intent.
// | Choose identifier names that are easy to read.
// | For example, a property named HorizontalAlignment is more readable than AlignmentHorizontal.
// | Make type names unambiguous across namespaces and problem domains by avoiding generic terms or adding prefixes/suffixes.
// | (e.g., use 'PhysicsSolver', not 'Solver')

// **CASING AND PREFIXES:**
// | Use Pascal case (e.g., ExamplePlayerController, MaxHealth, etc.) unless otherwise noted.
// | For local/private variables and parameters, use camel case (e.g., examplePlayerController, maxHealth, etc.).
// | Avoid snake case, kebab case, Hungarian notation.
// | If the file contains a MonoBehaviour, the source file name must match.
// | Consider using prefixes for private member variables (m_), constants (k_), or static variables (s_),
// | so that names reveal more about the variable at a glance.
// | Alternatively, some guides suggest prefixing private member variables
// | with an underscore (_) to distinguish them from local variables.
// | Drop redundant initialization (i.e., don't write '= 0' for ints, '= null' for reference types, etc.)
// | as they are initialized to 0 or null by default.
// | Consistently specify (or omit) access level modifiers.
// | We recommend explicitly specifying private to make the access level clear and avoid any ambiguity.
// | Avoid redundant names: if your class is named Player, you don't need member variables named PlayerScore or PlayerTarget.
// | Simplify them to Score or Target.

// **FORMATTING:**
// | Choose K&R (opening brace on same line) or Allman (opening brace on next line) brace style.
// | Readability is key. Keep lines short where possible. Consider horizontal whitespace.
// | You can also define a standard maximum line width in your style guide (some prefer fewer than 120 characters).
// | Split long lines into shorter statements rather than letting them overflow.
// | Use a single space before flow control conditions, e.g.: while (x == y).
// | Avoid spaces inside parentheses, e.g.: x = dataArray[index].

// SPACING:
// | Use a single space after commas between function arguments, e.g.: CollectItem(myObject, 0, 1);
// | Don't add spaces after parentheses and function arguments, e.g.: CollectItem(myObject, 0, 1);
// | Don't use spaces between function name and parentheses, e.g.: DropPowerUp(myPrefab, 0, 1);
// | Use vertical spacing (extra blank lines) for visual separation, e.g.:
```
  for (int i = 0; i < 100; i++)
  {
      DoSomething(i);
  }
```
Use one variable declaration per line in most cases. While less compact, it enhances readability.
// | Don't use spaces between function name and parentheses, e.g.: DropPowerUp(myPrefab, 0, 1);
// | Use a single space before flow control conditions, and a space on each side of comparison operators, e.g.: if (x == y).

// **COMMENTS:**
// | Add comments when necessary. That is, when the code itself cannot explain, and good naming alone isn't sufficient.
// | However, if you need to add a comment to explain tangled, complex logic, consider refactoring your code to make it more obvious.
// | Good naming eliminates guesswork. Consider renaming before commenting.
// | Comments shouldn't just answer "what" or "how", but fill the gap and tell us "why".
// | Use // comments to keep explanations next to the logic.
// | If your Inspector fields need explanation, use Tooltip on serialized fields instead of comments.
// | Rather than using Region, treat it as a "code smell" indicating your class is too large and needs refactoring.
// | For legal information or licensing, use external reference links to save space.
// | Use XML summary tags before public methods or functions for documentation/IntelliSense output.
// | Avoid author attribution, e.g.: // Created by, // Modified by, etc. Use version control to track changes.
// | Documenting "why" is far more important than documenting "what" or "how".

// **CLASS ORGANIZATION:**
// | Organize your class in this order: Fields, Properties, Events,
// | MonoBehaviour methods (Awake, Start, OnEnable, OnDisable, OnDestroy, etc.), Public methods, Private methods, Other classes.
// | Using #region is generally discouraged as it hides complexity and makes code harder to read.

---
## Code Example
```csharp
// **USING LINES:**
// | Keep using lines at the top of the file.
// | Remove unused lines.
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

// NAMESPACES:
// | Use namespaces to ensure your classes, interfaces, enums, etc.
// | don't conflict with existing content from other namespaces or the global namespace.
// | Use Pascal case, no special characters or underscores.
// | Add using lines at the top to avoid repeatedly typing the namespace.
// | Use the dot (.) operator to create sub-namespaces, e.g.: MyApplication.GameFlow, MyApplication.AI, etc.
// | Some suggest namespaces should reflect the project's folder structure for logical grouping.
// | Remove unused 'usings' beyond the "minimum required set".

namespace UnityCSharpStyleSheetExample
{
    // ENUMS:
    // | Use enums when an object or action can only have one value at a time.
    // | Enum names and values use Pascal case.
    // | Enum names use singular nouns, as they represent a single value from a set of possibilities.
    // | They should not have prefixes or suffixes.
    // | You can place public enums outside the class to make them global.
    public enum Direction
    {
        North,
        South,
        East,
        West,
    }

    // FLAG ENUM:
    // | Use flag enums when multiple values can be selected simultaneously, representing option combinations and enabling bitwise operations.
    // | Use plural nouns to indicate the possibility of multiple selections (e.g., AttackModes).
    // | No prefixes or suffixes.
    // | Use column alignment for binary values.
    // | Alternatively, consider using the 1 << bitnum style.
    [Flags]
    public enum AttackModes
    {
        // Decimal                             // Binary
        None = 0,                          // 000000
        Melee = 1,                         // 000001
        Ranged = 2,                        // 000010
        Special = 4,                       // 000100
        MeleeAndSpecial = Melee | Special  // 000101
    }

    // INTERFACES:
    // | When unrelated classes need to share common functionality but implement it differently, interfaces let you define a common contract.
    // | Prefix interface names with uppercase I.
    // | Follow with an adjective phrase describing the capability.
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


    // CLASSES or STRUCTS:
    // | Use Pascal case.
    // | Name with nouns or noun phrases. This distinguishes type names from methods, which use verb phrases.
    // | Avoid prefixes.
    // | One MonoBehaviour per file. If the file contains a MonoBehaviour, the source file name must match.
    public class StyleExample : MonoBehaviour
    {
        // FIELDS:
        // | Avoid special characters (backslashes, symbols, Unicode characters); they interfere with command-line tools.
        // | Use nouns for names, but prefix booleans with verbs.
        // | Use meaningful names. Make names searchable and pronounceable. Don't abbreviate (unless it's math).
        // | Public fields use Pascal case. Private variables use camel case.
        // | Specify (or omit) the default private access modifier, but be consistent.
        // | If you feel this doesn't negatively impact ambiguity or readability, we suggest omitting the implicit and redundant part (e.g., private) for brevity.
        // | There are many opinions on prefix usage.
        // | Pick what works best for you and your team and stay consistent with your style guide.
        // | You can prefix private fields with underscore (_) to distinguish them from local variables.
        // | You can also use more explicit prefixes: m_ = member variable, s_ = static, k_ = constant.
        // | Use readability over brevity as a guiding principle.

        // Some prefer no prefix for private fields, specifying the access modifier as follows
        private int elapsedTimeInHours;

        // Some prefer the underscore prefix for private fields.
        int _elapsedTimeInDays;

        // We recommend using explicit prefixes: m_ = member variable, for clarity and
        // readability over saving a few keystrokes. However, we suggest omitting "private" as it's implicitly present for brevity.
        // Whichever style you choose, be consistent.
        int m_elapsedTimeInSeconds;

        // Use [SerializeField] attribute to show private fields in the Inspector.
        // Booleans ask a question that can be answered with true or false.
        [SerializeField] bool m_isPlayerDead;

        // Static fields: prefix with s_
        static int s_sharedCount;

        // Constants: prefix with k_
        const int k_maxCount = 100;

        // This groups data from the custom PlayerStats class in the Inspector.
        [Serializable] PlayerStats m_stats;

        // Use Range attribute to set minimum and maximum values.
        // This constrains the value to a range and creates a slider in the Inspector.
        [Range(0f, 1f)] [SerializeField] float m_rangedStat;

        // Tooltip can replace comments on serialized fields and serves a dual purpose.
        [Tooltip("Another stat for the player.")]
        [SerializeField] float m_anotherStat;

        // PROPERTIES:
        // | Prefer over public fields.
        // | Pascal case, no special characters.
        // | Use expression-bodied properties to shorten, but choose your preferred format.
        // | e.g., use expression-bodied for read-only properties but { get; set; } for others.
        // | Use Auto-Implemented Properties for public properties without a backing field.
        // | While you can also use functions to expose private data, properties are generally recommended.
        // | For get or set operations involving complex logic or computation, use methods instead of properties.

        // Private backing field
        int m_maxHealth;

        // Read-only, returns the backing field
        public int MaxHealthReadOnly => m_maxHealth;

        // Equivalent to:
        // public int MaxHealth { get; private set; }

        // Explicitly implementing getter and setter
        public int MaxHealth
        {
            get => m_maxHealth;
            set => m_maxHealth = value;
        }

        // Write-only (not using backing field)
        public int Health { private get; set; }

        // Write-only, without an explicit setter
        public void SetMaxHealth(int newMaxValue) => m_maxHealth = newMaxValue;

        // Auto-implemented property, no backing field
        public string DescriptionName { get; set; } = "Fireball";

        // EVENTS:
        // | Name with verb phrases.
        // | Present participle = "before", past participle = "after".
        // | Most events use System.Action delegate (accepting 0 to 16 parameters).
        // | Only define custom EventArgs when necessary (using System.EventArgs or a custom struct).
        // | Alternatively, use System.EventHandler; choose one and apply consistently.
        // | Choose a naming scheme for events, event handlers (subscriber/observer),
        // | and event-raising methods (publisher/subject).
        // | e.g.: event/action = "OpeningDoor", event-raising method = "OnDoorOpened", event handler = "MySubject_DoorOpened"

        // Event before
        public event Action OpeningDoor;

        // Event after
        public event Action DoorOpened;

        // Event with int parameter
        public event Action<int> PointsScored;

        // Custom event with custom EventArgs
        public event Action<CustomEventArgs> ThingHappened;

        // These are event-raising methods, e.g., OnDoorOpened, OnPointsScored, etc.
        // Prefix event-raising methods (in the subject) with "On".
        // Alternatively, event handlers e.g., MySubject_DoorOpened().

        public void OnDoorOpened()
        {
            DoorOpened?.Invoke();
        }

        public void OnPointsScored(int points)
        {
            PointsScored?.Invoke(points);
        }

        // This is a custom EventArg composed of a struct.
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

        // METHODS:
        // | While "function" and "method" are often used interchangeably, in Unity development "method" is the correct term
        // | because in C# you cannot write a function without incorporating it into a class.
        // | Start method names with a verb or verb phrase to show action. Add context as needed. e.g.: GetDirection, FindTarget, etc.
        // | Methods returning bool should ask a question: like boolean variables themselves.
        // | If a method returns a true/false condition, prefix the method with a verb.
        // | This phrases them as questions, e.g.: IsGameOver, HasStartedTurn.
        // | Parameters use camel case. Format parameters passed to methods like local variables.

        // | Some general tips about methods:
        // | Avoid long methods. If a method is too long, consider splitting it into smaller methods.
        // | Avoid methods with too many parameters. If a method has more than three parameters,
        // | consider using a class or struct to group them.
        // | Avoid excessive overloading: you can generate endless permutations of method overloads.
        // | Avoid side effects: a method only needs to do what its name advertises.
        // | A good method name reflects what it does.
        // | Avoid making a method work in multiple different modes based on flags.
        // | Instead, create two methods with different names, e.g., GetAngleInDegrees and GetAngleInRadians.

        // Methods start with a verb.
        public void SetInitialPosition(float x, float y, float z)
        {
            transform.position = new Vector3(x, y, z);
        }

        // Ask a question when the method returns bool.
        public bool IsNewPosition(Vector3 newPosition)
        {
            return (transform.position == newPosition);
        }

        void FormatExamples(int someExpression)
        {
            // VAR:
            // | While avoiding ambiguity and always seeking to improve readability,
            // | you can use var when the type is clearly apparent from context.
            // | With good naming, ambiguity is no longer an issue, as the variable name already conveys intent.
            // | Using var makes refactoring simpler, as it abstracts the concrete type,
            // | reducing the places that need updating when types change.
            // | In foreach loops, var ensures the iteration variable matches the type provided by the enumerator.
            // | If you explicitly declare a mismatched type, the compiler may allow it, leading to runtime errors.

            var powerUps = new List<PlayerStats>();
            var dict = new Dictionary<string, List<GameObject>>();

            // SWITCH STATEMENT:
            // | Generally recommended to replace longer if-else chains with switch for better readability.
            // | Formatting may vary. Choose one for your style guide and follow it.
            // | This example indents each case with its break below.
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


            // BRACES:
            // | Don't omit braces where possible, even for single-line statements.
            // | Or avoid single-line statements altogether to aid debugging.
            // | Keep braces in nested multiline statements.

            // This single-line statement keeps braces...
            for (int i = 0; i < 100; i++) { DoSomething(i); }

            // ... but this is more readable and generally better for debugging.
            for (int i = 0; i < 100; i++)
            {
                DoSomething(i);
            }

            // Separate statements for readability.
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

    // OTHER CLASSES:
    // | Define as many additional helper/non-MonoBehaviour classes as needed in the file.
    // | This is a serializable class that groups fields in the Inspector.
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
