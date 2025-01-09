---
title: "Metrics in Value Stream Mapping (VSM)"
alias:
  - OVS
tags: [  DevOps, SAFe® ]
---

## Các metrics

### processs step metrics

1. **Lead Time (LT)**
   * **Definition**:: Total time from when a request is made to when value is delivered.
   * **Purpose**:: Measures overall responsiveness of the process.
2. **Cycle Time (CT)**
   * **Definition**:: Time required to complete a single unit of work.
    $$
    Cycle Time = \frac{Process Time}{Number of Items in Batch}
    $$
   * **Purpose**:: Evaluates the efficiency of individual steps.
3. **Process Time (PT)**
   * **Definition**:: Actual working time for a specific process step (excluding waiting).
4. **%C/A (Percent Complete and Accurate)**
   * **Definition**:: Proportion of work completed without rework or correction.
   * **Purpose**:: Indicates quality and handoff efficiency.


### Value stream metrics

1. The total process time
   $$
    Total Process Time = Process Time_{1st\ step} + Process Time_{2nd\ step} + ... + Process Time_{last\ step}
   $$
2. The total lead time
   $$
    Total Lead Time = Lead Time_{1st\ step} + Lead Time_{2nd\ step} + ... + Lead Time_{last\ step}
   $$
3. The activity ratio
   $$
   Activity Ratio = \frac{Total Process Time}{Total Lead Time}
   $$
4. Rolled %C&A
  $$
  Rolled \%C\&A = \%C\&A_{1st\ step} \times \%C\&A_{2nd\ step} \times ... \times \%C\&A_{last\ step} x 100
  $$