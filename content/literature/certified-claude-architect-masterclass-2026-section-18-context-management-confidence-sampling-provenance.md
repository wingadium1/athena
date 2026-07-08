---
title: "Section 18: Context Management - Confidence, Sampling & Provenance"
type: literature
source: "certified-claude-architect-masterclass-2026"
author: "Jacob Bushong"
date-read: 2026-07-07
tags: [claude, udemy, course-section]
---

## Summary

## Key Ideas

L1: **Field-level confidence** — each extracted field gets its own score (not one per record). Enables granular routing.

L1: **Routing threshold** — fields above cutoff auto-accepted (flow downstream); below cutoff queued for human review.

L1: **Record-level confidence problem** — aggregate masks weak spots. Example: record scores 0.85, but date field is 0.40.

L1: **Record-level routing** — entire record sent to human review if any field is below threshold. Simpler but wastes reviewer effort on already-correct fields.

L1: **Field-level routing** — each field routed independently. Reviewer only touches fields needing help.

L1: **Schema design** — each field object has `value` (extracted content) and `confidence` (score 0.0–1.0). Model populates both in one call.

L1: **Field selection** — not every field must carry a confidence score; choose based on downstream risk.

L1: **High-stakes fields** — dates, monetary amounts, entity identifiers typically need confidence.

L1: **Borderline zone** — some systems add a secondary check for scores near the threshold.

L1: **Mixed confidence example** — name 0.95, total 0.88, date 0.38 → name & amount auto-accepted, date→review.

L1: **Failure mode** — teams trust confidence scores without calibration. Model may report 0.90 on a field where it's only 60% correct, letting errors flow into production.

L1: **Calibration** — verifying whether scores match real-world accuracy on labeled data.

L2: **Validation set**: held-out labeled samples used to measure extraction accuracy, never used for training or prompt adjustment.

L2: **Calibration**: a model is calibrated when its reported confidence matches its actual accuracy (e.g., 0.90 → correct 90% of the time).

L2: **Reliability diagram**: plots model's reported confidence (x-axis) vs empirical accuracy (y-axis); perfect = diagonal; above diagonal = overconfident; below = underconfident.

L2: **Overconfidence is more dangerous** than underconfidence — an overconfident model silently feeds errors into auto-accept (says 0.90 but only 60% correct).

L2: **Underconfidence** wastes reviewer capacity (says 0.60 but actually 90% correct).

L2: **Four-step calibration check**: (1) group predictions into 0.1-width confidence buckets; (2) compute empirical accuracy per bucket; (3) plot bucket midpoint vs accuracy with diagonal; (4) identify gaps.

L2: **Routing threshold**: start from policy's accuracy target (e.g., 95%) on y-axis, trace horizontally to calibration curve, read down to x-axis — fields at/above threshold auto-accept, below go to review.

L2: **Building a validation set**: representative production samples (not cherry-picked) + ground-truth labels from human experts; must stay held out.

L2: **Calibration is a recurring operational task** — model upgrades or prompt changes require rerunning on the same validation set.

L3: Stratified sampling deliberately oversamples outputs most likely wrong — weighting by low confidence, rare categories, and high stakes.

L3: Random sampling (e.g., 1% of 10K outputs/day) dilutes signal: ~95 correct vs ~5 errors per 100 reviews, so most reviewer effort is wasted.

L3: Three stratification dimensions: (1) confidence — tunable threshold (e.g., 0.70) below which sampling rate rises; (2) category balance — per-category minimum quotas prevent high-volume categories from crowding out rare but risky ones; (3) stakes — some outputs require human review regardless of confidence because error consequences are severe.

L3: Combining dimensions: confidence × category (prioritize low confidence AND underrepresented categories); confidence × stakes (high stakes → always review); full three-way via scoring function.

L3: Reviewer burnout is a system design failure (not a people problem): processing mostly correct outputs recalibrates cognitive thresholds → reviewers approve carelessly. Stratification counters this by raising error density in the review queue.

L3: Content moderation example: 5 categories; rare category has 20% error rate vs 2% for common ones. Random sampling under-invests in the highest risk outputs. Solution: per-category sample quotas.

L4: **Purpose of Review Queue**: Dedicated work surface where low-confidence outputs land for human inspection before reaching downstream consumers.

L4: **Routing Decision**: Model confidence score + stakes determine path — low confidence or high stakes → queue; high confidence/low stakes → continue directly downstream.

L4: **Queue Priority Ordering**: Items sorted by lowest confidence + highest stakes first.

L4: **Three Reviewer Actions (exactly three)**: **Approve** — output correct, released downstream, logged with timestamp + reviewer. **Edit** — output wrong, reviewer provides corrected version; original+correction pair feeds future training. **Reject** — output wrong, no correction; item discarded, input type flagged for analysis.

L4: **Design Principles**: (1) Single action surface — input, output, confidence, suggested fix on one screen; no tab switches. (2) Context first. (3) Queue priority automatic. (4) Audit trail on every action.

L4: **Bypass Policy**: Rule letting certain outputs skip the queue; must be logged and audited like a normal review action. Three scenarios — emergency override, high-confidence floor bypass, reviewer unavailability (fallback with warning flag).

L4: **Operational Metrics**: Queue depth (items waiting) and queue latency (wait time before review).

L4: **Feedback Loops**: Approve → positive training signal. Edit → high-value corrected pair from real production data. Reject → identifies poorly-handled input classes.

L4: **Synchronous vs Asynchronous Review**: Synchronous — downstream waits, pipeline stalls if no reviewer. Asynchronous — placeholder returned immediately, deferred fulfillment.

L5: **Provenance tracking** attaches a verifiable source pointer to every claim in synthesized output — each sentence needs a traceable origin.

L5: **Claim-to-source mapping** is the data structure linking each output claim to the specific source documents it came from.

L5: **Inline citations** expose claim-to-source mapping in readable output next to the claim, unlike end-of-paragraph bibliographies.

L5: **Core problem**: LLMs synthesize multiple sources into fluent prose but make sources invisible; reviewers can't tell which sentence came from which document.

L5: **Inline vs end-of-paragraph**: inline = per-claim accountability; end-of-paragraph = paragraph-level at best.

L5: **High-stakes synthesis** (regulatory filings, clinical summaries, legal analysis) needs inline citations. End-of-paragraph is acceptable only when the cost of a wrong claim is low.

L5: **Schema** — each record holds: (1) claim text, (2) list of source identifiers, (3) optional confidence score. Claims become first-class data objects (queryable, auditable, independently updatable).

L5: **Four reasons for provenance tracking**: (1) Trust, (2) Auditability, (3) Compliance (GDPR, EU AI Act), (4) Error correction.

L5: **Verification workflow**: sample-based, covers 10-15% of claims. Catches fabricated citations and distorted paraphrases.

L5: **Source identifier** must be unique and stable — unstable identifiers break the provenance trail.

L5: **Fabricated citation** — the model invents a source that doesn't match actual content or doesn't exist. Most dangerous failure mode in synthesis.

L5: **Fix**: every citation must be verified against the actual source before output is finalized.

L6: **Conflict Annotation**: Practice of explicitly surfacing disagreement in synthesis output rather than hiding it from the reader.

L6: **Resolution Strategies** (three options): (1) Prefer Newer — more recent source wins (good for regulations, versions, pricing). (2) Prefer Authoritative — designated high-trust sources outrank others (requires predefined authority hierarchy). (3) Escalate to Human — when neither rule applies; expensive but necessary for high stakes.

L6: **Averaging Anti-Pattern**: System silently blends or picks one conflicting source without logging the choice. Reader gets a confident-sounding claim with no indication a choice was made; the discarded source may have been more accurate or authoritative.

L6: **Conflict Detection** (upstream step, before synthesis): (1) Semantic similarity — find passages discussing the same topic. (2) Entailment checking — test if passage contradicts, supports, or is unrelated. Semantically similar + entailment says contradict → conflict.

L6: **Conflict Log**: Persistent record of every detected disagreement — logs conflicting claims, source IDs, resolution strategy applied, and selected/discarded claims. Without it, a confident claim is assumed to be consensus; with it, an auditable trail exists.

L6: **Presentation by Stakes**: High stakes → surface inline next to claim. Medium → footnote or expandable. Full review → conflict summary at end.

L6: **Key Aphorism**: "Synthesis without provenance is opinion, not analysis."

## Quotes

## My Take

## Links
