---
name: fastapi-structure
description: "FastAPI project structure standard. Use whenever writing, extending, or modifying a FastAPI application — new routers, new services, new schemas, new dependencies, or refactoring existing modules. Also use when the user asks to review a FastAPI codebase for structure compliance."
---

All FastAPI code in this project must satisfy the structure rules in [`RULES.md`](RULES.md). Read `RULES.md` before writing any new file or modifying an existing one.

**layered** is the target adjective: a FastAPI app where each concern lives in exactly one layer, so any collaborator can navigate, test, and extend it without asking where anything goes. Every rule in `RULES.md` serves that goal.

## Branches

### Branch A — Writing new code

Before creating or modifying any FastAPI file:

1. Read `RULES.md` in full.
2. Identify the layer the new code belongs to: router, service, schema, model, or dependency.
3. Write code already conforming to that layer's rules — naming, typing, docstrings, Pydantic schemas, no cross-layer leakage.
4. After finishing each router, service, or schema, check it against `RULES.md §Layer Rules` before moving to the next.

**Completion criterion:** every delivered router, service, schema, and dependency passes all categories in `RULES.md §Layer Rules` — explicit check, not assumed. `main.py` contains only application initialization.

---

### Branch B — Review or audit of existing code

1. Read the target file(s) in full before writing a single comment.
2. Run the `RULES.md §Smells` scan for every category; record location (`file:line`) and the violated rule.
3. Report findings grouped by category (Layers → Routers → Services → Schemas → Dependencies → main.py → Smells). For each finding: quote the offending block, name the rule, and provide a corrected version.

**Completion criterion:** every category in `RULES.md §Smells` has been explicitly checked, and every finding has a corrected version.

---

### Branch C — Refactor existing code

Run Branch B first to obtain the full report. Then apply every fix to the files. After editing, re-read each file from top to bottom to confirm no new smell was introduced.

**Completion criterion:** all files re-read as **layered** against `RULES.md` — every fixed smell confirmed absent, no new smell introduced.
