# Verified relationships: reviewed summary tables

Snapshot checks performed October 22, 2026. Counts describe this dataset, not a source guarantee. Keys and matches are rechecked in each notebook; independently loaded snapshots are not a transactional cross-table guarantee.

| Relationship | Verified evidence | Interpretation limit |
| --- | --- | --- |
| activity ↔ steps | 987 exact participant/date/start/end matches, one-to-one, no unmatched; steps values agree in all 987 | Matching estimates do not independently validate sensor accuracy |
| goals ↔ steps | 987 exact-period matches; steps goals agree in all 987 | Target definitions and adaptive logic remain undocumented |
| goals ↔ activity | 987 exact-period matches; compared identifiers/groups/durations agree | Floors missing in 558 records; 98/429 eligible records meet positive floor targets |
| kilocalories ↔ activity and steps | 987 exact-period matches per join; compared identifiers/groups/durations agree | Active/BMR kcal definitions and algorithm unconfirmed; no official total inferred |

Tested record-ID, participant/date and participant/period keys are unique and non-null in reviewed snapshots. The joins use participant/date/start/end plus one-to-one cardinality validation, not row counts. All comparisons preserve unmatched counts and avoid silent deduplication. Matching through steps provides an indirect kilocalories/goals relationship; no fresh direct goals/kilocalories join is claimed.

Shared period concerns: 970 windows are exactly 24 hours and 17 have other durations; two overlaps were detected in steps/kilocalories. Reported duration equals end minus start. Calendar labels do not match UTC start dates under a Unix-seconds interpretation; timezone and endpoint conventions need source confirmation. Reporting windows do not prove wear time. Unequal follow-up and missing dates limit pooled comparisons. Goals week labels differ from ISO date conventions in one week-number and nine weekday records.

Recorded steps meet positive goals in 361/987 records (36.58%). Active kcal versus steps has pooled Pearson 0.607 and within-participant-centered Pearson 0.668 for 970 exactly-24-hour windows. Both are descriptive, with repeated participants and unequal coverage. No causal, medical or predictive-validation claim is made.

Unverified semantics: intensity target units/weighting and daily/weekly scope; zero/sentinel conventions beyond observed values; active/BMR energy definitions, kcal algorithm, prorating and additivity; timezone, date labels, actual wear coverage and enrolment boundaries. No CSV export, RAG or model building is included.

## Stress summary review

Stress ↔ activity: 987 exact participant/date/start/end matches, one-to-one, no unmatched records; compared record identifiers, groups and reported durations agree. Stress has 987 rows across eight participants; tested keys are unique/non-null. No fresh direct stress/steps, stress/goals or stress/kilocalories join is claimed; those relationships are indirect through the separately validated activity matches.

Stress quality: 217 missing cells. Average score is non-null but equals −1 in 27 records, all labelled unknown; the 74 unknown qualifiers also include 47 nonnegative-score records. Sentinel and qualifier meanings remain unverified. No automatic validity claim is attached to nonnegative scores. No zero stress-score/duration values are recorded in this snapshot.

Observed duration identities: low + medium + high equals stress duration in 943/943 complete nonnegative records (44 excluded). Stress + rest + activity equals window duration in 3/946 complete records; the other 943 fall below it, with median residual 8,070 seconds. These arithmetic checks do not establish mutually exclusive categories, missing wear time or exhaustive coverage. Stress repeats the 970 exactly-24-hour/17 other-duration pattern and two overlapping periods; timezone and wear-time concerns remain.

Stress versus activity steps: among 948 nonnegative-score exactly-24-hour records, pooled Pearson is 0.010, pooled Spearman −0.039 and within-participant-centered Pearson 0.065. Including potential sentinel scores changes the association (987-record pooled Pearson 0.078; centered 0.211). These are descriptive repeated-measure correlations, not causal, medical or model-validation evidence. Score scale, averaging rules, sentinel codes, qualifier rules and duration semantics require the data owner’s dictionary.

## Heart-rate values summary review (sixth table)

Reviewed tables are activity, steps, goals, kilocalories, stress and heart_rate_values. That heart_rate_values summary review did not load or review heart_rate_sample; the separate sample review is documented below.

Heart_rate_values ↔ activity: 961 exact participant/date/start/end matches, 26 activity-only periods, no heart-rate-only periods; one-to-one cardinality validated, tested keys unique/non-null. Compared IDs, groups and durations agree in every match. The participant/date join has the same 961/26/0 counts: absence is not just a period-bound mismatch. The 26 absent summaries involve five participants, include 21 exactly-24-hour windows and have steps from 0 to 12,781 (median 0). No absence is imputed to zero or given an unsupported explanation. Other reviewed-table relationships are indirect through activity; no fresh direct heart-rate/steps, goals, kilocalories or stress join is claimed.

Three missing cells occur in one min/max/average triple; resting has none. No nonpositive recorded HR values occur. All 960 complete min/average/max triples are ordered. Resting falls below the reported minimum in 34/960 complete comparisons, never above maximum; its algorithm/window semantics are unverified, so this is diagnostic rather than an error claim. There are 949 exactly-24-hour windows, 12 other-duration windows and two overlaps. Duration equals end minus start for all records; timezone/date and actual wear coverage remain unverified.

For exactly-24-hour complete positive-HR/nonnegative-steps subsets, average HR versus steps uses 948 records: pooled Pearson 0.106, Spearman 0.098, centered Pearson 0.387. Resting uses 949 records: pooled Pearson −0.191, Spearman −0.139, centered Pearson −0.004. These descriptive repeated-measure associations differ by weighting and participant means; they are not causal, clinical or predictive-validation results. Units, averaging/min/max windows and resting estimation algorithm need source confirmation. Participant/date coverage ranges 93.04%–100% inside observed spans, not enrolment or wear coverage.

## Heart-rate sample review (seventh table)

Seven tables have now been reviewed: activity, steps, goals, kilocalories, stress, heart_rate_values and heart_rate_sample. The prior heart_rate_values notebook reviewed summaries only; the separate sample notebook now examines the sample stream with SQL aggregates, without loading millions of raw rows into pandas. All sample/summary checks use one read-only repeatable-read snapshot.

Heart_rate_sample contains 4,912,004 rows across 960 export/reporting periods and eight participants. All sample fields are non-null; heart_rate values are positive (30–202 recorded units). Tested participant/date/period/timestamp keys are unique; no exact duplicate rows occur. Each additional_id maps to one participant/period, but participant/timestamp alone is not unique: 480 timestamps are shared across periods, with equal heart-rate values. Two period overlaps create duplicate-export risk for pooled calculations; no automatic global deduplication is performed.

Derived sample periods ↔ heart_rate_values: 960 exact participant/date/start/end matches, no sample-only periods and one summary-only period; outer one-to-one cardinality is validated before comparisons. Record IDs/groups/durations are unique within each matched sample period and agree with summaries. All 960 min/max pairs agree exactly. Plain row-weighted sample means equal summaries in 5/960 periods, are within 0.5 in 747/960 and within one recorded unit in 822/960. There are 138 differences above one unit; maximum absolute difference is 18.458 and median 0.320. Summary averaging/rounding/filtering and resting algorithms remain unverified. No resting estimate is reconstructed, no absence becomes zero, and min/max agreement is not independent sensor validation.

There are 948 exactly-24-hour sample periods and 12 other-duration periods. All sample timestamps lie inside closed reporting bounds, with 810 exactly at the end; endpoint inclusivity/timezone conventions need documentation. Within-export-record cadence has 15-second observed mode/median/p90/p99 across 4,911,044 intervals, but 227 gaps exceed one hour (maximum 57,975 seconds). In the distinct participant-wide sequence, 950 unambiguous across-record intervals include 126 gaps above one hour; 482 intervals have ambiguous membership near shared timestamps. Across-record gaps are not folded into within-record cadence. Neither 15-second spacing nor long gaps prove nominal cadence or wear/nonwear.

Per-participant observed dates range 15–250; sample-row counts range 61,352–1,387,204. Date coverage inside individual first-to-last labelled spans ranges 93.04%–100%; unknown enrolment time and actual wear coverage are excluded. Record sample counts range 4–6,000, so labelled periods do not establish complete sampling. Relationships with activity and other reviewed tables remain indirect through the separate validated heart_rate_values matches; no fresh direct sample/activity join is claimed. Source units, timestamp conventions, export IDs, cadence, sentinel codes and summary rules remain unverified. No medical, causal or predictive conclusion follows.

## stress_level_sample review

Stress_level_sample has 460,975 rows across 981 periods/eight participants. All fields are non-null, yet scores include −1 (104,279 rows) and −2 (43,259); source meanings remain unverified. There are 108 exact duplicate rows across 74 within-period timestamp collisions, with no within-period value conflicts. Across periods, 164 participant/timestamps repeat and one has conflicting scores; a participant/timestamp key alone is unsafe.

Derived sample periods ↔ stress summaries: 978 exact participant/date/start/end matches, three sample-only, nine summary-only; one-to-one cardinality and unique/non-null period keys checked. IDs/groups/durations agree in all matches. Nonnegative sample maxima equal reported maxima in only 90/948 complete pairs (median absolute difference 4). Means including negative codes differ by median 8.146 across 951 pairs; nonnegative means differ by median 0.383 across 948 pairs, 857 within one unit. Timestamp collapse is a sensitivity calculation, not a global duplicate policy or verified source algorithm.

Within-record observed mode/median/p90/p99 are 180 seconds, but 29 gaps exceed one hour. Four reporting-window overlaps and 19 non-24-hour windows occur; record-boundary gaps are separated from internal cadence. Date coverage within observed participant spans is 89.36%–100%, not wear time. Score/sentinel meanings, cadence, summary weighting, timezone and endpoints remain unresolved; no clinical or causal conclusion follows.


## body_battery_sample review

Body_battery_sample has 315,848 rows across 708 periods/seven participants, with no null/nonpositive scores, exact duplicates or within-period timestamp collisions. Recorded scores range 5–100; source interpretation remains unverified. Ninety-one participant/timestamps repeat across periods without conflicting values. There are two overlapping reporting windows, 695 exactly-24-hour periods and 13 other-duration periods. Observed internal mode/median/p90/p99 are 180 seconds, but 82 gaps exceed one hour; cadence and wear semantics remain unverified.

Derived sample periods ↔ aggregated body_battery_activity_events: 178 exact participant/date/start/end matches, 530 sample-only periods, no event-only periods. All matched record IDs/groups/durations agree, with unique/non-null period keys and explicit one-to-one validation after event aggregation. The 448 events cover two participants, both among the sample table's seven. Missing event-table matches do not mean zero events or inactivity. Sample date coverage is 93.10%–100% within observed spans, not enrolment or wear coverage. No event-effect, clinical or causal inference is made.


## body_battery_activity_events review

Body_battery_activity_events has 448 events across 178 periods/two participants, with no null cells, exact duplicate rows or tested event-identity/start collisions. Labels are SLEEP 177, ACTIVITY 172, RECOVERY 81, NAP 17, STRESS 1. Source event/impact definitions remain unverified.

All 178 derived event periods match body_battery_sample aggregates on participant/date/start/end; 530 sample-only periods remain and both event users are among the sample table's seven. Period keys are unique/non-null after aggregation, one-to-one joins are enforced and matched record IDs/groups/durations agree. Raw event×sample joins are avoided.

Under a declared half-open diagnostic, 122 events extend outside reporting bounds and 171 cross UTC dates. Raw lengths sum to 5,699,100 seconds; clipping yields summed lengths 5,365,033 and summed per-period union 5,362,633, with 2,400 overlap-excess seconds in one period. These are diagnostic quantities, not official totals or complete wear time; one reporting-window overlap remains a cross-period risk. Impacts span −22 to 79 (181 negative, 29 zero, 238 positive), without assumed sign meaning/additivity or proportional allocation on clipping. No medical, causal or event-effect inference follows.


## overall_sleep review

Overall_sleep contains 299 records/five participants; tested keys are unique/non-null, with no null cells or exact duplicates. Recorded scores range 26–98 (median 76), without zero/negative values; source scale/algorithm remain unverified. Qualifiers are FAIR 126, GOOD 92, POOR 60, EXCELLENT 21, not clinical thresholds defined here. There are 293 exactly-24-hour windows and six other-duration windows, no window overlaps, and all reported durations equal end minus start. Observed-date coverage is 78.57%–99.18% within individual first-to-last spans, not enrolment/wear coverage.

Overall_sleep ↔ sleep: 299 exact participant/date/start/end one-to-one matches, no unmatched periods; all compared record IDs/groups/durations agree, with the same five participants. Overall_sleep ↔ aggregated sleep_level_events: 299 matches, no summary-only periods, 498 event-only periods. Events have 18,402 rows across 797 periods/eight participants; all five summary users occur among those eight. Matched context agrees. Missing summaries are not zero scores/no sleep; event intervals are aggregated before joining to prevent multiplication. Source score, qualifier, period/night and timezone meanings remain unresolved.

