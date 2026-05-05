---
name: Informdata Actual Work Time % = Inverse of Downtime
description: Actual Work Time % + Downtime % = 1.000 for every row — same metric flipped; no true attendance signal in this Excel
type: feedback
related: [project_informdata_dashboard.md, feedback_qa_tat_not_hours.md]
originSessionId: a6e3bf34-3b23-476d-a9d2-ccf10951064f
---
`Actual Work Time %` and `Downtime %` in the MTD Source sheet are perfect inverses — they sum to exactly 1.000 for every processor row. Using both would show the same data twice.

**Why:** Verified by printing both columns for all 151 rows — constant sum of 1.000, no exceptions. The `Paycom ÷ Total Expected Hours Worked` ratio is also a constant ~1.176 (formula artifact), not a real attendance signal.

**How to apply:** If asked to build an "Attendance" KPI for this dataset, use Downtime % instead (lower = better). There is no absenteeism or schedule-presence metric in this Excel file. Do not expose `Actual Work Time %` as a separate column — it adds zero information beyond Downtime.
