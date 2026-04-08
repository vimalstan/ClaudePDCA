# Memory Rules

- Update scratchpad at natural breakpoints (completed step, decision made, blocker hit)
- PreCompact: always save scratchpad before context compression
- Write decisions to .claude/ first, basic-memory second — .claude/ is authoritative
- At task completion: extract learnings from scratchpad before deleting
- New patterns go to candidates.json (per-task), promoted at phase gate
- Never auto-promote learnings to standards.md — human approves
