---
name: prompt-engineering
description: "Standard for writing, reviewing, and iterating LLM prompts. Use whenever writing a new prompt, modifying a SYSTEM_PROMPT or a user prompt builder, adding a few-shot example, or investigating a quality regression after a prompt change."
---

A prompt is a **contract**: it specifies the inputs the LLM receives and the output it
must produce. Like any contract, it has a structure, a set of constraints, and a way to
verify it holds. Every decision below serves one goal: keeping that contract explicit,
testable, and easy to change safely.

Read [`ANATOMY.md`](ANATOMY.md) before writing or modifying any prompt. It defines the
layers every prompt is built from.

## Branches

### Branch A — Writing a new prompt

Before writing any prompt:

1. Read `ANATOMY.md` in full. Identify which layers the new prompt needs.
2. Decide the output schema first — what structured type must the LLM return?
   Define it as a Pydantic model before drafting the prompt text.
3. Write the `SYSTEM_PROMPT` to set role and constraints. Keep it to rules that apply
   to every call — not to any specific input.
4. Write the user-prompt builder as a pure function that assembles the dynamic blocks
   and returns a plain string. No LLM calls inside builders.
5. Make every constraint in the `SYSTEM_PROMPT` numbered and checkable. Vague rules
   ("be accurate") are no-ops; precise rules ("return the value exactly as it appears
   in the reference data") are enforceable by guard-rail filters.
6. Add at least one guard-rail filter for each structural constraint. A constraint
   without a filter is untested.

**Completion criterion:** output schema defined as a Pydantic model, `SYSTEM_PROMPT` has
only numbered checkable rules, user-prompt builder is a pure function, and every
structural constraint has a corresponding guard-rail filter.

---

### Branch B — Modifying an existing prompt

Before changing any existing prompt:

1. Record the current behaviour: run the existing prompt against at least one known input
   and note the output. This is the baseline.
2. Make the change.
3. Re-run against the same input. Confirm the change produced the intended difference and
   nothing else changed.
4. If a guard-rail filter was previously catching an error the prompt change is meant to
   prevent, verify the filter still applies — do not remove filters because the LLM
   "probably won't" produce the error anymore.

**Completion criterion:** baseline recorded before the change; output delta confirmed to
match intent; all existing guard-rail filters still present.

---

### Branch C — Investigating a quality regression

When output quality has dropped:

1. Isolate the failing input. Reproduce the failure with a single call.
2. Determine which layer caused the regression: wrong context block, violated constraint
   in `SYSTEM_PROMPT`, or missing guard-rail filter.
3. Fix the layer. If the fix is a new constraint, add a guard-rail filter for it.
4. Re-run the failing input to confirm the regression is resolved.
5. Run at least one prior known-good input to confirm no regression was introduced.

**Completion criterion:** failing input produces correct output; at least one prior
known-good input still produces correct output.
