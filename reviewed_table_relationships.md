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
