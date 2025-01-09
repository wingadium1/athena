---
title: "Flow Framework®"
tags: [ DevOps ]
---

> [!info]
> To measure the performance of the [[refs/safe_value_stream|value streams]], Kersten proposes the Flow Framework®, a structure  of Flow artifacts and the measurement of those artifacts using Flow Metrics®.


Kersten  describes the need to move from project-based design to product development using long-lived  value streams. And he identified the following  four outcomes that he wanted to monitor:
 * Value
 * Cost
 * Quality
 * Happiness

These items related to 4 Flow Items:
* Features to deliver: bring about direct business value.
  * In SAFe®, Flow Framework® features can be mapped to feature -> decompose into user story and enabler story (inside a [[refs/safe_pi|Program Increment (PI)]])
* Defect fixes
  * fixing any defects
  * the fix is pulled by the customer as part of the solution
* Risk avoidance
  * Reduce disks are delivered by the value stream to difference stakeholders
  * In SAFe®, it means ot fullfill NFRs (Non Functional Requirements) and mitigate or eliminate risks are compliance enablers
    * Compliance Enablers: are activities or work items designed to ensure that the system, product, or process adheres to external regulations, standards, or internal policies (e.g., ISO standards, GDPR, FDA, etc.).
    * Compliance Enablers might involve documentation, audits, testing, or specific tasks required to meet compliance.
    * **Timeboxed Compliance Enablers as Features:**
      * When compliance enablers are too large or complex to be completed by a single team, they are identified as features.
      * These features are prioritized and planned at the ART level during PI planning.
      * The ART, which consists of multiple teams, works collaboratively to accomplish the compliance feature within the timebox of the PI
      * Example: Creating system-wide audit documentation or conducting a security review that affects multiple teams.
    * **Story-Sized Compliance Enablers for Individual Teams:**
      * When compliance enablers are small enough to be handled by a single team, they are sized as stories.
      * These story-sized enablers are assigned to individual teams within the ART and handled during their regular iteration planning.
      * Example: Updating a specific component's logging to meet compliance standards, which can be completed by one team.
* Technical debt reduction
  * SAFe categorizes Flow Framework® debt items as enablers.
    * compliance enablers -> Flow Framework® risks, other types of enablers help to maintain the architecture
    * Infra enablers -> enhance the development process (include the incorporation & maintain of automation for testing and deployment)
    * Arch enablers -> improve the architecture (technical foundation) of the ARTS's product or solution -> help teams build systems that are scalable, maintainable, and extensible
      * The outcome of arch enablers is Architectural Runway (collection of existing technical capabilities and architectural enablers that provide the foundation for developing and deploying future business features.) -> when these enablers are in place, future features can be developed quickly (the runway)
    * Exploration enablers -> allow team members of the ART to research unknown technologies or determine  the best functional approach.
To track the progress of these Flow Items, Kersten found 4 Flow Metrics®.
 * Flow Velocity®
   * Number of Flow Items completed in a unit of time
 * Flow Time®
   * Time to complete an individual Flow Item
 * Flow Load®
   * Measure of WIP
 * Flow Efficiency®
   * Ratio of active time to Flow Time®

And an additional type:
* Flow Distribution® looks at the number of Flow Items complete for a value stream and measures each  type of Flow Item as a percentage of the total number of Flow Items.