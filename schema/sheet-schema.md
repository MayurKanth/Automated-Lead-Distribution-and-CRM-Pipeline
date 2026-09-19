# Sheet schema reference

All column names below reflect the production schema. No real values are included anywhere in this document.

## `SourceData`
Raw, append-only mirror of Google Form responses.

| Column | Type | Notes |
|---|---|---|
| Date | Date | Submission date |
| Full Name | Text | |
| Phone Number | Number | Stored with country code, e.g. `91XXXXXXXXXX` |
| Per Hr Shoot | Text | Categorical, e.g. `1-3_hrs`, `6hr_+` |
| Content You Are Planning | Text | Free text |
| Do You Need Editing Services | Text | `Yes` / `No` |

## `LeadTracker`
Master working sheet. One row per lead; one column group per follow-up attempt (10 groups total).

**Core columns**

| Column | Type | Notes |
|---|---|---|
| Date | Date | Copied from SourceData |
| Lead ID | Text | Auto-generated, e.g. `L0166` |
| Name | Text | |
| Phone | Number | |
| Per Hr Shoot | Text | |
| Content You Are Planning | Text | |
| Do You Need Editing Services | Text | |
| Allocation Date | Date | Set by Distribute New |
| Sales Person | Text | Set by Distribute New |
| Call Status | Text | Result of first contact |
| Lead Quality | Text | e.g. `Interested`, `Not Interested`, `Followup` |
| Calling Date | Datetime | |
| Remark | Text | Free-text notes |

**Repeated per follow-up round (1–10)**

| Column pattern | Type | Notes |
|---|---|---|
| `Call Status N` | Text | |
| `Lead Quality N` | Text | |
| `Remark N` | Text | |
| `Date N` | Date | |

**Derived / summary columns**

| Column | Type | Notes |
|---|---|---|
| Follow-up Count | Formula | `=COUNTA(...)` across all `Call Status N` columns |
| Lead Status | Text | e.g. `Busy`, `Converted`, `Dead` |
| Allocation Count | Number | Times this lead has been re-allocated |


## `Not Call Picked Leads`
Leads with no successful contact yet.

| Column | Type | Notes |
|---|---|---|
| Date | Date | |
| Lead ID | Text | |
| Name | Text | |
| Phone | Number | |
| Email | Text | Optional |
| Allocation Date | Date | |
| Sales Person | Text | |
| Call Status | Text | Typically blank |
| Lead Quality | Text | Typically blank |
| Calling Date | Date | |
| Allocation Count | Number | Times re-attempted/re-allocated |

## `Clients`
Formula-driven view of converted leads, pulled from `LeadTracker`.

| Column | Type | Notes |
|---|---|---|
| Client ID | Formula | `UNIQUE(QUERY(LeadTracker, "select ... where Lead Status = 'Converted'"))` |
| Client Name | Formula | |
| Phone | Formula | |
| Sales Person | Formula | |
| Connected Date | Date | Manually confirmed |
| Status | Text | e.g. `Done` |
| Allocation Date | Formula | `VLOOKUP` back into `LeadTracker` |

## `Daily Call Summary`
Two side-by-side blocks: a single-day view and a date-range view.

| Metric row | Formula basis |
|---|---|
| Total allocated | `COUNTIFS` on `LeadTracker` Allocation Date |
| Total Call Made | `COUNTIFS` on Allocation Date + Calling Date not blank |

## `Week Summary`
| Column | Notes |
|---|---|
| Start Date / End Date | User-set range |
| Total Leads Allocated | Formula |
| Total Interested | Formula |
| Total Converted | Formula |
| Overall Conversion Rate | Formula |
| Avg. Follow-ups per Lead | Formula |
| Call Picked % | Formula |
