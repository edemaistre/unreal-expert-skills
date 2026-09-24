# Changelog

## v0.1 (2026-09-24)

First release: 10 agent skills for Unreal Engine 5.8, a lead (`unreal-expert`) and nine specialists.

- 10 skills written from 115 expert videos (77 h) and 82 official documentation pages, each with references and an offline-tested `ue_<domain>.py` module; the lead carries the shared toolkit.
- Blind-graded written scenarios (round 1): 51 % without the skills, 95 % with them; all 9 scenarios improved. Measured before the refactor below.
- Includes the refactor after that blind test: every point the graders found missing from both answers, and every error found in a skill's own content, went back into the owning skill. The internal drafts of that day were labelled v0.1 and v0.2 (or v1 and v2); mentions of them in the references and code comments are history, and both are folded into this public v0.1.
- Not yet verified inside Unreal Engine: the engine was not installed when v0.1 was cut (Epic sign-in pending). 1,534 offline checks pass with system Python and fake `unreal` modules only. Treat the in-engine code paths as unverified until a first live run (`tests/OPEN_ISSUES.md` in the build project).
- SKILL.md descriptions quoted so every frontmatter is strict YAML.
- Version stamped everywhere: `metadata.version`, a visible version line, `__version__` in every script.
