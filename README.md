# Unreal Engine Expert Skills v0.1

Agent skills that let Claude (Claude Code) or Codex drive Unreal Engine 5.8 the way the experts in the best tutorials, conference talks and official documentation do. Built on 2026-09-24 with the same method as [Blender Expert Skills](https://github.com/edemaistre/blender-expert-skills), [Maya Expert Skills](https://github.com/edemaistre/maya-expert-skills) and [ZBrush Expert Skills](https://github.com/edemaistre/zbrush-expert-skills): transcripts of top-rated videos, cited expert notes, then skills with tools and review rubrics, graded blind against the same model without them. Start with the lead skill, `unreal-expert`.

## Skills

| Skill | Use it when |
|---|---|
| `unreal-animation` | a UE5 task involves characters in motion: "import FBX from Maya" with a skeleton, "retarget animations", IK Rig, IK Retargeter, feet sinking or floati... |
| `unreal-cinematics` | making a trailer, cutscene, cinematic or short film in Unreal Engine 5.8 with Sequencer: shot lists, master and shot sequences, cameras, lenses, depth... |
| `unreal-expert` | an agent drives Unreal Engine 5.8 on a Mac for any task: Editor Python (the unreal module), headless UnrealEditor-Cmd -run=pythonscript jobs, Epic's U... |
| `unreal-gameplay` | building or debugging UE5 gameplay: a character with double jump or dash, abilities with cooldowns, enemy AI that patrols, chases and attacks, a healt... |
| `unreal-lighting-rendering` | lighting or rendering in Unreal Engine 5.8: physical light units and exposure (EV100, lux, lumens), Lumen GI and reflections, MegaLights, virtual shad... |
| `unreal-materials` | a UE5 task involves materials or textures: a master material and instances for a kit, Substrate or legacy, Blendable vs Adaptive GBuffer, glass, emiss... |
| `unreal-performance` | an Unreal Engine 5.8 project misses its frame rate or stutters: "optimize to 60 fps", "we run at 38 fps", CPU or GPU bound, game or render thread too ... |
| `unreal-pipeline-automation` | importing FBX, USD, OBJ or glTF from Maya, ZBrush or Blender into Unreal Engine 5.8 in bulk or reimporting changed files, writing Editor Python tools,... |
| `unreal-vfx` | a UE5 task involves Niagara or Chaos destruction: a Niagara fireball, projectile trail, impact explosion, muzzle flash, spell, smoke, sparks, debris, ... |
| `unreal-world-building` | building or fixing an Unreal Engine 5 level or open world: World Partition, OFPA, Data Layers, Level Instances, Packed Level Actors, HLOD, landscape s... |

## Status at v0.1

- 10 skills (the lead `unreal-expert` and nine specialists) written from 115 expert videos (77 h) and 82 official documentation pages, each with references and an offline-tested `ue_<domain>.py` module. The lead carries the shared toolkit (`ue_env`, `ue_run`, `ue_remote`, `ue_review`, `ue_audit`, `ue_stat`).
- Blind-graded written scenarios (round 1): 51 % without the skills, 95 % with them (69/135 vs 128/135); all 9 scenarios improved. One grader per scenario: a direction, not a statistic. These scores were measured before the refactor that v0.1 includes; round 2 is planned after the first run in Unreal.
- Not verified inside Unreal Engine: the engine was not installed when v0.1 was cut (Epic sign-in pending), so nothing has run in the engine. 1,534 offline checks pass across the 10 modules, with system Python and fake `unreal` modules only. Every Unreal API name, cvar and command in the skills is marked "not yet run in Unreal"; C++ builds and the MCP toolsets are untested. Treat the in-engine code paths as unverified until a first live run (`tests/OPEN_ISSUES.md` in the build project).

## Requirements

Unreal Engine 5.8 on macOS Apple Silicon. The lead skill `unreal-expert` explains the execution channels: Epic's experimental Unreal MCP server, Editor Python in a running editor (Python Remote Execution), headless `UnrealEditor-Cmd -run=pythonscript` jobs, the Remote Control API, console commands, commandlets and UAT. The offline layers of the modules run with system Python 3.

## Install

```bash
git clone https://github.com/edemaistre/unreal-expert-skills.git ~/unreal-expert-skills
for d in ~/unreal-expert-skills/unreal-*; do
  ln -sfn "$d" ~/.claude/skills/"$(basename "$d")"     # Claude Code
  ln -sfn "$d" ~/.agents/skills/"$(basename "$d")"     # Codex and other agents
done
```
Update with `git -C ~/unreal-expert-skills pull`. For a cloud agent that only sees a project repo, copy the `unreal-*` folders into that project's `.claude/skills/` (Claude Code) or `.agents/skills/` (Codex). Keep the folders side by side: the domain skills import the lead skill's `scripts/`.

## Version

v0.1, released 2026-09-24. Each `SKILL.md` carries `metadata.version: "0.1"` and a visible version line; each script exposes `__version__ = "0.1"`. History in `CHANGELOG.md`; git tag `v0.1`. Mentions of v0.1, v0.2, v1 or v2 inside the references and code comments name the internal drafts of 2026-09-24 (see `CHANGELOG.md`).

Paths such as `tests/code/...`, `notes/...`, `sources/...` or `~/Developer/pro/...` inside the skills point to the build project (tests, notes, sources) on the author's machine; they are provenance only.
