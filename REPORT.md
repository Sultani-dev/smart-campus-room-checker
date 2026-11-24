# Practice #12: Code-based vs Low-code (Smart Campus Room Availability Checker)

Name / ID: ____________  Class day & time: ____________
AI tool / version / model: ____________
Low-code platform used: ____________  (plan: free)

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

## 2. Eight-case test table (fill "Observed" from your real runs)
Times are relative to "tomorrow" so tests stay valid on any date.

| ID | Input | Expected | Code-based observed | Low-code observed |
|---|---|---|---|---|
| T1 | 11:30, 60 min | Available | | |
| T2 | 10:30, 60 min (overlaps B1) | Unavailable (OVERLAP) | | |
| T3 | 15:00, 60 min, blocked | Unavailable (ROOM_BLOCKED) | | |
| T4 | duration 0 | Validation error | | |
| T5 | duration 121 | Validation error | | |
| T6 | start in the past | Validation error | | |
| T7 | 11:00, 60 min (adjacent to B1) | Available | | |
| T8 | bookings after all checks | Unchanged | | |

## 3. Low-code logic to paste (Appsmith JS Object, or adapt to your builder)
Widgets: DatePicker/DateTime `StartInput`, Input (number) `DurInput`, Switch `BlockedSwitch`, Button `CheckBtn` (onClick → `Checker.run()`), Text `ResultText`.
Bookings: a static JSON array in a JS Object or table (fictional).

```javascript
export default {
  bookings: [
    { title: "Algorithms seminar", start: "TOMORROW 10:00", end: "TOMORROW 11:00" },
    { title: "Study group",        start: "TOMORROW 13:00", end: "TOMORROW 14:30" }
  ], // replace with real ISO datetimes of tomorrow, e.g. "2026-11-25T10:00:00"
  run() {
    const start = new Date(StartInput.selectedDate);
    const dur = Number(DurInput.text);
    if (isNaN(start)) return "Enter a valid start date and time.";
    if (!Number.isFinite(dur) || DurInput.text === "") return "Enter the duration as a number of minutes.";
    if (start <= new Date()) return "Start time must be in the future.";
    if (dur <= 0) return "Duration must be greater than 0 minutes.";
    if (dur > 120) return "Duration cannot exceed 120 minutes.";
    if (BlockedSwitch.isSwitchedOn) return "Unavailable: the room is blocked.";
    const end = new Date(start.getTime() + dur * 60000);
    const hit = this.bookings.find(b => start < new Date(b.end) && end > new Date(b.start));
    return hit ? "Unavailable: overlaps with " + hit.title : "Available for the selected time.";
  }
}
```
Bind `ResultText.text` to `{{Checker.run()}}` or set it in the button handler. If something is simulated or unsupported in your platform, label it here: ____________

## 4. Effort measurements (real stopwatch values only)
| Phase | Code-based (min) | Low-code (min) |
|---|---|---|
| Setup | | |
| Implementation | | |
| Debugging | | |
| Testing | | |
| Unresolved defects | | |

Measured vs. impression: keep these in separate lists.

## 5. Security checklist (mark ✔ / ✘ / n/a and add evidence)
| Check | Code-based | Low-code |
|---|---|---|
| Input validated before use (type, range) | ✔ `validate()` | |
| Output uses `textContent`, no `innerHTML` with user data (XSS) | ✔ | |
| No SQL/query built from user text (injection) | ✔ no DB | |
| Tried `<script>alert(1)</script>` in inputs: no execution (DAST, manual) | | |
| No secrets or API keys in client code or screenshots | ✔ none | |
| App sharing/permissions: not public beyond need | n/a local | |
| Connector/datasource access least privilege | n/a | |
| Dependencies (SCA): none external | ✔ | platform updates by vendor: |
| Platform update/patch responsibility | me | |
| No scanning of third-party services without authorization | ✔ | ✔ |

## 6. Comparison matrix (fill after measuring)
| Criterion | Code-based | Low-code |
|---|---|---|
| Functional tests | __ / 8 | __ / 8 |
| Build and repair time | __ min | __ min |
| Customization | | |
| Security controls | | |
| Versioning / portability | Git, plain HTML | |
| Ongoing cost | free, self-hosted | |

## 7. Recommendation (3–4 sentences, based on your numbers)
____________

## 8. AI-use reflection (short)
Which parts were AI-generated, what you verified yourself, what you changed or rejected, any incorrect AI suggestion you caught: ____________
