# Linux SCHED_DEADLINE & Constant Bandwidth Server

## Hard Real-Time Guarantees
Linux `SCHED_DEADLINE` implements Earliest Deadline First (EDF) scheduling coupled with the Constant Bandwidth Server (CBS) algorithm. Tasks specify three parameters:
* `runtime`: Maximum compute time granted per period.
* `deadline`: Relative deadline timestamp.
* `period`: Recurrence interval.

---

## CBS Enforcement
To prevent a malicious or buggy real-time task from monopolizing the CPU, CBS throttles any task exceeding its declared `runtime` budget within the current `period`, preserving schedulability for remaining tasks.
