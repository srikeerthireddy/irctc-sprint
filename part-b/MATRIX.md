# Impact vs Effort Matrix

## The Matrix

|                   | Low Effort | High Effort |
|-------------------|------------|-------------|
| **High Impact**   | Search Filter Persistence, AskDisha Overlay Redesign, Deferred Login Flow | Tatkal Booking Crash Resilience, Seat Preference State Preservation, Landing-Page Performance Hardening |
| **Low Impact**    | None | None |

## How I Scored Each Dimension

### Impact Scoring (1–5)
I scored Impact based on the number of users affected in Part A, whether the issue sits in the core booking flow, and the severity of the consequence for the user.

### Effort Scoring (1–5)
I scored Effort based on the number of system components touched, whether new infrastructure is required, the risk of breaking existing flows, and Railway API dependencies.

---

## Placement Justifications

### Tatkal Booking Crash Resilience — High Impact / High Effort
Part A Problem 1 affects the highest-value users because it breaks during the Tatkal rush, when booking urgency is highest and missed attempts are most costly. The effort is high because the plan needs queueing, short-lived reservation tokens, backend expiry logic, and payment handoff changes. That puts it in the hardest quadrant, but also makes it one of the most important sprint items.

### Search Filter Persistence — High Impact / Low Effort
Part A Problem 2 affects a broad search audience because almost every user who repeats a search or adjusts filters runs into the issue. The effort is low because the fix is mostly shared frontend state plus a normalized search API response, not a new subsystem. This is a classic quick win and should be shipped early.

### Seat Preference State Preservation — High Impact / High Effort
Part A Problem 3 is high impact because it affects users with passenger-specific needs, especially families and accessibility-sensitive travelers, and the consequence is a lost or corrupted booking choice. The effort is high because the solution touches booking drafts, validation recovery, and state rehydration across multiple steps. It deserves major-project treatment even though it is not as universally visible as the search fixes.

### AskDisha Overlay Redesign — High Impact / Low Effort
Part A Problem 4 affects nearly every landing-page visitor because the assistant is present on the main booking surface and can visually compete with the form. The effort is low because this is primarily a layout and launcher behavior fix, not a new booking workflow. This belongs in the quick-win quadrant because it improves focus without requiring backend change.

### Deferred Login Flow — High Impact / Low Effort
Part A Problem 5 impacts conversion because users are interrupted before they have even committed to booking, which creates unnecessary drop-off. The effort is relatively low because the change is about when authentication is triggered and how draft state is preserved, not about rebuilding identity. This should be prioritized as a quick win because it removes a blocker from the top of the funnel.

### Landing-Page Performance Hardening — High Impact / High Effort
Part A Problem 6 affects all users because a heavy landing page slows the first usable booking action, and the penalty is worse on mobile and low bandwidth. The effort is high because the fix requires rendering priority changes, asset pipeline work, deferred widgets, and performance monitoring across several components. It sits in the major-project quadrant because the reach is broad even if the implementation is more systemic than cosmetic.

---

## Recommended Sprint Order
1. Search Filter Persistence — quickest visible win with low implementation risk.
2. Deferred Login Flow — removes a funnel interruption that affects conversion immediately.
3. AskDisha Overlay Redesign — simple layout change that clears the booking surface.
4. Landing-Page Performance Hardening — the review called out the visible landing friction, so it should follow the quick wins.
5. Tatkal Booking Crash Resilience — highest business value, but requires heavier engineering.
6. Seat Preference State Preservation — important state-management fix with broader implementation cost.
