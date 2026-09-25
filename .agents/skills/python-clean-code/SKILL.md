---
name: python-clean-code
description: "Python coding standard for this project. Use whenever writing, extending, or modifying any Python file — new modules, new functions, new classes, bug fixes, refactors. Also use when the user asks to review, clean up, or audit existing Python code for Clean Code or PEP 8 compliance."
---

All Python code in this project must satisfy the standards defined in [`RULES.md`](RULES.md). Read `RULES.md` before writing any new file or modifying an existing one.

**clean** is the target adjective: code that any reviewer can understand, test, and extend without asking for help. Every rule in `RULES.md` serves that goal.

## Branches

### Branch A — Writing new code

Before creating or modifying any Python file:

1. Read `RULES.md` in full.
2. Write the code already in conformance — names, typed signatures, docstrings, error handling, class structure.
3. After finishing each function or class, check it against `RULES.md §Code Smells` before moving to the next.

**Completion criterion:** every delivered function and class passes all categories in `RULES.md` — explicit check, not assumed.

---

### Branch B — Review or audit of existing code

1. Read the target file(s) in full before writing a single comment.
2. Run the `RULES.md §Code Smells` scan for every category; record location (`file:line`) and the violated rule.
3. Report findings grouped by category (Names → Functions → Comments → Formatting → Errors → DRY → Classes → Smells → Python-specific). For each finding: quote the offending line or block, name the rule, and provide a corrected version. Borderline findings: explain the trade-off rather than omitting them.

**Completion criterion:** every category in `RULES.md §Code Smells` has been explicitly checked, and every finding has a corrected version.

---

### Branch C — Refactor existing code

Run Branch B first to obtain the full report. Then apply every fix from that report to the file. After editing, re-read the file from top to bottom to confirm no new smell was introduced.

**Completion criterion:** the file re-reads clean against `RULES.md` — every fixed smell confirmed absent, no new smell introduced.
