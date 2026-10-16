# Risk Metrics Evaluation Table

This matrix defines the metrics used to measure the risk of a change request, how each metric is measured, and the thresholds for each risk level.

| Metric | Description | Measured as | No Risk | Low Risk | Medium Risk | High Risk | Very High Risk | Prohibitive |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Effort** | Person-hours the team needs to implement the change. | % of (total planned person-hours of the project – person-hours already completed) | 0% | > 0% – 5% | > 5% – 15% | > 15% – 30% | > 30% – 50% | > 50% |
| **Budget** | Total cost of the change: cash spending plus the internal cost of the team's time. | % of the remaining budget, after subtracting the costs already committed and the profit | 0% | > 0% – 10% | > 10% – 20% | > 20% – 30% | > 30% – 40% | > 40% |
| **Time** | Sprints the change adds to the schedule. | % of the sprints left until the final release | 0% | > 0% – 10% | > 10% – 20% | > 20% – 30% | > 30% – 40% | > 40%, or a milestone moves |
| **Expertise** | Whether the team can do the work of the change with the skills it has. | Number of team members able to do the work that requires the least common skill of the change | 3 or more | — | 2 | 1 | Partial coverage only (no member has the skill, but one has related experience) | Nobody, and there is no time to train or hire |
| **Resource Allocation** | Whether the resources the change needs (services, licenses, hardware) can be obtained in time. | Availability of the resources the company does not already own | All already owned | Bought and available immediately | Bought with a delivery time that fits the schedule | Single supplier or long-term contract | — | Cannot be obtained in time |
| **Legal Compliance** | Whether the change complies with legal regulations. | Is it legal? | Yes | — | — | — | — | No |

Notes:

- Each percentage range excludes its lower bound and includes its upper bound, so every value falls into exactly one level (for example, 5% is Low Risk and 5.1% is Medium Risk).
- Profit is subtracted once: if the committed costs already include the profit, it is not subtracted again.
- Budget measures the money of the change; Resource Allocation measures whether what has to be acquired can be obtained, so the two metrics do not measure the same thing.
- Expertise counts the people who can do the work, not the people who cannot, so a specialized task that needs one capable person is not rated by the size of the team.
- Legal Compliance is binary: a change is either legal (No Risk) or not (Prohibitive). Legal obligations the change brings, such as a privacy notice, are recorded as actions of the change, not as a risk level.
- The overall risk of a change is the highest level among its metrics.
