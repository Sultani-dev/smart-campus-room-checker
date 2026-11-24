# Practice #12: Code-based vs Low-code (Smart Campus Room Availability Checker)

Name / ID: ____________  Class day & time: ____________
AI tool / version / model: Claude (Anthropic), Sonnet 5.5
Low-code platform used: Appsmith (plan: free)

## 1. Shared requirement sheet
**User story:** As a student, I choose a future start time and a duration so that I can see whether room A-101 is available and why.

**Inputs:** start time, duration (min), blocked flag, fictional bookings (B1 tomorrow 10:00–11:00, B2 tomorrow 13:00–14:30).
**Outputs:** `available` (boolean), reason code, visible message.

| ID | Acceptance criterion |
|---|---|
| AC1 | Start time must be in the future |
| AC2 | Duration > 0 and ≤ 120 minutes |
| AC3 | Overlapping bookings are unavailable |
| AC4 | Blocked room is unavailable |
| AC5 | Clear decision shown; existing bookings never modified |
| AC6 | Invalid input gets an understandable message |

Rule order: validate input → blocked → overlap. Overlap test: `start < b.end && end > b.start` (adjacent is allowed).
Data is fictional only: no personal data, login, payment, or production database.

## 2. Eight-case test table
Times are relative to "tomorrow" so tests stay valid on any date.

| ID | Input | Expected | Code-based observed | Low-code observed |
|---|---|---|---|---|
| T1 | 11:30, 60 min | Available | Available (PASS) | OK (PASS) |
| T2 | 10:30, 60 min (overlaps B1) | Unavailable (OVERLAP) | OVERLAP (PASS) | OVERLAP (PASS) |
| T3 | 15:00, 60 min, blocked | Unavailable (ROOM_BLOCKED) | ROOM_BLOCKED (PASS) | ROOM_BLOCKED (PASS) |
| T4 | duration 0 | Validation error | DURATION_TOO_SHORT (PASS) | DURATION_TOO_SHORT (PASS) |
| T5 | duration 121 | Validation error | DURATION_TOO_LONG (PASS) | DURATION_TOO_LONG (PASS) |
| T6 | start in the past | Validation error | PAST_START (PASS) | PAST_START (PASS) |
| T7 | 11:00, 60 min (adjacent to B1) | Available | Available (PASS) | OK (PASS) |
| T8 | bookings after all checks | Unchanged | Unchanged (PASS) | ____ (scroll the table and fill in) |

Code-based summary line in the app: "8 / 8 passed (observed in this browser run)".

Manual form runs (screenshots attached):
- Code-based: 11:30 tomorrow, 55 min → Available [OK]; 11:30, 100 min, blocked → ROOM_BLOCKED; 11:30, 100 min, not blocked → Available [OK].
- Low-code: ____ (add your three manual runs: Available / Overlap / Blocked)

## 3. Low-code logic (Appsmith JS Object `Checker`)
Widgets: `StartInput`, `DurInput`, `BlockedSwitch`, `CheckBtn` (onClick → `{{Checker.run()}}`), `ResultText` (Text: `{{appsmith.store.result || "Press Check availability"}}`), `TestTable` (Table data: `{{Checker.runTests()}}`).

Differences from the report template (the final code is in the repository):
- Booking times are computed with `dayAt(1, h, m)` (tomorrow) instead of hard-coded ISO strings, so they stay correct every day.
- Validation, availability rules and test runner are separate functions (`check`, `run`, `runTests`), as in the code-based version.
- The result is passed to the Text widget with `storeValue("result", ...)`.

Simulated / unsupported in this platform: ____________ (write "nothing" if everything worked)

## 4. Effort measurements (real stopwatch values only)
| Phase | Code-based (min) | Low-code (min) |
|---|---|---|
| Setup | ____ | ____ |
| Implementation | ____ | ____ |
| Debugging | ____ | ____ |
| Testing | ____ | ____ |
| Unresolved defects | ____ | ____ |

Measured (numbers above): ____________
Impressions (not measured): ____________
Example of a defect found while debugging: the expression for `ResultText` was first pasted into the wrong field (Font family) and Appsmith reported "does not evaluate to type string"; moving it to the Content → Text field fixed it.

## 5. Security checklist (✔ / ✘ / n/a)
| Check | Code-based | Low-code |
|---|---|---|
| Input validated before use (type, range) | ✔ `validate()` | ✔ `check()` (same rules) |
| Output uses `textContent`, no `innerHTML` with user data (XSS) | ✔ | ____ (Text widget; confirm with the test below) |
| No SQL/query built from user text (injection) | ✔ no DB | ✔ no datasource/query used |
| Tried `<script>alert(1)</script>` in inputs: no execution (DAST, manual) | ____ (the duration field is `type=number`, the browser refuses the text; write what you saw) | ____ (write: no alert, INVALID_DURATION) |
| No secrets or API keys in client code or screenshots | ✔ none | ✔ none |
| App sharing/permissions: not public beyond need | n/a local | ____ (app not shared/deployed publicly?) |
| Connector/datasource access least privilege | n/a | n/a no datasource connected |
| Dependencies (SCA): none external | ✔ | platform updates by vendor |
| Platform update/patch responsibility | me | Appsmith vendor |
| No scanning of third-party services without authorization | ✔ | ✔ |

## 6. Comparison matrix
| Criterion | Code-based | Low-code |
|---|---|---|
| Functional tests | 8 / 8 | __ / 8 (T1–T7 PASS seen; confirm T8) |
| Build and repair time | __ min | __ min |
| Customization | Full control of HTML/CSS/JS | Limited to widgets and JS Objects |
| Security controls | Own responsibility (validation, textContent) | Platform provides widgets and permissions; validation still written by me |
| Versioning / portability | Git, plain HTML | ____ (what did you see: export, Git, etc.) |
| Ongoing cost | free, self-hosted | free plan used; Upgrade prompt visible; ____ |

## 7. Recommendation (3–4 sentences, based on your numbers)
____________ (write after §4 is filled: which approach was faster to build and repair, which passed more tests, and which you would choose for this feature and why, quoting your own numbers)

## 8. AI-use reflection (short)
- AI-generated: the `index.html` rule engine and tests (earlier practices), the Appsmith `Checker` JS Object, and this report's structure.
- Verified myself: ran the 8 tests in both apps and checked the manual form cases; ____ 
- Changed or rejected: ____ 
- Incorrect AI suggestion caught: ____ 
