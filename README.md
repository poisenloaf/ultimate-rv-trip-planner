# Ultimate RV Trip Planner skills

Source of truth for the skills in the **Ultimate RV Trip Planner** Grok Bot template: https://x.ai/bot/BZr7_j7WC1klRjG8SrwWx

- `skills/<slug>/SKILL.md` — the four template skills. Each has `template_version` in its frontmatter.
- `VERSION` — current template version (line 1) and release date (line 2).
- `CHANGELOG.md` — what changed in each version.

## Monthly update check
Bots made from the template run a routine at 9 AM on the 1st of each month. It compares the installed skills' `template_version` with `VERSION` here. If this repo is newer, it updates only these four skills (never the owner's memories, boards, routines or other skills), asks first if the owner edited a skill, and says briefly what changed. If current, it stays quiet. The owner can say "turn off updates".

## Feedback
rvtripplanner@agentmail.to
