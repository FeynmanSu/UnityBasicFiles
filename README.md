# UnityBasicFiles

A ready-to-use coding standard and Claude Code configuration template for **Unity 6.3 (6000.3.x+) URP** cross-platform game development.

Drop these files into a Unity project to get consistent coding standards, auto-applied rules for C# and HLSL editing, and a specialized Claude Code skill for Render Graph authoring, shader optimization, and performance tuning.

## What's Included

```
.
├── claude.md                    # Main project spec (always loaded by Claude Code)
├── .gitignore                   # Unity 6.3 URP-tailored gitignore
│
└── .claude/
    ├── settings.json            # Claude Code permission settings
    ├── settings.local.json      # Local permission overrides
    │
    ├── rules/
    │   ├── csharp-unity.md      # Auto-applied when editing Assets/Scripts/**/*.cs
    │   └── hlsl-urp.md          # Auto-applied when editing Assets/Shaders/**/*.hlsl|.shader
    │
    └── skills/
        └── unity-6-urp/
            ├── SKILL.md         # Specialized skill for URP development tasks
            └── references/
                ├── Unity_C_Sharp_Style_Guide.md
                └── Unity_Cowork_and_Coding_Standard.md
```

## Target Specifications

| Spec | Value |
|------|-------|
| Engine | Unity 6.3 (6000.3.x+) |
| Render Pipeline | Universal Render Pipeline (URP) |
| Rendering | Render Graph + GPU Resident Drawer |
| Scripting | C# 12 / .NET 8 |
| Platforms | PC, Android, iOS, Console (optional) |

## How It Works

### claude.md (Project Spec)

Loaded into every Claude Code conversation. Defines:

- Technical specifications (Render Graph, Shader authoring, Input System)
- Cross-platform performance budgets (draw calls, tri counts, FPS targets per platform)
- Code style guidelines (naming conventions, structural patterns, prohibited patterns)
- Asset pipeline rules (Addressables, texture formats, audio settings)
- URP Asset tiered configuration (PC High / PC Low / Mobile)

### Auto-Applied Rules

These activate automatically based on file path — no manual invocation needed.

**`csharp-unity.md`** — Enforced on `Assets/Scripts/**/*.cs`:
- Naming: `m_camelCase` (private), `s_camelCase` (static), `k_PascalCase` (constant), `PascalCase` (public)
- Prohibited: `GetComponent` in Update, `Camera.main`, `Resources.Load`, `Coroutine`, per-frame allocations
- Required: `async/await` with UniTask, Addressables for asset loading, `sealed` on non-inheritable classes

**`hlsl-urp.md`** — Enforced on `Assets/Shaders/**/*.hlsl` and `*.shader`:
- SRP Batcher compliance (`CBUFFER_START/CBUFFER_END`)
- `TEXTURE2D`/`SAMPLER` macros only (no `sampler2D`)
- Mobile precision rules (`half` for color/lighting, `float2` for UVs)
- No `discard`/`clip()` on mobile opaque objects
- Render Graph integration patterns (`SetGlobalTextureAfterPass`, not `SetGlobalTexture`)

### Unity 6 URP Skill

Invoked when writing new features, optimizing code, authoring shaders, or tuning URP performance. Provides:

- Render Graph pass authoring patterns (`IRenderGraphRecorder`, `TextureHandle`)
- Cross-platform shader best practices
- Performance optimization workflows (GPU Resident Drawer, Forward+, LOD, occlusion culling)
- Debugging guidance (Frame Debugger, RenderDoc, platform profilers)

### Reference Documents

Two comprehensive style guides available as context for the skill:

- **Unity C# Style Guide** — Naming, formatting, comments, code examples
- **Clean Coding and Coding Style Guide** — Team collaboration standards, SOLID/DRY/KISS principles, code smell detection, UI Toolkit naming (BEM)

## Performance Budgets

| Platform | FPS | Draw Calls | Triangles/Frame | Verts/Object |
|----------|-----|------------|-----------------|--------------|
| PC High | 60 | ≤ 500 | ≤ 2M | Unlimited |
| PC Low | 60 | ≤ 200 | ≤ 500K | ≤ 50K |
| Android | 30 | ≤ 100 | ≤ 200K | ≤ 10K |
| iOS | 60 | ≤ 150 | ≤ 300K | ≤ 15K |

## Usage

1. Copy the contents of this repository into the root of your Unity 6.3 URP project.
2. Open the project with [Claude Code](https://docs.anthropic.com/en/docs/claude-code).
3. Start working — rules auto-apply when you edit C# or shader files, and the skill activates for URP-related tasks.

## Naming Convention Quick Reference

| Type | Convention | Example |
|------|-----------|---------|
| Classes, Methods, Properties | `PascalCase` | `TowerController` |
| Private Fields | `m_camelCase` | `m_currentHealth` |
| Serialized Fields | `[SerializeField] private` + `m_camelCase` | `m_moveSpeed` |
| Constants | `k_PascalCase` | `k_MaxTowerCount` |
| Static Fields | `s_camelCase` | `s_instanceCount` |
| Interfaces | `I` prefix | `IDamageable` |
| Shader Properties | `_PascalCase` | `_BaseColor` |

## Key Prohibited Patterns

- `GetComponent<T>()` / `FindObjectOfType<T>()` / `GameObject.Find()` in `Update()` or `FixedUpdate()`
- `Camera.main` (calls `FindObjectOfType` internally)
- `Resources.Load()` (use Addressables)
- `Coroutine` (use `async/await` with UniTask)
- `Graphics.Blit()` (use `Blitter.BlitCameraTexture()`)
- `ScriptableRenderPass.Execute` (use `IRenderGraphRecorder.RecordRenderGraph`)
- Legacy `Input.GetKeyDown` (use Input System Package)
- `sampler2D` in shaders (use `TEXTURE2D` / `SAMPLER` macros)

## License

This project provides coding standards and configuration files. Adapt them to fit your team's needs.
