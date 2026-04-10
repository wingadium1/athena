---
title: "Feature Flag"
aliases: ["feature flag", "feature switch", "feature toggle", "feature gate"]
tags: [devops, cicd, deployment, progressive-delivery]
created: 2026-04-10
updated: 2026-04-10
---

Feature flag là kỹ thuật cho phép bật/tắt một tính năng trong ứng dụng mà không cần deploy lại code. Về bản chất, nó tách biệt hai việc thường đi cùng nhau: **deployment** (code lên production) và **release** (tính năng visible với user).

Khi code được deploy nhưng feature flag tắt, tính năng mới tồn tại trong production nhưng không ai thấy. Khi team sẵn sàng, chỉ cần bật flag — không cần deploy lại, không có downtime, và có thể rollback ngay lập tức bằng cách tắt flag trở lại.

## Các use case chính

- **Progressive rollout**: bật dần cho 1% → 10% → 100% user (canary release)
- **A/B testing**: hai nhóm user thấy hai phiên bản khác nhau
- **Kill switch**: tắt ngay một tính năng đang gây lỗi production mà không cần hotfix
- **Environment targeting**: bật cho internal users hoặc beta testers trước

## Liên quan đến Continuous Deployment

Feature flags là enabler quan trọng của Continuous Deployment: team có thể deploy liên tục mà không lo "feature chưa xong bị lộ ra" — flag đảm bảo chỉ những gì sẵn sàng mới visible. Đây cũng là cơ chế giúp tách biệt deploy từ release theo nguyên tắc của continuous delivery.

Tools phổ biến: LaunchDarkly, Flagsmith, CloudBees Feature Management, Unleash.

## Connections

- [[permanent/continuous-deployment]] — feature flags cho phép deploy liên tục an toàn
- [[permanent/continuous-delivery]] — tách deploy khỏi release là nguyên tắc cốt lõi của CD
- [[permanent/devops-pipeline-stages]] — feature flags nằm trong Release stage của pipeline

## Sources

- [[refs/feature_flag]]
