# Smart Campus Room Availability Checker: Code-based vs Low-code

Practice #12, Software Engineering (AI-Driven). The same small feature is built twice and compared:

1. **Code-based**: plain HTML/CSS/JavaScript (`index.html`)
2. **Low-code**: Appsmith (free plan)

The feature: a student chooses a future start time and a duration (1–120 min) and sees whether the fictional room A-101 is available, with a reason code. All data is fictional (no personal data, login, payment, or database).

## Links
- Live demo (code-based): ____________
- Live demo (Appsmith): ____________

## Repository contents
| Path | Description |
|---|---|
| `index.html` | Code-based solution (rules, UI and the T1–T8 tests in one file) |
| `appsmith/` | Exported Appsmith application (JSON) |
| `screenshots/` | Screenshots of both implementations and their test results |
| `REPORT.md` | Requirement sheet, test table, effort measurements, security checklist, comparison, recommendation, AI-use reflection |

## Acceptance criteria
- AC1: Start time must be in the future
- AC2: Duration > 0 and ≤ 120 minutes
- AC3: Overlapping bookings are unavailable
- AC4: Blocked room is unavailable
- AC5: Clear decision shown; existing bookings never modified
- AC6: Invalid input gets an understandable message

Rule order: validate input → blocked → overlap. Adjacent bookings are allowed.

## How to run
**Code-based:** open `index.html` in a browser (no build step, no dependencies). Press "Run tests" to execute T1–T8.

**Low-code:** import the JSON from `appsmith/` into Appsmith (Import application), then open the page. The logic is in the JS Object `Checker` (`check`, `run`, `runTests`). The check button calls `Checker.run()` and the test table uses `Checker.runTests()`.

## Tests
Eight identical cases (T1–T8) are run in both versions: free interval, overlap, blocked room, duration 0, duration 121, past start, adjacent interval, and bookings unchanged. Expected and observed results are in `REPORT.md`, section 2.

## Tools
- AI tool: Claude (Anthropic), Sonnet 5.5
- Low-code platform: Appsmith (free plan)

## Notes
Only values I measured myself are reported as results. Anything simulated or unsupported in the low-code platform is labeled in `REPORT.md`, section 3.
