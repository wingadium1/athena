---
draft: true
---
# Permanent Notes

_Last updated: 2026-05-06 (ingest: Danh xưng làng Bắc Bộ — Kẻ, Xá và biến thiên lịch sử)_

| Note                                                                                              | Domain              | Summary                                                                                 |
| ------------------------------------------------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------- |
| [[permanent/llm-wiki-pattern\|LLM Wiki Pattern]]                                                  | PKM/LLM             | Pattern xây dựng knowledge base với LLM làm maintainer                                  |
| [[permanent/rag-vs-compiled-knowledge\|RAG vs Compiled Knowledge]]                                | PKM/LLM             | So sánh RAG (re-derive mỗi lần) vs compiled knowledge (tích lũy)                        |
| [[permanent/zettelkasten\|Zettelkasten]]                                                          | PKM                 | Phương pháp atomic notes + linking của Niklas Luhmann                                   |
| [[permanent/cip-counterpart-fund-viet-nam\|CIP và Counterpart Fund]]                              | Lịch sử/Kinh tế     | Cơ chế ẩn lương QLVNCH trong viện trợ kinh tế Mỹ — 1955–1975                            |
| [[permanent/map-vien-tro-vu-khi-qlvnch\|MAP và EDA — Định Giá Thấp Vũ Khí Viện Trợ]]              | Lịch sử/Kinh tế     | Structural underreporting vũ khí viện trợ qua cơ chế EDA book value                     |
| [[permanent/lau-nam-goc-qua-giam-sat-quoc-hoi\|Lầu Năm Góc và Giám Sát Quốc Hội]]                 | Lịch sử/Chính trị   | Bức tranh tổng thể về khoảng trống oversight của Quốc Hội Mỹ trong Vietnam War          |
| [[permanent/ming-tax-base-erosion\|Nhà Minh — Sụp Đổ Cơ Sở Thuế Ruộng Đất]]                       | Lịch sử/Kinh tế     | Guiji/touxian: đất thoát sổ thuế qua đặc quyền gentry; nửa diện tích mất trong 140 năm  |
| [[permanent/ming-silver-inflation\|Nhà Minh — Lạm Phát Bạc và Bẫy Tài Chính]]                     | Lịch sử/Kinh tế     | Manila Galleon, bạc mất giá, Nhất điều tiên gắn ngân sách vào bạc; cú sốc kép 1630s     |
| [[permanent/tu-giac-nuoc\|Tứ Giác Nước — Mô Hình Đô Thị Sông Nước Việt Nam]]                      | Lịch sử/Đô thị      | Kinh đô VN cổ 3–4 mặt sông; so sánh bối sơn diện thủy TQ và mô hình 1 mặt sông châu Âu  |
| [[permanent/openstack-neutron-overlay-protocols\|OpenStack Overlay Protocols (VXLAN/GRE/GENEVE)]] | Infrastructure      | Overlay tunnels giải quyết VLAN limit; VNI allocation và OVS flow pipeline              |
| [[permanent/openstack-external-network-mapping\|OpenStack External Network Mapping]]              | Infrastructure      | Provider networks kết nối vào tenant overlays qua OVS bridge + router namespace         |
| [[permanent/openstack-dvr-architecture\|DVR — Distributed Virtual Router]]                        | Infrastructure      | Phân tán L3 routing ra compute nodes; east-west + floating IP north-south               |
| [[permanent/openstack-floating-ip-nat\|Floating IP NAT Mechanism]]                                | Infrastructure      | DNAT/SNAT iptables rules; centralized vs. DVR models                                    |
| [[permanent/openstack-bgp-evpn-external\|BGP/EVPN External Connectivity in OpenStack]]            | Infrastructure      | Dynamic routing at scale: neutron-dynamic-routing, BGPVPN, ovn-bgp-agent                |
| [[permanent/kolla-ansible-deployment-patterns\|Kolla-Ansible Deployment Patterns]]                | Infrastructure      | Multi-node deploy; hostname resolution trap; RAM requirements; nova cell_v2 discovery   |
| [[permanent/ovs-bridge-management-nic-pitfall\|OVS Bridge Management NIC Pitfall]]                | Infrastructure      | OVS L2 takeover làm mất SSH; recovery qua noVNC; rule: chỉ provider NIC vào br-ex       |
| [[permanent/openstack-provider-net-routing\|OpenStack Provider Net Routing (No Floating IP)]]     | Infrastructure      | Static route từ jump host qua ctrl-1; gateway IP phải trên br-ex                        |
| [[permanent/packer-openstack-image-pipeline\|Packer OpenStack Image Pipeline]]                    | Infrastructure      | Packer + Glance; use_floating_ip=false; image lifecycle candidate→approved              |
| [[permanent/openstack-trove-guest-agent-connectivity\|Trove Guest Agent Connectivity (OVN)]]      | Infrastructure      | OVN logical IP vs Linux IP; RabbitMQ dual listener; DNAT Keystone; SNAT internet        |
| [[permanent/openstack-trove-postgresql-ha\|Trove PostgreSQL HA Replication]]                      | Infrastructure      | Primary/replica via --replica-of; Swift mandatory; security group gotcha; failover      |
| [[permanent/patroni\|Patroni]]                                                                    | Infrastructure/DB   | HA orchestrator cho PostgreSQL; Raft consensus qua etcd; automatic failover             |
| [[permanent/pgpool-ii\|Pgpool-II]]                                                                | Infrastructure/DB   | Connection proxy: pooling, read load balancing, write routing tới primary               |
| [[permanent/postgresql-ha-patroni-pgpool-combo\|PostgreSQL HA: Patroni + Pgpool-II]]              | Infrastructure/DB   | Combined pattern: Patroni lo failover, pgpool-II lo routing — không overlap             |
| [[permanent/swift-single-node-setup-kolla\|Swift Single-Node Setup (Kolla)]]                      | Infrastructure      | Loopback disk; GPT label KOLLA_SWIFT_DATA; XFS label d0; ring builder replicas=1        |
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
| [[permanent/y-bat\|Y Bát (衣鉢)]]                                                                 | Phật giáo/Văn hóa   | Áo cà sa + bình bát; biểu tượng giới luật, truyền thừa chánh pháp, lối sống giản dị     |
| [[permanent/guom-kiem-dao-vu-khi-dai-viet\|Gươm, Kiếm, Đao — Vũ khí Đại Việt]]                    | Lịch sử/Văn hóa     | Taxonomy vũ khí lạnh Việt Nam; từ nguyên gươm/kiếm; Hồ Gươm; so sánh dao/jian TQ        |
| [[permanent/danh-xung-lang-bac-bo\|Danh xưng làng Bắc Bộ qua các thời kỳ]]                        | Lịch sử/Văn hóa     | Kẻ (tiền Bắc thuộc) → Xá (Bắc thuộc) → Hán-Việt (TK X+); nghề làng, phân bố địa lý      |
