# Unreal Engine Expert Skills v0.2.1

Agent skills that let Claude (Claude Code) or Codex drive Unreal Engine 5.8 the way the experts in the best tutorials, conference talks and official documentation do. Built on 2026-09-24 with the same method as [Blender Expert Skills](https://github.com/edemaistre/blender-expert-skills), [Maya Expert Skills](https://github.com/edemaistre/maya-expert-skills) and [ZBrush Expert Skills](https://github.com/edemaistre/zbrush-expert-skills): transcripts of top-rated videos, cited expert notes, then skills with tools and review rubrics, graded blind against the same model without them. Start with the lead skill, `unreal-expert`. Generated assets can come from the Scenario MCP or from the [Scenario for Unreal plugin](https://github.com/scenario-labs/unreal-plugin), whose `scenario-unreal` skill drives its local MCP server; these skills take over once the files are in the project.

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

## Status at v0.2 (unchanged in v0.2.1)

- 10 skills (the lead `unreal-expert` and nine specialists) written from 115 expert videos (77 h) and 82 official documentation pages, each with references and an offline-tested `ue_<domain>.py` module. The lead carries the shared toolkit (`ue_env`, `ue_run`, `ue_remote`, `ue_mcp`, `ue_review`, `ue_audit`, `ue_stat`).
- Run in Unreal Engine 5.8.3 on macOS (Apple silicon): the MCP recipes of every domain, the in-editor helpers, headless and offscreen editor jobs. Each `SKILL.md` has a status line naming what ran and what did not yet run.
- Built around Epic's Unreal MCP toolsets: every specialist has `references/mcp-<domain>.md` with the argument JSON of the calls it needs, and the lead explains the channels and batching through `execute_tool_script`.
- Measured with an arena (the same agent, Claude Sonnet 5.5 with Epic's Unreal MCP, on 14 tasks, no skills against v0.1 against v0.2): on the 10 development tasks the mean score is 0.89, 0.94 and 0.98, at $0.44, $0.50 and $0.33 per task; on the 4 held-out tasks the score ties (0.96, 0.95, 0.95) and v0.2 costs $0.51 against $0.66 without skills. One or two trials per cell and one grader, and the v0.2 author saw the development failures: a direction, not a statistic. Details and limits in the CHANGELOG.
- Not verified: Unreal 5.6 and 5.7, Windows and Linux, an Opus agent, the items each `SKILL.md` marks as not yet run (for example a Movie Render Graph render end to end, Motion Matching, packaged builds).

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

v0.2.1, 2026-10-22. Each `SKILL.md` carries `metadata.version: "0.2.1"` and a visible version line; each script exposes `__version__ = "0.2.1"`. History in `CHANGELOG.md`; git tag `v0.2.1`. Mentions of v0.1, v0.2, v1 or v2 inside the references and code comments name earlier drafts; the CHANGELOG explains them.

Paths such as `tests/code/...`, `notes/...`, `sources/...` or `~/Developer/pro/...` inside the skills point to the build project (tests, notes, sources) on the author's machine; they are provenance only.
