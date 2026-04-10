---
title: "AWS Lambda Cold Start"
aliases: ["lambda cold start", "lambda execution context", "serverless cold start"]
tags: [AWS, Lambda, Serverless, Performance, Latency]
created: 2026-04-10
updated: 2026-04-10
---

Every Lambda invocation goes through two phases. The **cold start** covers everything before your code runs: downloading the deployment package, provisioning the execution environment, and initializing the runtime. The **warm start** is everything after — just your handler code executing. Cold starts are the visible latency spikes that appear periodically in production Lambda metrics.

**The 15-minute reuse window**: AWS freezes the execution context after a function returns and thaws it for the next invocation. This reuse window is roughly 15 minutes. Any object initialized outside the handler function persists across warm invocations within that window. This is the core optimization lever.

**Practical optimizations**:

1. _Move heavy initialization outside the handler_ — SDK client instantiation, database connections, and expensive computed values belong at module scope, not inside the handler. They run once per cold start, not once per request.

2. _Use keep-alive HTTP connections_ — By default, Node.js AWS SDK closes the underlying HTTPS connection after each request. Create an `https.Agent({ keepAlive: true })` and pass it as `httpOptions.agent` when instantiating DynamoDB/S3/etc. clients. This eliminates TCP handshake overhead on warm invocations.

3. _Cache SDK clients at module scope_ — Same principle: instantiate `new AWS.DynamoDB(...)` outside the handler so it's reused across invocations.

4. _Use `/tmp` for cross-invocation caching_ — Lambda provides 512 MB (now up to 10 GB) of ephemeral storage at `/tmp` that persists within the execution context lifetime. Store computed artifacts, downloaded config files, or ML model weights here to avoid re-fetching on every warm start.

5. _Mind background processes_ — If you fire async work inside the handler, ensure it completes before the handler returns. Frozen context can carry incomplete background work into the next invocation, causing subtle bugs.

**The key insight**: cold starts are unavoidable on the first invocation and after ~15 minutes of inactivity. Optimizing warm starts is where most of the latency wins live, because they represent the majority of invocations in any steady-traffic system.

## Connections

- [[permanent/seekable-oci]] — Related pattern: lazy-loading container images to reduce cold start for ECS/Fargate/EKS
- [[permanent/tail-latency]] — Cold starts manifest as tail latency spikes in Lambda-based systems

## Sources

- [[journal/optimize_lambda_function]] — Original blog post (2019), based on Node Summit talk by Matt Lavin
