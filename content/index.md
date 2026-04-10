# Wiki Index

_Last updated: 2026-04-10 (refactor: migrate journal/ — 5 permanent notes created from journal/ cluster migration)_

Catalog của toàn bộ Athena Wiki. AI agent cập nhật file này sau mỗi lần ingest.

---

## Permanent Notes

| Note                                                                                              | Domain              | Summary                                                                                 |
| ------------------------------------------------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------- |
| [[permanent/llm-wiki-pattern\|LLM Wiki Pattern]]                                                  | PKM/LLM             | Pattern xây dựng knowledge base với LLM làm maintainer                                  |
| [[permanent/rag-vs-compiled-knowledge\|RAG vs Compiled Knowledge]]                                | PKM/LLM             | So sánh RAG (re-derive mỗi lần) vs compiled knowledge (tích lũy)                        |
| [[permanent/zettelkasten\|Zettelkasten]]                                                          | PKM                 | Phương pháp atomic notes + linking của Niklas Luhmann                                   |
| [[permanent/cip-counterpart-fund-viet-nam\|CIP và Counterpart Fund]]                              | Lịch sử/Kinh tế     | Cơ chế ẩn lương QLVNCH trong viện trợ kinh tế Mỹ — 1955–1975                            |
| [[permanent/map-vien-tro-vu-khi-qlvnch\|MAP và EDA — Định Giá Thấp Vũ Khí Viện Trợ]]              | Lịch sử/Kinh tế     | Structural underreporting vũ khí viện trợ qua cơ chế EDA book value                     |
| [[permanent/lau-nam-goc-qua-giam-sat-quoc-hoi\|Lầu Năm Góc và Giám Sát Quốc Hội]]                 | Lịch sử/Chính trị   | Bức tranh tổng thể về khoảng trống oversight của Quốc Hội Mỹ trong Vietnam War          |
| [[permanent/openstack-neutron-overlay-protocols\|OpenStack Overlay Protocols (VXLAN/GRE/GENEVE)]] | Infrastructure      | Overlay tunnels giải quyết VLAN limit; VNI allocation và OVS flow pipeline              |
| [[permanent/openstack-external-network-mapping\|OpenStack External Network Mapping]]              | Infrastructure      | Provider networks kết nối vào tenant overlays qua OVS bridge + router namespace         |
| [[permanent/openstack-dvr-architecture\|DVR — Distributed Virtual Router]]                        | Infrastructure      | Phân tán L3 routing ra compute nodes; east-west + floating IP north-south               |
| [[permanent/openstack-floating-ip-nat\|Floating IP NAT Mechanism]]                                | Infrastructure      | DNAT/SNAT iptables rules; centralized vs. DVR models                                    |
| [[permanent/openstack-bgp-evpn-external\|BGP/EVPN External Connectivity in OpenStack]]            | Infrastructure      | Dynamic routing at scale: neutron-dynamic-routing, BGPVPN, ovn-bgp-agent                |
| [[permanent/devops\|DevOps]]                                                                      | DevOps              | Văn hóa + thực hành kết hợp Dev và Ops để rút ngắn delivery cycle                       |
| [[permanent/site-reliability-engineering\|Site Reliability Engineering (SRE)]]                    | DevOps              | Google model: dùng software engineering để giải quyết vấn đề operations                 |
| [[permanent/slsa\|SLSA — Supply-chain Levels for Software Artifacts]]                             | DevOps/Security     | Framework bảo vệ software supply chain theo 4 levels                                    |
| [[permanent/cicd\|CI/CD]]                                                                         | DevOps              | Cặp thực hành tự động hóa tích hợp và delivery liên tục                                 |
| [[permanent/continuous-integration\|Continuous Integration (CI)]]                                 | DevOps              | Tự động hóa build, test sau mỗi commit; triết lý shift-left                             |
| [[permanent/continuous-delivery\|Continuous Delivery (CD)]]                                       | DevOps              | Đảm bảo codebase luôn release-ready; release là quyết định của người                    |
| [[permanent/continuous-deployment\|Continuous Deployment]]                                        | DevOps              | Mọi commit pass pipeline đều tự động lên production                                     |
| [[permanent/devops-topology\|DevOps Topology]]                                                    | DevOps              | 9 mô hình tổ chức nhóm DevOps + 8 anti-patterns cần tránh                               |
| [[permanent/devops-pipeline-stages\|DevOps Pipeline Stages]]                                      | DevOps              | 9 stage từ Plan đến Monitor; shift-left security, feature flags                         |
| [[permanent/dora-metrics\|DORA Metrics]]                                                          | DevOps/Metrics      | 4 metrics đo velocity + stability của DevOps organization                               |
| [[permanent/mean-time-to-restore\|Mean Time to Restore (MTTR)]]                                   | DevOps/Metrics      | Thời gian phục hồi sau incident; cặp với Change Failure Rate trong DORA                 |
| [[permanent/feature-flag\|Feature Flag]]                                                          | DevOps              | Tách deployment khỏi release; progressive rollout và kill switch                        |
| [[permanent/flow-framework\|Flow Framework]]                                                      | Flow/Metrics        | 4 Flow Items + 5 Flow Metrics đo hiệu suất value stream (Mik Kersten)                   |
| [[permanent/cost-of-delay\|Cost of Delay]]                                                        | Flow/Product        | Giá trị kinh tế bị mất do trì hoãn delivery; nền tảng của CD3 prioritization            |
| [[permanent/little-s-law\|Little's Law]]                                                          | Flow/Math           | L = λW: mối quan hệ WIP, throughput, cycle time trong hệ thống ổn định                  |
| [[permanent/kingmans-formula\|Kingman's Formula]]                                                 | Flow/Math           | VUT equation: utilization + variability → wait time tăng phi tuyến                      |
| [[permanent/time-to-market\|Time to Market (TTM)]]                                                | Product/Metrics     | Từ ý tưởng đến tay khách hàng; rộng hơn Lead Time for Changes                           |
| [[permanent/pirate-metrics\|Pirate Metrics (AARRR)]]                                              | Product/Growth      | 5 stages vòng đời khách hàng: Acquisition → Activation → Retention → Referral → Revenue |
| [[permanent/safe\|SAFe — Scaled Agile Framework]]                                                 | Agile/SAFe          | Framework tích hợp Lean, Agile, DevOps cho Business Agility; 4 cấu hình, 10 nguyên tắc  |
| [[permanent/safe-planning-interval\|SAFe Planning Interval (PI)]]                                 | Agile/SAFe          | Khung thời gian cadence-based 8–12 tuần; ART deliver value theo từng PI                 |
| [[permanent/safe-ip-iteration\|SAFe IP Iteration]]                                                | Agile/SAFe          | Innovation & Planning iteration cuối PI: buffer, hackathon, PI Planning, I&A            |
| [[permanent/safe-enablers\|SAFe Enablers]]                                                        | Agile/SAFe          | Backlog items mở rộng architecture runway: Exploration, Architectural, Infrastructure   |
| [[permanent/safe-value-stream\|SAFe Value Stream]]                                                | Agile/SAFe          | Operational vs Development Value Stream; tổ chức xung quanh value theo principle #10    |
| [[permanent/value-stream-mapping\|Value Stream Mapping (VSM)]]                                    | Lean/Flow           | Lean tool visualize toàn bộ flow; identify waste qua PT, LT, %C&A metrics               |
| [[permanent/safe-agile-release-train\|SAFe Agile Release Train (ART)]]                            | Agile/SAFe          | Team of teams long-lived; cross-functional; delivery unit chính trong Essential SAFe    |
| [[permanent/safe-system-team\|SAFe System Team]]                                                  | Agile/SAFe          | DevOps team của ART; owns CI/CD pipeline, integration, và demo environment              |
| [[permanent/agile-manifesto\|Agile Manifesto]]                                                    | Agile               | 4 values + 12 principles nền tảng Agile Software Development (2001)                     |
| [[permanent/agile-cadence\|Agile Cadence]]                                                        | Agile               | Nhịp điệu đều đặn của events; nền tảng cho PI planning và team synchronization          |
| [[permanent/calmr\|CALMR]]                                                                        | DevOps/SAFe         | 5 trụ cột DevOps trong SAFe: Culture, Automation, Lean Flow, Measurement, Recovery      |
| [[permanent/scaling-devops-with-safe\|Scaling DevOps với SAFe]]                                   | DevOps/SAFe         | Essential SAFe, ART, 3 roles (RTE/PM/SA), và 4 khía cạnh DevOps theo CALMR              |
| [[permanent/devops-transformation-canvas\|DevOps Transformation Canvas]]                          | DevOps/SAFe         | Workshop tool 9 components; current/future state VSM để identify và improve bottleneck  |
| [[permanent/westrum-culture-typology\|Westrum Culture Typology]]                                  | Culture/Leadership  | 3 kiểu văn hóa (Pathological/Bureaucratic/Generative); xử lý thông tin bất thường       |
| [[permanent/continuous-learning-culture\|Continuous Learning Culture (CLC)]]                      | Culture/Learning    | 3 dimensions + 5 Senge disciplines; Business Agility competency trong SAFe              |
| [[permanent/kotter-8-step-change\|Kotter's 8-Step Change Model]]                                  | Leadership/Change   | 8 bước transformation tổ chức; urgency → coalition → vision → embed vào culture         |
| [[permanent/machine-learning\|Machine Learning]]                                                  | ML/AI               | Thuật toán học từ dữ liệu; supervised vs unsupervised; overfitting là vấn đề core       |
| [[permanent/k-means-clustering\|K-Means Clustering]]                                              | ML/Algorithm        | Thuật toán phân cụm unsupervised; K centroids, iterative convergence, elbow method      |
| [[permanent/naive-bayes-classifier\|Naive Bayes Classifier]]                                      | ML/Algorithm        | Supervised classifier dựa trên Bayes theorem; naive independence assumption             |
| [[permanent/overfitting-underfitting\|Overfitting và Underfitting]]                               | ML                  | Hai thái cực của generalization; K-fold cross-validation để detect                      |
| [[permanent/theory-of-constraints\|Theory of Constraints (TOC)]]                                  | Lean/Flow           | Hệ thống bị giới hạn bởi constraint; 5 focusing steps; nền tảng lý thuyết của VSM       |
| [[permanent/seekable-oci\|Seekable OCI (SOCI)]]                                                   | Infrastructure      | AWS lazy-loading container images; SOCI index; faster cold start cho Fargate/ECS/EKS    |
| [[permanent/resilience-vs-robustness\|Resilience vs Robustness]]                                  | DevOps/Reliability  | MTBF vs MTTR trade-off; 4 Resilience Engineering capabilities; transition path to CD    |
| [[permanent/tail-latency\|Tail Latency]]                                                          | Distributed Systems | P99 latency outliers; compound effect in microservices; percentile monitoring           |
| [[permanent/hedged-requests\|Hedged Requests]]                                                    | Distributed Systems | Client-side pattern: race multiple instances, use first response to reduce tail latency |
| [[permanent/china-data-regulation\|China Data Regulation]]                                        | Compliance          | CSL + DSL + PIPL; data classification (Core/Important/General); cross-border transfer   |
| [[permanent/kubernetes-node-pool-design\|Kubernetes Node Pool Design]]                            | Kubernetes          | Taint/toleration + affinity; workload segregation; cost optimization by pool type       |
| [[permanent/kubernetes-node-consolidation\|Kubernetes Node Consolidation]]                        | Kubernetes          | Minimal node count; Cluster Autoscaler + overprovisioning; trade-off stateful/variable  |
| [[permanent/zero-trust-network-kubernetes\|Zero Trust Network in Kubernetes]]                     | Kubernetes/Security | mTLS STRICT + ServiceAccount identity; Calico + Istio sidecar vs Ambient mode           |
| [[permanent/container-runtime-security-falco\|Container Runtime Security (Falco)]]                | Kubernetes/Security | Syscall monitoring; kernel module + rule-based alerting; last line of defense           |
| [[permanent/multi-cloud-architecture\|Multi-Cloud Architecture]]                                  | Cloud/Architecture  | Private cloud + AWS hybrid; K8s portability; GitOps; distributed storage; trade-offs    |
| [[permanent/lossless-semantic-tree\|Lossless Semantic Tree (LST)]]                                | Testing             | Pre-test analysis: full map of branches, deps, data flows; zero-call verification       |
| [[permanent/ai-assisted-testing\|AI-Assisted Testing]]                                            | Testing/AI          | LST as structured context for AI; human (analysis) + AI (implementation) multiplier     |
| [[permanent/software-architect-role\|Software Architect Role]]                                    | Architecture/Career | Architect vs developer: trade-off decisions; cost-awareness; ADR; Conway's Law          |
| [[permanent/system-design-patterns\|System Design Patterns]]                                      | Architecture        | 8 core patterns: consistent hashing, CAP, sharding, caching, CDN, partitioning, queues  |
| [[permanent/cassandra-kubernetes-ha\|Cassandra HA on Kubernetes]]                                 | Kubernetes/Database | DC = AZ mapping; StatefulSet per AZ; NetworkTopologyStrategy; Spot-safe HA design       |
| [[permanent/t-shaped-developer\|T-Shaped Developer]]                                              | Career/Agile        | Broad + deep skillset; cross-functional team enabler; contrast with I-shaped specialist |
| [[permanent/aws-lambda-cold-start\|AWS Lambda Cold Start]]                                        | AWS/Serverless      | Cold vs warm start; execution context reuse; keep-alive, /tmp caching optimizations     |
| [[permanent/git-object-model\|Git Object Model]]                                                  | Git/Internals       | blob/tree/commit DAG; refs + logs; content-addressed recovery pattern                   |

---

## Literature Notes

| Note                                                                                              | Source                             | Date Read  |
| ------------------------------------------------------------------------------------------------- | ---------------------------------- | ---------- |
| [[literature/karpathy-llm-wiki\|LLM Wiki — Karpathy]]                                             | Andrej Karpathy (gist)             | 2026-04-08 |
| [[literature/tetsu-kasuya-46-method\|Phương pháp 4:6 — Tetsu Kasuya]]                             | Project Barista                    | 2026-04-08 |
| [[literature/dang-phong-21-nam-vien-tro-my\|21 Năm Viện Trợ Mỹ ở VN]]                             | Đặng Phong (sách)                  | 2026-04-08 |
| [[literature/openstack-neutron-external-connectivity\|OpenStack Neutron Docs]]                    | OpenStack official docs            | 2026-04-09 |
| [[literature/openstack-neutron-overlay-research-2026-04\|OpenStack Overlay Protocols — Research]] | OpenStack Docs, RFC 7348, RFC 8926 | 2026-04-09 |

---

## Maps of Content

- [[maps/map-of-openstack-networking\|OpenStack Networking]] — Overlay, DVR, Floating IP, BGP/EVPN

---

## Legacy (refs/, til/, journal/)

_Preserved as-is. Linked into permanent notes gradually._

| File                                                                              | Type    | Topic              |
| --------------------------------------------------------------------------------- | ------- | ------------------ |
| [[refs/The_DORA_KPI_metrics\|DORA KPI Metrics]]                                   | ref     | DevOps             |
| [[refs/devops_topology\|DevOps Topology]]                                         | ref     | DevOps             |
| [[refs/devops_pipelines_and_toolchains\|DevOps Pipelines]]                        | ref     | DevOps             |
| [[refs/ron_westrum_3_types_of_cultures\|Ron Westrum — 3 Culture Types]]           | ref     | Culture/Leadership |
| [[refs/ron_westrum_generative_culture\|Generative Culture]]                       | ref     | Culture/Leadership |
| [[refs/continuous_learning\|Continuous Learning]]                                 | ref     | Learning           |
| [[refs/machine_learning\|Machine Learning]]                                       | ref     | ML                 |
| [[refs/unsupervised_learning\|Unsupervised Learning]]                             | ref     | ML                 |
| [[refs/naive_bayes_classifier\|Naive Bayes Classifier]]                           | ref     | ML                 |
| [[refs/multinomial_distribution\|Multinomial Distribution]]                       | ref     | ML/Math            |
| [[refs/safe_pi\|SAFe PI]]                                                         | ref     | Agile/SAFe         |
| [[refs/safe_enabler\|SAFe Enabler]]                                               | ref     | Agile/SAFe         |
| [[refs/safe_value_stream_mapping\|SAFe Value Stream Mapping]]                     | ref     | Agile/SAFe         |
| [[refs/safe_operational_value_stream\|SAFe Operational Value Stream]]             | ref     | Agile/SAFe         |
| [[refs/safe_metrics_in_value_stream_mapping\|SAFe Metrics in VSM]]                | ref     | Agile/SAFe         |
| [[refs/TTM\|TTM]]                                                                 | ref     | Business           |
| [[refs/john_kotter_8_steps_driving_transform_of_a_culture\|Kotter 8-Step Change]] | ref     | Leadership         |
| [[journal/8_most_important_system_design_concepts\|System Design Concepts]]       | journal | System Design      |
| [[journal/cassandra_on_aws_eks_spot\|Cassandra on EKS Spot]]                      | journal | AWS/Infra          |
| [[journal/optimize_lambda_function\|Lambda Optimization]]                         | journal | AWS                |
| [[journal/how_to_recover_corrupted_git_repository\|Recover Corrupted Git Repo]]   | journal | Git                |
| [[journal/command_tips\|Command Tips]]                                            | journal | CLI                |
| [[journal/t_shaped\|T-Shaped Skills]]                                             | journal | Career             |
| [[til/TIL\|TIL]]                                                                  | til     | Misc               |
| [[cooking/cafe\|Cafe]]                                                            | cooking | Cooking            |
