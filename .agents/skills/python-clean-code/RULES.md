# Rules — Python Clean Code

Exhaustive checklist for the `python-clean-code` skill. Every item is a binary verdict: pass or fail.

---

## Names

| Rule | Fail example | Pass example |
| --- | --- | --- |
| Variables and functions in `snake_case` | `calcAge()`, `tmpData` | `calculate_client_age()`, `raw_data` |
| Classes in `PascalCase` | `customer_order` | `CustomerOrder` |
| Constants in `UPPER_CASE` | `maxRetries = 3` | `MAX_RETRIES = 3` |
| No single-letter names outside loop counters | `x = fetch()` | `response = fetch()` |
| No abbreviations that obscure intent | `calc_ci()` | `calculate_compound_interest()` |
| Boolean names read as predicates | `status` | `is_active`, `has_errors` |

---

## Functions

| Rule | Guidance |
| --- | --- |
| Single responsibility | A function does one thing; its name describes it completely. If "and" fits naturally in the name, split it. |
| ≤ 20 lines (guideline, not a hard limit) | Longer functions are a signal to look for extraction opportunities, not an automatic violation. |
| ≤ 3 parameters | Beyond 3, group related params into a dataclass or typed dict. |
| No flag arguments | `process(data, is_dry_run=True)` should be two functions: `process(data)` and `preview(data)`. |
| Return type annotated | `def get_name(id: int) -> str:` |

---

## Comments

| Rule | Guidance |
| --- | --- |
| No comments that restate the code | `# increment counter` above `counter += 1` — delete it |
| Docstrings on public functions, classes, and modules (PEP 257) | Use triple-double-quotes; first line is a one-sentence summary ending with a period |
| Comments explain *why*, not *what* | Reserve inline comments for non-obvious decisions: algorithm trade-offs, upstream quirks, intentional workarounds |

---

## Formatting (PEP 8)

| Rule | Guidance |
| --- | --- |
| 4-space indentation, no tabs | |
| Lines ≤ 79 characters | Use implicit line continuation inside brackets; avoid backslash continuation |
| Two blank lines between top-level definitions | Functions and classes at module level are separated by two blank lines |
| One blank line between methods inside a class | |
| No trailing whitespace | |
| Imports: stdlib → third-party → local, each group separated by a blank line | |
| No wildcard imports (`from module import *`) | |

---

## Error Handling

| Rule | Guidance |
| --- | --- |
| Raise specific exceptions | `raise ValueError("id must be positive")` not `raise Exception("error")` |
| Never bare `except:` or `except Exception:` without re-raise or logging | Swallowing exceptions hides bugs |
| Keep error-handling code separate from business logic | Prefer a thin try/except wrapper around a clean inner call |
| Use `contextlib.suppress` only when suppression is genuinely intentional | Document *why* the exception is safe to ignore |

```python
# Fail
try:
    result = compute(data)
except:
    pass

# Pass
try:
    result = compute(data)
except ValueError as exc:
    logger.warning("Invalid input: %s", exc)
    raise
```

---

## DRY (Don't Repeat Yourself)

| Rule | Guidance |
| --- | --- |
| No duplicated logic blocks | Extract to a shared function or method |
| No copy-pasted constants | Define once as a named constant |
| No parallel structures that evolve together | If two places must change together, one of them is the wrong place for the logic |

---

## Classes

| Rule | Guidance |
| --- | --- |
| Single Responsibility Principle | One class, one reason to change |
| No God Objects | A class doing I/O *and* parsing *and* business logic is three classes |
| Prefer composition over inheritance | Inherit only for genuine "is-a" relationships |
| `__init__` only initialises — no side effects, no I/O | |
| Use `dataclass` or `NamedTuple` for plain data holders | Avoids boilerplate and signals intent |

---

## SOLID

| Principle | Quick check |
| --- | --- |
| **S** — Single Responsibility | Does the class/function have exactly one reason to change? |
| **O** — Open/Closed | Can you extend behaviour without editing existing code? (strategy pattern, plugins, hooks) |
| **L** — Liskov Substitution | Can every subclass substitute its parent without breaking callers? |
| **I** — Interface Segregation | Does the caller depend only on the methods it actually uses? |
| **D** — Dependency Inversion | Does the high-level module depend on abstractions (protocols/ABCs), not concretions? |

---

## Code Smells

Check each category explicitly:

| Smell | Signal |
| --- | --- |
| Long parameter list | Function takes > 3 positional arguments |
| Dead code | Unreachable branches, commented-out blocks, unused imports |
| Magic literals | Bare numbers or strings with no named constant (`if status == 2`) |
| Deeply nested logic | More than 2 levels of `if/for/while` nesting in one function |
| Mutable default argument | `def f(items=[]):` — use `None` and assign inside |
| God Object | A class with > ~7 public methods spanning unrelated concerns |
| Shotgun surgery | A single change requires edits in many unrelated places |
| Feature envy | A method uses more data from another class than its own |

---

## Python-Specific (Pythonic / PEP conventions)

| Rule | Fail example | Pass example |
| --- | --- | --- |
| Use comprehensions for simple transforms | `result = []; for x in xs: result.append(f(x))` | `result = [f(x) for x in xs]` |
| Use `enumerate` instead of index counter | `for i in range(len(items)):` | `for i, item in enumerate(items):` |
| Use `with` for resources | `f = open(path); …; f.close()` | `with open(path) as f:` |
| Prefer f-strings over `%` or `.format()` | `"Hello, %s" % name` | `f"Hello, {name}"` |
| Prefer `tuple` / `frozenset` for immutable collections | `ALLOWED = ["GET", "POST"]` | `ALLOWED = ("GET", "POST")` |
| Use type hints on all public interfaces (PEP 484) | `def get(id):` | `def get(id: int) -> User:` |
| Use `Protocol` or `ABC` for dependency inversion, not concrete classes | | |
| Module `__init__.py` exports only the public API | Avoid re-exporting implementation details | |
| Docstrings on every public function, class, and module (PEP 257) | | |
| Never catch `BaseException` | Catches `KeyboardInterrupt` and `SystemExit` | |
| `is` / `is not` for identity; `==` / `!=` for equality | `if x == None:` | `if x is None:` |
| Use `Path` (pathlib) instead of string-based `os.path` operations | `os.path.join(base, "file.txt")` | `Path(base) / "file.txt"` |
