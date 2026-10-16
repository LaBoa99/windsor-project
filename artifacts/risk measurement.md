# Risk Metrics Evaluation Table

This matrix defines the metrics used to measure the risk of a change request, the measurement baseline of each metric (what counts as 100%), and the thresholds for each risk level.

| Metric | Description | Measurement Baseline | No Risk | Low Risk | Medium Risk | High Risk | Very High Risk | Prohibitive |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Effort** | Person-hours the team needs to implement the change. | 100% = Total planned person-hours of the project – person-hours already completed | 0% | > 0% – 25% | > 25% – 50% | > 50% – 75% | — | > 75% |
| **Budget** | Total cost of the change: cash spending plus the internal cost of the team's time. | 100% = Remaining budget, after subtracting the costs already committed and the profit | 0% | > 0% – 10% | > 10% – 25% | > 25% – 30% | > 30% – 40% | > 40% |
| **Time** | Sprints the change adds to the schedule. | 100% = Sprints left until the final release | 0% | > 0% – 10% | > 10% – 25% | > 25% – 30% | > 30% – 40% | > 40% |
| **Expertise** | Team members who lack a skill the change requires, based on the technical stack and skill set of each member. | 100% = All team members | 0% | > 0% – 20% | > 20% – 30% | > 30% – 60% | > 60% – 80% | > 80% |
| **Resource Allocation** | Cost of the resources the company must acquire for the change (services, licenses, hardware), excluding the resources it already owns. | 100% = Remaining budget, after subtracting the costs already committed and the profit | 0% | > 0% – 10% | > 10% – 25% | > 25% – 30% | > 30% – 40% | > 40% |
| **Legal Compliance** | Whether the change complies with legal regulations. | Is it legal? (Yes / No) | Yes | — | — | — | — | No |

Notes:

- Each range excludes its lower bound and includes its upper bound, so every value falls into exactly one level (for example, 25% is Low Risk and 25.1% is Medium Risk).
- Profit is subtracted once: if the committed costs already include the profit, it is not subtracted again.
- Budget counts the whole cost of the change; Resource Allocation counts only what has to be acquired, so the two metrics do not measure the same amount.
- The overall risk of a change is the highest level among its metrics.
