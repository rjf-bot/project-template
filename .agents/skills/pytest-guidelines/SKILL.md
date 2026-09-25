---
name: pytest-guidelines
description: "Pytest standards for this project. Use whenever writing, extending, or reviewing test files — new test functions, fixtures, parametrize calls, mock setups, or coverage gaps. Also use when the user asks to audit existing tests for quality, coverage, or anti-patterns."
---

Every test in this project is a **contract**: a precise, checkable claim about one behaviour of one unit. Every rule below serves that goal — keeping each contract independent, legible, and worth trusting.

## File structure

- Root directory: `tests/`, mirroring `app/`. If `app/services/seeds_llm_service.py` exists, the test file is `tests/services/test_seeds_llm_service.py`.
- Required prefixes: files `test_*.py`, functions `test_*`, classes `Test*`.

## Contract anatomy (AAA)

Each test function follows three explicit phases, separated by blank lines:

```python
def test_filter_hallucinations_removes_package_absent_from_catalog():
    # Arrange
    packages = ["[ABAN 001] - Valid package", "[ABAN 999] - Nonexistent package"]
    catalog = {"[ABAN 001] - Valid package"}

    # Act
    result = filter_hallucinations(packages, catalog)

    # Assert
    assert result == ["[ABAN 001] - Valid package"]
```

One `assert` per distinct aspect. Multiple `assert` statements over the same property are acceptable; `assert` statements over different properties belong in separate contracts.

## Naming

The test name describes **what** is being tested and **what the expected outcome is**:

```
test_<unit>_<condition>_<expected_result>
```

Correct examples:
- `test_filter_assignments_index_out_of_range_is_dropped`
- `test_jaccard_similarity_identical_strings_returns_1`
- `test_seeds_llm_service_empty_scope_lines_returns_empty_list`

## Fixtures

Use fixtures for reusable setup — never repeat Arrange across multiple tests:

```python
@pytest.fixture
def minimal_catalog() -> set[str]:
    return {"[ABAN 001] - Cement plug", "[ABAN 002] - BPP"}
```

`session` or `module` scope only when setup is genuinely expensive and side-effect-free.

## Parametrize

Use `@pytest.mark.parametrize` to cover input variations without duplicating the contract body:

```python
@pytest.mark.parametrize("probe_type, expected_filtered", [
    ("Ancorada", True),
    ("DP", False),
    (None, False),
])
def test_filter_catalog_by_rig_respects_probe_type(probe_type, expected_filtered):
    ...
```

## Isolating external dependencies

Never let a test depend on Azure OpenAI, Document Intelligence, disk, or real network. Use `unittest.mock.patch` or `pytest-mock`:

```python
def test_llm_service_calls_openai_once(mocker):
    mock_call = mocker.patch("app.services.seeds_llm_service.AzureChatOpenAI.__call__")
    mock_call.return_value = ...
```

Tests that require real credentials must be marked `@pytest.mark.live` and **never** run in the default CI.

## Required markers

| Marker | When to use |
| --- | --- |
| `@pytest.mark.live` | Requires real Azure OpenAI or Document Intelligence |
| `@pytest.mark.slow` | Execution time > 5 s |

Both are excluded from `make test` via `pyproject.toml`. Never omit them for convenience.

## What a contract must cover

Each new contract must address at least one item from every applicable category:

- **Happy path** — valid input, expected output
- **Edge case** — empty list, `None`, empty string, boundary index
- **Invalid input** — wrong type, out-of-domain value; assert the exception with `pytest.raises`
- **Critical business rule** — guard-rail filters, deduplication logic, Jaccard, few-shot selection
- **Regression** — every fixed bug gets a contract that goes red without the fix and green with it

## Coverage

Minimum target: **80% coverage** (`make test-coverage-report`).

Coverage measures executed lines, not useful contracts. Prefer one contract that goes red on a real bug over ten that only raise the percentage.

## Anti-patterns

Each item below is a broken contract in disguise:

- **Execution order matters** — contracts are independent; if order matters, state is leaking.
- **Global state modified without teardown** — use fixtures with `yield` to clean up.
- **Conditional logic in the test** (`if`, `for`) — signals the contract covers two behaviours; split them.
- **`@pytest.mark.skip` without a comment** — document the reason and the issue link.
- **Real production data** — use factories, fixtures, or constants defined in the test itself.

## Completion criterion

When writing or reviewing tests, the work is done when:

1. Every new behaviour delivered has at least one corresponding contract.
2. Each contract goes **red** if the behaviour is reverted (verify mentally or temporarily).
3. `make test` passes with no unexpected errors or warnings.
4. No anti-pattern listed above is present in the modified files.
