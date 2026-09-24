# Skill-writing brief (read fully before writing)

## Goal
Turn the distilled expert notes into one skill of a TEAM that makes Claude Opus 5.5, driving Unreal Engine 5.8 on macOS (Apple Silicon), work like the experts in the sources. Each skill is written so it can later become the system prompt of one agent (a persona) in a team led by `unreal-expert`. Opus already knows textbook Unreal: read the baseline answers in `tests/baseline/answers.md` (scenarios in `tests/scenarios.md`) to see where it is generic, UE4-era, wrong for 5.8, not executable, or blind to what an agent without a mouse can do. The skill carries the EXPERT DELTA plus agent procedures, not a textbook.

Unreal Engine is NOT installed on this Mac yet (Epic sign-in pending). Mark every snippet "not yet run in Unreal"; test pure-Python parts offline now.

## The team
| Skill | Persona | Owns |
|---|---|---|
| unreal-expert | lead / technical director | execution channels (Epic's 5.8 MCP server, Editor Python in a running editor, headless `UnrealEditor-Cmd -run=pythonscript`, Remote Control API, console commands), the expert loop, review and audit toolkit, 5.8 version traps, macOS specifics, handoffs |
| unreal-world-building | environment and level artist | project and level setup, World Partition, OFPA, Data Layers, Level Instances, HLOD, landscape, foliage, PCG, Nanite, modular kits |
| unreal-materials | technical artist (shading) | master materials and instances, Substrate, layering, landscape materials, RVT, decals, translucency, stylized and toon shading, shader cost and permutations, texture settings |
| unreal-lighting-rendering | lighter | physically based lighting and exposure, Lumen, MegaLights, VSM, sky and fog, post process and color, path tracer, Movie Render Graph for stills |
| unreal-gameplay | gameplay programmer / designer | Blueprint architecture and real costs, gameplay framework, Enhanced Input, GAS, AI (StateTree, Behavior Trees, EQS, Mass), UMG and CommonUI, replication, save state, C++ vs Blueprint |
| unreal-animation | technical animator | skeletal import, Animation Blueprints, Motion Matching, IK Rig and Retargeter, Control Rig, root motion, MetaHuman, ML Deformer, cloth, animation budgets |
| unreal-cinematics | cinematics artist | Sequencer structure, cameras and lenses, shot lighting, Movie Render Graph output, color pipeline, Sequencer Python automation |
| unreal-vfx | real-time VFX artist | Niagara (systems, sim stages, fluids, lightweight emitters, data channels, scalability), VFX art principles, Chaos destruction and physics |
| unreal-pipeline-automation | pipeline / tools TD | Editor Python tools, Interchange import pipelines, naming and validation, automation tests, command-line cook and package, source control, CI |
| unreal-performance | performance engineer | profiling (stat, Insights, TraceQuery), budgets, hitches (PSO, GC, loads, physics), scalability and device profiles, Lumen/Nanite/VSM/Niagara cost, packaging for performance |

Each domain skill states its handoffs explicitly ("receives: ..., delivers: ... to unreal-lighting-rendering"). Sister teams: maya-expert, zbrush-expert, blender-expert (installed in `~/.claude/skills/`).

## Shared toolkit contract (`skills/unreal-expert/scripts/`, written by the lead agent in parallel)
Use these names; do not reimplement them. If you need more, write `scripts/ue_<domain>.py` in your own skill.
- `ue_env.py`: `find_engine()` (UnrealEditor and UnrealEditor-Cmd on macOS, env `UE_ROOT` override), `find_project(path)`.
- `ue_run.py`: `run_python(uproject, script, args=None, map=None, timeout=1800)` runs a Python job headless through `UnrealEditor-Cmd <uproject> -run=pythonscript -script=...` and returns the dict printed on the job's last line `UE_RESULT {json}`; `run_commandlet(uproject, name, args)`; `result(obj)` helper for jobs.
- `ue_remote.py`: `RemoteControl(host="127.0.0.1", port=30010)` with `.call(object_path, function, params)`, `.get_property(...)`, `.set_property(...)`; `PythonRemote()` with `.exec(code)` using the editor's Python remote execution; notes on using Epic's MCP server as the preferred live channel.
- `ue_review.py`: in-editor `screenshot(path, width, height)` and `render_still(level_sequence_or_camera, out_dir, preset)` (Movie Render Graph), plus offline `image_checks(path)` returning luminance histogram, clipped fraction, crushed fraction, mean EV-like value, contrast between centre and surround, and an "all white or all black" flag (the MCP talk showed an agent approve an all-white frame).
- `ue_audit.py`: in-editor `audit_assets(paths, rules)` (naming, texture sizes and compression, Nanite, collision, LODs, material instance parents, references) and `verdict(report, profile)`.
- `ue_stat.py`: offline parsers for `stat unit` style logs, CSV profiler files and TraceQuery JSONL, with `budget_check(frames, target_ms)`.

## Inputs
- Your domain's notes and digests in `notes/` (read ALL notes of your folders, not only digests). Mapping: world-building = world-building + pcg + editor; materials = materials + shading; lighting-rendering = lighting + rendering + post-process; gameplay = gameplay-framework + blueprints + ai + ui + cpp; animation = animation + characters + rigging; cinematics = sequencer + cinematography + virtual-production; vfx = niagara + destruction + physics; pipeline-automation = pipeline + python + editor-tools + editor; performance = optimization + profiling + packaging.
- Version changes: `sources/unreal-version-deltas.md` (authoritative for 5.8 facts).
- Official docs: `sources/docs/*.md`.

## Output: `skills/unreal-<domain>/`
```
SKILL.md                    core playbook, 1,200 to 2,300 words
references/expert-notes.md  principles and judgment by expert, with source + timestamp
references/procedures.md    agent procedures (full code), each with its test path and status
references/critique.md      the rubric the agent uses to judge its own output
references/gui-paths.md     editors, menus and settings for the same procedures (for a computer-use agent)
references/sources.md       every source: expert, credential, URL, what it is best for, best timestamps
scripts/ue_<domain>.py      reusable code (in-editor parts and offline-testable parts)
```

### SKILL.md shape
```
---
name: unreal-<domain>
description: Use when <triggers: tasks, symptoms, keywords>. Third person, starts with "Use when", no workflow summary, under about 600 characters.
---
# <Title>
<2 to 3 sentences: what expert level means here and the core stance.>
**REQUIRED BACKGROUND:** unreal-expert (channels, review loop, 5.8 traps).
## Stance (the expert delta)      5 to 8 attributed, non-obvious rules with reasons
## Establish first                 inputs that change the plan (platform, frame budget, style, scale) and defaults
## Workflow                        numbered stages; each: goal, how (Python, cvar, MCP tool, or GUI), GATE (measurable + visual)
## Numbers                         compact table with what each value is relative to
## Quality gates                   measurable (stat, Insights, validators, audits, image checks) and visual
## Common mistakes                 table: mistake | what it looks like | fix
## Handoffs                        receives from / delivers to which teammate, in which form
## UE 5.8 notes                    only the version traps relevant here
## References                      one line per reference file
```

## Rules
- Be honest about what an agent can and cannot author: graph editors (Blueprint, material, Niagara, PCG, Control Rig) are only partly scriptable; give the substitute (template assets duplicated and parameterized, material instances, data assets, PCG parameters, MCP toolsets, Control Rig's Copy Python Script).
- Attribute expert claims; mark your own additions [added]. Never invent numbers. When experts disagree, give the deciding condition (console vs PC vs mobile, gameplay vs cinematic).
- Descriptions trigger discovery: include the words users say ("Lumen is noisy", "optimize to 60 fps", "import FBX from Maya", "PCG forest", "Niagara fireball", "retarget animations").
- Link other skills by name only. No em dashes. Never run `rm` (move files to `archive/`).
- Write only inside `skills/unreal-<domain>/` and `tests/code/unreal-<domain>/` (temp files in `archive/tests/`). Offline tests run with system python3; byte-compile everything.
- Final reply (under 200 words): files written, SKILL.md word count, what was tested offline, open questions for the first run in Unreal.
