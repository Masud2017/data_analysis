# Internship EDA: presentation notes for Friday October 23, 2026

## What is complete

All **13 tables have dedicated descriptive-analysis notebooks**. Each review distinguishes observed data checks from undocumented source semantics. Large sample streams use SQL aggregates; events are checked as intervals; comparisons use validated keys and explicit join cardinality. No CSV export, RAG, predictive model or slides were created. The database was not modified.

| Table | Recorded rows | Participants | Main interpretation |
| --- | ---: | ---: | --- |
| activity | 987 | 8 | Activity summaries for labelled reporting periods |
| steps | 987 | 8 | Recorded steps and positive-goal attainment |
| goals | 987 | 8 | Target settings and within-participant changes |
| kilocalories | 987 | 8 | Recorded active/BMR estimates; source definitions unverified |
| stress | 987 | 8 | Stress summaries, qualifier labels and duration checks |
| heart_rate_values | 961 | 8 | Heart-rate summaries; resting algorithm unverified |
| heart_rate_sample | 4,912,004 | 8 | Timestamped values, cadence and summary comparison |
| stress_level_sample | 460,975 | 8 | Timestamped scores including potential sentinel codes |
| body_battery_sample | 315,848 | 7 | Timestamped score stream; source meaning unverified |
| body_battery_activity_events | 448 | 2 | Labelled intervals and recorded impact estimates |
| overall_sleep | 299 | 5 | Overall recorded score and qualifier |
| sleep | 299 | 5 | Categorical component ratings, not numeric durations/percentages |
| sleep_level_events | 18,402 | 8 | Labelled intervals, including awake |

Counts describe the verified review snapshots, not permanent source guarantees. Separate notebooks use separate read-only snapshots; rerunning can reveal changes.

## A plain-language explanation of the method

“I first asked what one row represents. Some tables contain a reporting-period summary, others contain individual timestamped samples, and others contain intervals. I checked types, missingness, unusual codes, duplicates, time bounds and coverage before calculating results. I then checked participant/date/start/end keys and reported unmatched records. Samples and events were aggregated to the comparison grain before joining, so repeated rows could not multiply a summary.”

“I kept original values and exclusions explicit. For instance, stress has negative potential sentinel codes despite no null average scores. The sleep component names look numeric, but their contents are labels. Missing floor values were excluded rather than treated as zero. I separated pooled associations from within-participant deviations and reported complete-case denominators.”

## Relationships worth showing the teacher

| Relationship | Verified evidence | Why it matters |
| --- | --- | --- |
| activity ↔ steps ↔ goals | 987 exact matches in reviewed pairwise comparisons; duplicated steps/goal values agree | Equal row counts alone were not used as proof |
| kilocalories ↔ activity/steps | 987 exact matches to each | Compare recorded periods; do not infer an official energy total |
| heart_rate_values ↔ activity | 961 matches, 26 activity-only periods | Missing summaries are not zero heart rate |
| heart_rate_sample ↔ heart_rate_values | 960 matched sample periods, one summary-only period | Min/max agree; sample mean is not always the summary average |
| stress_level_sample ↔ stress | 978 matches, three sample-only, nine summary-only | Different export coverage and sentinel filtering affect agreement |
| body-battery events ↔ samples | 178 matches, 530 sample-only periods; two event users among seven sample users | Event absence is not proof that no event happened |
| overall_sleep ↔ sleep | 299 matches, no unmatched periods | Both summary tables cover the same five participants |
| sleep-level events ↔ sleep summaries | 299 matches to each, 498 event-only periods; eight event users versus five summary users | Different cohort/period coverage must be disclosed |

The detailed checks, context-field agreement and unverified meanings are in `reviewed_table_relationships.md`. Relationships not directly joined are explicitly described there as indirect.

## Findings to present with their denominators

- Recorded steps meet a positive recorded goal in **361/987 periods (36.58%)**. This is not a population or full-wear-day estimate.
- Floors targets are met in **98/429 eligible periods (22.84%)**; **558 missing-floor records** are excluded. Recorded zero floors remain eligible.
- Heart-rate samples share **480 timestamps across reporting periods**. Period minima/maxima agree in all 960 matched summaries, but **138 sample means differ by more than one recorded unit**. Summary averaging rules remain unknown.
- Stress samples include **108 exact duplicate rows**, **147,538 negative-code values**, and one cross-period timestamp with conflicting scores. The nonnegative sample-mean comparison is much closer to summaries than a mean containing those negative codes, but is still not a verified algorithm.
- Body-battery events have **122 intervals outside reporting bounds** and **171 UTC-date crossings**. Clipping/union is a diagnostic convention, not a causal impact or official time total.
- All seven sleep component fields contain ratings, not numbers. **Two records contain INVALID** in three component fields. No duration or stage percentage was calculated from those labels.
- Sleep-level events have **1,864 intervals outside reporting bounds** and **729 UTC-date crossings**. Their interval union includes **awake**, so it must not be called official asleep duration.

## Questions that remain for the data owner

1. What timezone and date-label convention define reporting periods and sleep nights? Are interval endpoints inclusive or exclusive?
2. Does reporting duration represent a window, actual valid observation, or device wear? What are enrolment and expected observation dates?
3. What do negative stress codes, unknown qualifiers and INVALID labels mean? Which score ranges are valid?
4. How are summary averages, maxima and resting estimates derived: filtering, weighting, rounding, sampling windows and missing-data rules?
5. What are active/BMR definitions, their additivity/prorating, intensity-goal units/scope, and body-battery impact meaning/additivity?
6. Why do sample/event/summary exports cover different participants and periods? What policy resolves repeated timestamps, conflicts and overlapping exports?

## A defensible closing sentence

“The initial EDA is complete across the 13 tables. It establishes record grain, quality issues and validated relationships, while keeping missing coverage and undocumented algorithms visible. The findings describe recorded data; they are not medical, causal or predictive conclusions.”
