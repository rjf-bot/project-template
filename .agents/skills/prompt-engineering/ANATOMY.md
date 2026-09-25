# Anatomy of a Prompt — Reference

The disclosed reference for [`prompt-engineering`](SKILL.md). Every prompt is built
from the layers below. Read this before writing or modifying any prompt.

---

## Layers

A prompt is composed of up to four layers, assembled in order:

| Layer | What it is | Where it lives |
|---|---|---|
| **SYSTEM_PROMPT** | Role declaration + invariant rules | Module-level constant in the service file |
| **Context block** | Static structured data the LLM must reason over | Built by a pure function |
| **Few-shot block** | Worked examples selected at runtime | Built by a pure function |
| **Task block** | The specific input for this call | Built by a pure function |

Not every prompt uses all four layers. Add a layer only when it earns its token cost.

---

## SYSTEM_PROMPT rules

The `SYSTEM_PROMPT` declares the LLM's role and states the invariant rules — rules that
apply to every call regardless of input.

**Do:**
- Open with a one-sentence role declaration.
- List constraints as numbered rules. Numbered rules are easier to reference in bug reports.
- Write each rule as a precise, checkable statement. Ask: can a guard-rail filter enforce
  this mechanically? If yes, the rule is precise enough.
- Keep only rules that apply to every call. Input-specific instructions belong in the task block.

**Do not:**
- Put dynamic data in `SYSTEM_PROMPT`.
- Repeat a rule in both `SYSTEM_PROMPT` and the task block — one source of truth per rule.
- Use vague directives ("be consistent", "be careful") — they are no-ops.

---

## Context block

The context block provides static structured data the LLM must consult to produce the
output. Common forms: a reference catalog, a document excerpt, a lookup table.

**Token budget:** before adding a large context block, verify the assembled prompt stays
within the model's effective context window. Log block sizes at build time.

**Selection:** when context must be filtered (e.g. only documents relevant to this
request), make the selection logic a pure function separate from the prompt builder.

---

## Few-shot block

Few-shot examples anchor the LLM's output format and style to curated data.

**Selection:** choose examples by similarity to the current input (e.g. Jaccard on token
overlap). In production, do not use leave-one-out (LOO) — apply LOO only in experiments
to avoid data leakage.

**Format:** each example must contain both the input and the expected output in the same
format the LLM will receive and produce. Inconsistent formatting between examples and
the task block is a common source of regressions.

**When to omit:** if no example is sufficiently similar to the current input, an
unrelated few-shot example adds noise. Gate inclusion on a minimum similarity threshold.

---

## Task block

The task block presents the specific input for the current call and states the output
instruction.

**Do:**
- Present inputs in a labeled, structured format.
- State the output instruction once, at the end.
- When the input contains multiple items of the same type (e.g. several values to
  infer), decompose each one explicitly before asking for inference. This prevents
  the LLM from conflating similar values that coexist in the same context.

**Do not:**
- Mix the output instruction into the middle of the input presentation.
- Repeat instructions already stated in `SYSTEM_PROMPT`.

---

## Output schema

Every prompt that uses structured output must have a Pydantic model defined before the
prompt text. The model is the ground truth for what the LLM is expected to return.

**Rule:** do not parse free-text LLM output with regex. Use structured output
(`with_structured_output(Model)` in LangChain, or the `response_format` parameter
directly) so the LLM is constrained to the schema at inference time.

---

## Guard-rail filters

A guard-rail filter is a deterministic post-LLM function that enforces one structural
constraint from `SYSTEM_PROMPT`. Every structural constraint must have a corresponding
filter.

**When adding a new constraint**, add its filter in the same commit. A constraint
without a filter is a promise the code does not keep.

**Filter signature convention:**
```python
def filter_<name>(raw: <type>, ...) -> tuple[<cleaned_type>, int]:
    """..."""
    # returns (cleaned_result, n_violations_dropped)
```
The violation count must be logged at WARNING level by the caller.

---

## Provider content filters

Some LLM providers apply content filters that block requests containing certain terms.
The blocks are not always predictable from the text alone.

**Protocol:**
1. Run the prompt without sanitisation first. Record which inputs are blocked.
2. For each blocked input, identify the triggering term empirically — do not guess.
3. Apply the minimum sanitisation that unblocks the call without distorting meaning.
4. If sanitisation cannot preserve meaning, treat the input as unresolvable by the LLM
   and apply a deterministic fallback. Record the reason explicitly.
5. Document confirmed blocking terms when discovered.

---

## Prompt files vs. inline strings

| Approach | When to use |
|---|---|
| Inline string constant (`SYSTEM_PROMPT = """..."""`) | Short, stable, rarely changed prompts |
| Template file (e.g. Jinja2) | Long prompts with many dynamic variables, or prompts shared across multiple callers |

Do not mix approaches within the same service. If a service starts as inline and grows
beyond ~50 lines of prompt text with multiple dynamic sections, migrate entirely to
templates.

---

## Regression checklist

Before merging any prompt change, verify:

- [ ] Output schema (Pydantic model) unchanged — or the change is intentional and all
      callers updated
- [ ] Every numbered rule in `SYSTEM_PROMPT` still has a corresponding guard-rail filter
- [ ] Baseline input produces expected output after the change
- [ ] Token count of the assembled prompt has not grown unexpectedly (log at DEBUG level)
