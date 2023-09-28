# Medi Score Calculation

## Background

Write a function to calculate the score for a patient. This is a simple rule-of-thumb score used to identify ill patients.

## Introduction

The Medi Score is a simple aggregate scoring system created for this test based on other scores used widely 
in healthcare in which a score is calculated based on patients' physiological measurements. While this scoring system
is similar to recognised scores like NEWS2 it is distinct from it and has some nuance of its own.

The following observations are routinely taken when patients present to (or are being monitored in) hospital:

 1. respiration rate
 2. oxygen saturation
 3. level of consciousness or new confusion (whether the patient is newly confused, disorientated or agitated)
 4. temperature

A score for each property is allocated as the property is measured. The score for the property reflects how different
the measurement is from the range of expected values.

The scores for each property are added together, and 2 additional points are added for patients requiring
supplemental oxygen to maintain their oxygen saturation level. This final sum is the Medi score, which ranges from
0 to 14.

## Application Requirements

The Medi score for a patient is the **sum** of the scores for each property in the following table. Ranges are inclusive.

Your function can either take these measures as separate parameters, or take a single struct/object containing these values as attributes/properties. 

The Medi score for the patient should be returned as an **integer**.


| Property                       | Score 3  | Score 2      | Score 1         | Score 0                        | Score 1               | Score 2               | Score 3          |
| ------------------------------ | -------- | ------------ | --------------- | ------------------------------ | --------------------- | --------------------- | ---------------- |
| Air or oxygen?                 |          | Oxygen       |                 | Air                            |                       |                       |                  |
| Consciousness                  |          |              |                 | Alert                          |                       |                       | CVPU             |
| Respiration rate (per minute)  | &le;8    |              | 9&ndash;11      | 12&ndash;20                    |                       | 21&ndash;24           | &ge;25           |
| SpO<sub>2</sub> (%)            | &le;83   | 84&ndash;85  | 86&ndash;87     | 88&ndash;92 (or &ge;93 on air) | 93&ndash;94 on oxygen | 95&ndash;96 on oxygen | &ge;97 on oxygen |
| Temperature (&deg;C)           | &le;35.0 |              | 35.1&ndash;36.0 | 36.1&ndash;38.0                | 38.1&ndash;39.0       | &ge;39.1              |                  |

The values of the observations will be the following data types:

| Property                | Data type      | Comment                                           |
| ----------------------- | -------------- | ------------------------------------------------- |
| Air or oxygen?          | Enum (integer) | `0` if air, `2` if on oxygen                      |
| Consciousness           | Enum (integer) | `0` if alert, non-zero if CVPU                    |
| Respiration range       | Integer        |                                                   |
| SpO<sub>2</sub>         | Integer        |                                                   |
| Temperature             | Float          | This should be rounded to a single decimal place. |

## Examples

Here are some examples of patients and their Medi scores.

### Patient 1

| Property                | Observation | Score | Comment                                                                  |
| ----------------------- | ----------- | ----- | ------------------------------------------------------------------------ |
| Air or oxygen?          | 0           | 0     | The patient is breathing air, and does not require supplementary oxygen. |
| Consciousness           | 0           | 0     | The patient is conscious.                                                |
| Respiration range       | 15          | 0     |                                                                          |
| SpO<sub>2</sub>         | 95          | 0     | As the patient is breathing air, this is a normal range.                 |
| Temperature             | 37.1        | 0     |                                                                          |

This patient's final Medi score is **0**.

### Patient 2

| Property                | Observation | Score | Comment                                               |
| ----------------------- | ----------- | ----- | ----------------------------------------------------- |
| Air or oxygen?          | 2           | 2     | The patient requires supplementary oxygen.            |
| Consciousness           | 0           | 0     | The patient is conscious.                             |
| Respiration range       | 17          | 0     |                                                       |
| SpO<sub>2</sub>         | 95          | 2     | As the patient is breathing oxygen, this is elevated. |
| Temperature             | 37.1        | 0     |                                                       |

This patient's final Medi score is **4**.

### Patient 3

| Property                | Observation | Score | Comment                                                             |
| ----------------------- | ----------- | ----- | ------------------------------------------------------------------- |
| Air or oxygen?          | 2           | 2     | The patient requires supplementary oxygen.                          |
| Consciousness           | 1           | 1     | The patient is unconscious or confused.                             |
| Respiration range       | 23          | 2     |                                                                     |
| SpO<sub>2</sub>         | 88          | 0     | This is a normal range for patients breathing either air or oxygen. |
| Temperature             | 38.5        | 1     |                                                                     |

This patient's final Medi score is **8**.

## Bonus 

If you've completed the Medi score calculation and feel you have enough time left to spend then please attempt to solve any of the following issues, you will need additional fields in the input to satisfy these requirements:

- Alerting for trends in the Medi score - While the score is useful on its own to assess the urgency of treatment, an increasing score over a short period of time would be worth notifying someone about. Can your system flag up an additional risk if a score has raised by more than 2 points within a 24 hour period?

- Capillary Blood Glucose is another metric that is regularly recorded, but its range changes depending on when the patient last ate. The ranges (in mmol/L) and scores are as follows:

| Property                       | Score 3       | Score 2      | Score 1         | Score 0             | Score 1               | Score 2               | Score 3          |
| ------------------------------ | ------------- | ------------ | --------------- | ------------------- | --------------------- | --------------------- | ---------------- |
| CBG (When Fasting)             | 3.4 and below | 3.5 - 3.9    |                 | 4.0 - 5.4           |                       | 5.5 - 5.9             | 6.0 and above    |
| CBG (2 hours after eating)     | 4.5 and below | 4.5 - 5.8    |                 | 5.9 - 7.8           |                       | 7.9 - 8.9             | 9.0 and above    |