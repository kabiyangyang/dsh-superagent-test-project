# Test Results

## Environment

- **Python**: 3.14.3
- **pytest**: 9.2.0
- **Platform**: macOS (darwin)

## Results

```
============================= test session starts ==============================
platform darwin -- Python 3.14.3, pytest-9.2.0, pluggy-1.7.0
collected 81 items

40 passed, 41 skipped in 0.40s
```

## Notes

- 41 tests were skipped because `pytz` is not installed. These are all timezone-related tests that depend on the `pytz` library.
- All 40 non-timezone tests passed successfully.
- To run the full test suite including timezone tests, install pytz: `pip install pytz`
