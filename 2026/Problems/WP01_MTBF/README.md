# WP.01: MTBF and MTTR

## Introduction

### Managing a Production Process

In production engineering, two key metrics we often look to report to management are

1.  **MTBF**: _Mean Time Between Failures_. Describes the arithmetic mean ('average') of the expected period between failures of a machine or process. A common definition is

$$\text{MTBF} = \frac{\sum{\left( \text{Start of Downtime} - \text{Start of Uptime} \right) }}{\text{Number of Failures} }$$

Note that this metric only really applies when the 'failure rate', or the probability of failure in a given time period, $\lambda$, is constant. This typically applies only during the mid-part of the life-cycle of a process or machine. Assuming the rate of failure $\lambda$ is constant, then the MTBF can also be defined as

$$\text{MTBF} = \frac{1}{\lambda}$$

1.  **MTTR**: _Mean Time Between Repair_. Once a machine (or process) has failed, we will need to effect a repair. Often this metric is more important (or at least taken more seriously from a management perspective), as usually during the repair time the output of the process stops. This usually has, often severe, financial implications and so MTTR is a frequently cited metric to those outside the production team.

We can define the MTTR in a similar fashion to the MTBF, as the arithmetic mean of the time period to repair divided by the number of repairs needed

$$\text{MTTR} = \frac{\sum{ \left( \text{Start of Failure} - \text{End of Repair} \right) }}{\text{Number of Repairs}}$$

Another way of looking at the MTBF and the MTTR is that the MTBF tells us the _most frequent failures_ within a process, and the MTTR tells us which are the _most serious failures_.

In reality, though, the actual risk diagram (or risk curve) will vary from the typical one. For some processes, rare but serious failures might be tolerable: for instance if the cost of mitigating that failure is particularly high. This might be because a specific replacement part is a high-cost item that in most cases would not be used (or where it would deteriorate when stored); or where the lead time for replacement is high. Here the management of the process risk may decide a high MTTR is tolerable if the MTBF is sufficiently high. Equally there are processes where even a rare failure cannot be tolerated, and so insist on minimising the MTTR at all costs (for instance if there are significant financial penalties for missed production).

With FlexSim, the MTBF and MTTR objects within FlexSim are used to set random breakdown and recovery times for groups of objects in the model. Each MTBF/MTTR object can have any number of object members and each object can be controlled by more than one MTBF/MTTR object. The MTBF/MTTR object allows you to also specify what state the objects will go into when they go down and what behaviour they should perform. A model may also contain any number of MTBF or MTTR objects.

### Learning Outcomes

- The use of MTBF and MTTR as reportable metrics
- Decision making and management of production processes through model analysis
- Statistical modelling of failure, and incorporating these models into FlexSim
