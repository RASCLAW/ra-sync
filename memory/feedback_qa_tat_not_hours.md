---
name: QA TAT Seconds Is Not Hours Worked
description: Total QA TAT Seconds in Informdata MTD Source = per-order audit turnaround time, not clock hours worked -- cannot use as hours proxy
type: feedback
related: [project_informdata_dashboard.md, feedback_informdata_actual_work_time.md]
originSessionId: 963b7072-75aa-4ddc-bb6e-e921a3857485
---
Never use `Total QA TAT Seconds / 3600` as "effective hours worked" for productivity calculations.

**Why:** QA TAT is the turnaround time per order for QA review — it reflects how fast each order gets audited, not how long the processor worked. Fuse-heavy agents (like Raven Rocha) have near-zero CMS hours but process full-month volume, producing absurd TAT-based PTGs (702% vs 51.1% source). Discovered session 152 when building agent productivity table.

**How to apply:** For any "hours worked" denominator in Informdata productivity formulas, use `Actual Production Hours CMS`, `Total Production Hours` (CMS + Fuse), or derive from `Total Expected Units` (pre-computed in MTD Source via Paycom scheduled hours × hourly goal). The safest option is `Total Expected Units` — it already handles CMS/Fuse platform splits and matches the source PTG exactly.
