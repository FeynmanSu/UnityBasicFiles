# Unity C# Style Guide
A project style guide. This guide is not a rigid rulebook — feel free to propose suggestions and discuss them with the team.

> **The most important principle: be consistent.** Follow this guide's recommendations and apply them consistently.

---
## Key Principles
  * **Readability over brevity**: Clarity is more important than the time saved by omitting a few vowels.
  * **Clear and consistent goals**: The goal is to make code easier to read, maintain, and keep consistent.
  * **Start right from the beginning**: Naming things correctly from the start saves time debugging and extending features later.
  * **Be pragmatic**: While you should follow industry standards as much as possible, consider your team size. A 100-person team and a 2-person team have different needs.
  * **Living document**: Style standards are living documents that should evolve as the project progresses and the team grows.
  * **Learn before you break**: As a beginner, learn the rules before breaking them. Keep the guide concise — the most important thing is to write working code first, then worry about "clean" code.
    * Shu-Ha-Ri philosophy
  * **Natural pronunciation**: Prefer names that read naturally in English. For example: `HorizontalAlignment` is better than `AlignmentHorizontal`.

---
## Naming
  * **Variable naming**: Should be descriptive, clear, and unambiguous. Except for booleans, use **nouns**.
  * **Booleans**: Names should start with a **verb** (e.g., `isDead`, `isWalking`, `hasDamageMultiplier`) to make their meaning more obvious.
  * **No abbreviations**: Unless it's a math operation or a universally recognized abbreviation, variable names should reveal their intent.
  * **Avoid generic words**: To prevent ambiguity across namespaces or problem domains, avoid generic terms or add prefixes/suffixes (e.g., use `PhysicsSolver` instead of `Solver`).

---
## Casing and Prefixes
  * **Pascal Case**: Used for class names, public fields, and methods (e.g., `ExamplePlayerController`, `MaxHealth`).
  * **Camel Case**: Used for local variables, private variables, and parameters (e.g., `examplePlayerController`, `maxHealth`).
  * **Avoid**: snake_case, kebab-case, or Hungarian notation.
  * **Member variable prefixes**:
    * Private member variables: `m_` or underscore `_`.
    * Constants: `k_`.
    * Static variables: `s_`.
  * **Simplify initialization**: Omit redundant initialization (e.g., `int` defaults to `0`, reference types default to `null`).
  * **Access modifiers**: Explicitly mark `private` to avoid ambiguity, or consistently omit based on team consensus.
  * **Eliminate redundancy**: If the class is named `Player`, the variable should be `Score`, not `PlayerScore`.

---
## Formatting and Spacing
  * **Brace style**: Use Allman (opening brace on a new line) style.
    * Always use braces even for single-line statements, with the opening brace on a new line.
  * **Line width limit**: Keep lines of code short when possible, but don't over-wrap.
  * **Whitespace usage**:
    * Add a space after commas.
      * `CollectItem(myObject, 0, 1);`
    * Add a space before flow control conditions like `if` or `while`.
      * `for (int i = 0; i < 100; i++)`
    * No spaces inside parentheses; no space between function name and parentheses.
      * `DropPowerUp(myPrefab, 0, 1);`
  * **Variable declarations**: In most cases, declare only one variable per line for readability.

---
## Comments and Organization
  * **Explain "why"**: Comments should explain "why", not just "what" or "how". If the code is too complex and needs a comment to explain, consider refactoring first.
  * **Self-documenting**: Prefer good naming over comments.
  * **Inspector tooltips**: For serialized fields, use `[Tooltip]` instead of comments so they're visible in the Unity Editor.
  * **Class ordering**: Recommended order: Fields -> Properties -> Events -> MonoBehaviour methods (Awake, Start, etc.) -> Public methods -> Private methods -> Nested classes.
  * **Prohibited practices**: Avoid `#region` (this usually indicates the class is too large). Avoid author annotations (use version control to track changes).

---
## Code Example
```csharp
// Remove unused usings
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
    public class StyleExample : MonoBehaviour
    {
        // Fields: avoid special characters; prefix booleans with verbs

        // Private fields: recommend m_ prefix to distinguish from local variables
        int m_elapsedTimeInSeconds;

        // Use [SerializeField] to expose private variables in the Inspector
        [SerializeField] bool m_isPlayerDead;

        // Static fields: use s_ prefix
        static int s_sharedCount;

        // Constants: use k_ prefix
        const int k_maxCount = 100;

        // Use Range attribute to constrain values in the Inspector
        [SerializeField, Range(0f, 1f)] float m_rangedStat;

        // Tooltip serves as both a comment and an editor hint
        [Tooltip("Another stat for the player.")]
        [SerializeField] float m_anotherStat;

        // Properties: prefer over public fields

        // Private backing field
        int m_maxHealth;

        // Read-only property using expression-bodied syntax
        public int MaxHealthReadOnly => m_maxHealth;

        // Auto-implemented property
        public string DescriptionName { get; set; } = "Fireball";

        // Events: name with verb phrases. Present participle = "before", past participle = "after"
        public event Action OpeningDoor; // Opening (before)
        public event Action DoorOpened;  // Opened (after)

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

        void FormatExamples(int someExpression)
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

        void DoSomething(int x)
        {
            // ..
        }
    }

    // Other structures: used for grouping data
    [Serializable]
    public struct PlayerStats
    {
        public int MovementSpeed;
        public int HitPoints;
        public bool HasHealthPotion;
    }
}

```
