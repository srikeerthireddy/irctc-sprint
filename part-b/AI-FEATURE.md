# AI Feature Specification: Waitlist Confirmation Probability Predictor

## Problem It Solves
This AI feature addresses Part A, Problem 1 and Part A, Problem 2 in [part-a/PROBLEMS.md](../part-a/PROBLEMS.md) by helping users judge whether a waitlisted or borderline train choice is worth keeping. The user problem is uncertainty: people do not know if they should wait, switch, or book an alternative before they invest more time in the flow.

## Proposed Feature — User Perspective
When a user views search results or a PNR status, they see a simple confidence label that estimates whether the ticket is likely to confirm. The user can use that signal to continue with the current option, pick a safer alternative, or hold off and search a different train.

## Model or API Choice
Use a custom XGBoost or LightGBM classifier exposed through an internal prediction service. This is the best fit because the task is structured prediction over tabular history, not conversational generation, and it can return a score quickly enough for low-bandwidth users.

## Training or Input Data
The model needs historical PNR outcomes, waitlist movement, route, quota, train number, booking lead time, cancellation rate, day of week, time of day, and proximity to departure. The source data comes from IRCTC historical booking records and railway availability or cancellation feeds where those feeds are available.

## How Output Is Shown to the User
The UI shows a result card with a confidence band such as High, Medium, or Low, plus a percentage range and a short reason label. If the score is useful, the screen also shows one or two alternative trains or classes so the user can act on the prediction instead of just reading it.

```text
RESULT CARD
----------------------------------------
Train 12622 Tamil Nadu Exp
WL 12

Confirmation chance: 68%  [Medium]
Reason: high cancellation movement on this route

[ View safer alternatives ]
```

## Confidence Threshold and Fallback
Only show the prediction when the model confidence is above the display threshold and the input data is complete enough to support a stable score. If the confidence is low, the API fails, or the data is missing, hide the prediction and fall back to a simple rule-based hint that uses recent cancellation movement only.

## Success Metrics
- Users click safer alternatives more often when the model predicts low confirmation odds.
- Users spend less time bouncing between borderline waitlist options.
- The prediction is shown without blocking the booking flow or increasing abandonment.

## Limitations and Risks
The model can be wrong when the railway changes patterns suddenly, so it must never be the only basis for a booking decision. Historical data may also reflect route- or season-specific bias, so the score should be framed as an estimate rather than a guarantee. If the model cannot produce a stable answer, the system should degrade to a plain, non-AI recommendation instead of forcing the user to trust a weak prediction.
