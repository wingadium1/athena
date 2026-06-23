---
draft: true
---

# Wiki Log

Append-only record of operations. Format: `## [YYYY-MM-DD] operation | description`

---

## [2026-06-23] capture | Claude Certified Architect (CCA-F) - Section 2, Lecture 4

Extracted 5 core concepts from lecture notes on Parallel Execution. Created permanent notes for Parallel Execution, Fan-out, Fan-in, Partial Failure Handling, and Merging Strategies. All notes are cross-linked and backlinked from the source literature note.

---

## [2026-06-23] capture | Claude Certified Architect (CCA-F) - Section 2, Lecture 3

Extracted 8 core concepts from lecture notes on Sequential Pipelines & Task Decomposition. Created permanent notes for Task Decomposition, Sequential Pipeline, Dependency Graph, Error Cascade, True vs. Artificial Dependency, Handoff Point, Orchestrator, and Over-Decomposition. All notes are cross-linked and backlinked from the source literature note.

---

## [2026-06-22] capture | Workflow-First Agentic Architecture

Added permanent note and backlinks from Claude section 1, software architect role, and LLM Wiki Pattern.

---

## [2026-06-22] capture | Claude Certified Architect (CCA-F) - 2026 Exam Prep — Section 1 complete

Section 1 summary finalized; tracker marked complete.

---

## [2026-06-22] capture | Claude Certified Architect (CCA-F) - 2026 Exam Prep — Section 1 started

Pages touched: 1 literature note (mới), literature-notes.md (cập nhật), wip tracker (cập nhật), index.md (cập nhật)

---

## [2026-04-08] ingest | 21 Năm Viện Trợ Mỹ ở Việt Nam — Đặng Phong

Key concepts added: cip-counterpart-fund-viet-nam, map-vien-tro-vu-khi-qlvnch, lau-nam-goc-qua-giam-sat-quoc-hoi
Pages touched: 1 literature note (mới), 3 permanent notes (mới), index.md (cập nhật)

Highlights:

- CIP counterpart fund: cơ chế ẩn lương QLVNCH trong viện trợ kinh tế — ngoài tầm giám sát Quốc Hội Mỹ
- MAP/EDA: structural underreporting vũ khí viện trợ qua depreciated book value vs replacement cost
- Bức tranh tổng thể: supplemental appropriations + Gulf of Tonkin deception + Pentagon Papers

---

## [2026-04-08] capture | Kinh nghiệm coldbrew tại nhà

Ghi lại journey: Hario Filter-in Bottle (vỡ) → Soriso 600ml (vấn đề filter nhỏ) → workaround túi lọc trà + fine robusta 1:10.
Pages touched: coldbrew-tai-nha.md (mới), pourover-setup-ca-nhan.md (cập nhật link), cafe.md (cập nhật MOC)

---

## [2026-04-08] ingest | Phương pháp 4:6 — Tetsu Kasuya + capture setup cá nhân

Key concepts added: tetsu-kasuya-46-method (literature), pourover-setup-ca-nhan (cooking)
Pages touched: 1 literature note, 1 cooking note (mới), cafe.md (cập nhật thành MOC), index.md

---

## [2026-04-08] ingest | LLM Wiki — Andrej Karpathy (gist)

Key concepts added: llm-wiki-pattern, rag-vs-compiled-knowledge, zettelkasten
Pages touched: 1 literature note, 3 permanent notes created, index.md updated

---

## [2026-04-08] setup | Khởi tạo Athena LLM Wiki

Chuyển đổi từ manual wiki sang AI-maintained wiki theo pattern của Karpathy.

**Thay đổi:**

- Tạo cấu trúc Zettelkasten: `content/fleeting/`, `content/literature/`, `content/permanent/`, `content/maps/`
- Tạo `raw/` directory cho sources (articles, books, assets)
- Viết `AGENTS.md` — schema + instructions cho AI agent
- Legacy folders (`refs/`, `til/`, `journal/`) được giữ nguyên, link dần vào permanent notes
- Tạo `index.md` catalog với danh sách legacy notes

**Notes hiện có:** ~100 files trong legacy folders
**Permanent notes:** 0 (chưa ingest source nào)

---

## [2026-04-09] ingest | OpenStack Neutron Overlay Networks — VXLAN/GRE/GENEVE research

Key concepts added: vlan-4094-limit, vxlan-vni-scalability, openstack-neutron-overlay-protocols, openstack-ml2-overlay-config
Pages touched: 1 literature note (mới), 4 permanent notes (mới), index.md (cập nhật)

Highlights:

- Vấn đề: 12-bit 802.1Q VID → 4094 VLAN limit, không đủ cho large-scale multi-tenant cloud
- VXLAN (RFC 7348): 24-bit VNI → 16.7M segments. UDP:4789. Fixed 8-byte header = limitation cho metadata
- GRE: point-to-point, không support OVN, poor ECMP. Đang fade out trong deployments mới
- GENEVE (RFC 8926): 24-bit VNI + variable-length TLV options. Default tunnel type của OVN. Cho phép in-band metadata, service chaining, transport security
- OVN + VXLAN caveat: OVN giảm identifier xuống 12-bit → chỉ còn 4096 networks (phủ nhận mục đích của VXLAN)
- Production recommendation 2026: GENEVE + OVN cho deployments mới; VXLAN + OVS + L2 population cho legacy
- Critical footgun: ml2_conf.ini max_header_size default=30, OVN cần >=38

---

## [2026-04-09] ingest | OpenStack Neutron external-to-tenant network connectivity (deep technical)

**Context**: Multi-tenant OpenStack deployment needing to map external networks into tenant overlay networks beyond VLAN 4094 limit.

Key concepts researched and filed:

- `openstack-overlay-networks` — VXLAN/GRE/GENEVE protocols, VNI allocation, OVS flow pipeline
- `openstack-external-network-mapping` — Provider network → router namespace → overlay bridge architecture
- `openstack-dvr-architecture` — DVR design, modes (dvr/dvr_snat/legacy), FIP namespace, IP consumption
- `openstack-floating-ip-nat` — iptables DNAT/SNAT in qrouter namespace, centralized vs. DVR path
- `openstack-bgp-evpn-external` — neutron-dynamic-routing, networking-bgpvpn, ovn-bgp-agent + FRR/EVPN

Pages touched: 5 permanent notes (mới), 1 literature note (mới), 1 MOC (mới), index.md (cập nhật)

Sources: OpenStack official docs (Neutron 25.x/28.x), Eran Gampel DVR blog, Red Hat DVR guide, networking-bgpvpn docs, ovn-bgp-agent docs, Red Hat EVPN/OpenShift article

Answer filed as permanent notes: YES (5 notes + MOC)

---

## [2026-04-10] refactor | Migrate refs/ Batch 1 — DevOps core

Source files archived: DevOps.md, cicd.md, continuous_integration.md, continuous_delivery.md, continuous_deployment.md, sre.md, slsa.md, devops_topology.md, devops_pipelines_and_toolchains.md, continuous_exploration.md (empty — archived only)

Permanent notes created (9):

- `devops` — DevOps culture, CI/CD relationship, SRE link
- `site-reliability-engineering` — SRE vs DevOps, SLO/SLI/Error Budget
- `slsa` — supply chain security framework, 4 levels
- `cicd` — CI/CD as linked pair
- `continuous-integration` — shift-left, CI vs CD vs Deployment comparison table
- `continuous-delivery` — release-ready state, vs Continuous Deployment
- `continuous-deployment` — fully automated pipeline to production, prerequisites
- `devops-topology` — 9 beneficial topologies + 8 anti-types
- `devops-pipeline-stages` — 9 stages, DevSecOps shift-left, feature flags

Pages touched: 9 permanent notes (mới), 10 refs/ files (archived), index.md (cập nhật)

---

## [2026-04-10] refactor | Migrate refs/ Batch 2 — DORA + Flow metrics

Source files archived: The_DORA_KPI_metrics.md, mean_time_to_recovery.md, pirate_metrics.md, flow_framework.md, don_reinertsen_cost_of_delay.md, kingman_s_formular.md, little_s_law.md, TTM.md, feature_flag.md

Permanent notes created (9):

- `dora-metrics` — 4 DORA metrics, velocity vs stability, elite/high/medium/low levels
- `mean-time-to-restore` — MTTR definition, relationship với CFR trong DORA
- `pirate-metrics` — AARRR framework, 5 customer lifecycle stages
- `flow-framework` — 4 Flow Items, 5 Flow Metrics, SAFe mapping (Mik Kersten)
- `cost-of-delay` — CoD concept, CD3 formula, Don Reinertsen
- `little-s-law` — L = λW, WIP/throughput/cycle time relationship
- `kingmans-formula` — VUT equation, utilization → wait time nonlinear growth
- `time-to-market` — TTM vs Lead Time for Changes, 5 business challenges
- `feature-flag` — deploy vs release separation, progressive rollout, kill switch

Pages touched: 9 permanent notes (mới), 9 refs/ files (archived), index.md (cập nhật)

---

## [2026-04-10] refactor | Migrate refs/ Batch 3 — SAFe + Agile

Source files archived (15): safe.md, safe_pi.md, safe_enabler.md, safe_ip_iteration.md, safe_value_stream.md, safe_value_stream_mapping.md, safe_operational_value_stream.md, safe_metrics_in_value_stream_mapping.md, scaling_devops_with_safe.md, agile_manifesto.md, agile_art.md, agile_art_system_team.md, agile_cadence.md, CALMR.md, devops_transforation_canvas.md

Permanent notes created (13):

- `safe` — SAFe framework, 4 core values, 10 principles, 4 configurations
- `safe-planning-interval` — PI cadence, 8–12 weeks, ART delivery unit
- `safe-ip-iteration` — IP iteration buffer, PI Planning, Inspect & Adapt
- `safe-enablers` — 4 enabler types: Exploration, Architectural, Infrastructure, Compliance
- `safe-value-stream` — Operational vs Development Value Stream; tổ chức xung quanh value theo principle #10
- `value-stream-mapping` — Lean VSM tool; PT/LT/%C&A metrics; waste identification
- `safe-agile-release-train` — ART as team of teams; cross-functional; common cadence
- `safe-system-team` — System Team as DevOps team of ART; CI/CD pipeline ownership
- `agile-manifesto` — 4 values, 12 principles; SAFe context notes
- `agile-cadence` — cadence vs sprint vs iteration; basis for PI synchronization
- `calmr` — CALMR 5 pillars; evolution from CMAS → CALMS → CALMR
- `scaling-devops-with-safe` — Essential SAFe, ART, 3 roles (RTE/PM/SA), và 4 khía cạnh DevOps theo CALMR
- `devops-transformation-canvas` — 9-component workshop tool; current/future state VSM

Merges applied:

- `safe_operational_value_stream.md` content merged into `safe-value-stream.md`
- `safe_metrics_in_value_stream_mapping.md` content merged into `value-stream-mapping.md`

Pages touched: 13 permanent notes (mới), 15 refs/ files (archived), index.md (cập nhật)

---

## [2026-04-10] refactor | Migrate refs/ Batch 4 — Leadership + Culture

Source files archived (12): ron_westrum_3_types_of_cultures.md, ron_westrum_generative_culture.md, ron_westrum_bureaucratic_culture.md, ron_westrum_pathological_culture.md, john_kotter_8_steps_driving_transform_of_a_culture.md, continuous_learning.md, clc_personal_mastery.md, clc_mental_models.md, clc_shared_vision.md, clc_system_thinking.md, clc_team_learning.md, follow_the_sun_model.md

Permanent notes created (3):

- `westrum-culture-typology` — 3 culture types (Pathological/Bureaucratic/Generative); 6 reactions to anomalies; DORA link
- `continuous-learning-culture` — CLC 3 dimensions; Senge's 5 disciplines merged; SAFe Business Agility competency
- `kotter-8-step-change` — 8 bước change management; urgency → coalition → vision → short-term wins → embed

Merges applied:

- 4 Westrum files (3 stubs + main) → `westrum-culture-typology.md`
- 6 CLC files (5 sub-disciplines + main) → `continuous-learning-culture.md`
- `clc_team_learning.md` (empty stub) → archived only, no permanent note
- `follow_the_sun_model.md` (empty stub) → archived only, no permanent note

Pages touched: 3 permanent notes (mới), 12 refs/ files (archived), index.md (cập nhật)

---

## [2026-04-10] refactor | Migrate refs/ Batch 5 — ML + Math

Source files archived (8): machine_learning.md, supervised_learning.md, unsupervised_learning.md, k_means_clustering.md, naive_bayes_classifier.md, multinomial_distribution.md, overfitting.md, theory_of_constraints.md

Permanent notes created (5):

- `machine-learning` — ML overview; supervised vs unsupervised; overfitting pointer (merged 3 files)
- `k-means-clustering` — K-Means algorithm; centroid initialization; elbow method; local minima warning
- `naive-bayes-classifier` — Bayes theorem; NBC algorithm; multinomial distribution merged in
- `overfitting-underfitting` — generalization problem; causes; K-fold cross-validation
- `theory-of-constraints` — 5 focusing steps; constraint thinking; links to VSM/Little's Law/Kingman

Merges applied:

- machine_learning + supervised_learning + unsupervised_learning → `machine-learning.md`
- multinomial_distribution → merged into `naive-bayes-classifier.md`

Pages touched: 5 permanent notes (mới), 8 refs/ files (archived), index.md (cập nhật)

---

## [2026-04-10] refactor | Migrate refs/ Batch 6 — Infra misc

Source files archived (1): Seekable_OCI.md

Permanent notes created (1):

- `seekable-oci` — SOCI technology; lazy loading; SOCI index; AWS Fargate use case

Pages touched: 1 permanent note (mới), 1 refs/ file (archived), index.md (cập nhật)

---

## [2026-04-10] refactor | Migrate writing/ — 12 permanent notes từ 9 cluster

Source files: writing/ (không archive — published content, read-only)

Permanent notes created (12):

- `resilience-vs-robustness` — Robustness (MTBF) vs Resilience (MTTR); Cynefin; 4 Hollnagel capabilities; transition path to CD
- `tail-latency` — P99 compound effect; latency outliers; percentile monitoring vs average
- `hedged-requests` — Race multiple service instances; first-response wins; trade-offs và when to use
- `china-data-regulation` — CSL/DSL/PIPL framework; Core/Important/General data classification; cross-border transfer rules
- `kubernetes-node-pool-design` — Taint/toleration + affinity/anti-affinity; pool segregation by workload type; cost strategies
- `kubernetes-node-consolidation` — Minimal node count; Cluster Autoscaler + overprovisioning; stateful/variable workload limitations
- `zero-trust-network-kubernetes` — mTLS STRICT; Calico + Istio sidecar mode; Istio Ambient mode; identity-based not IP-based
- `container-runtime-security-falco` — Syscall monitoring via kernel module; rule-based alerting; DevSecOps stack
- `multi-cloud-architecture` — Private cloud primary + AWS secondary; K8s portability; GitOps; Portworx/Ceph storage
- `lossless-semantic-tree` — Pre-test analysis methodology; zero-call verification; Mock Verification Pattern
- `ai-assisted-testing` — LST as AI context; human+AI multiplier model; structured prompts for better coverage
- `software-architect-role` — Developer vs Architect; trade-off decisions; cost-awareness; ADR; Architectural Knowledge Management

Pages touched: 12 permanent notes (mới), index.md (cập nhật), log.md (append)

---

## [2026-04-10] refactor | migrate journal/ — 5 permanent notes from journal/ cluster

Source files: journal/ (không archive — legacy personal posts, read-only)
Skipped: `note_for_project_aws_development.md` (link dump, no atomic concept), `command_tips.md` (tooling cheatsheet, not conceptual), `f_the_people_who_do_not_add_blank_line_at_end_of_file.md` (rant, no extractable concept), `todo.md` (1-line stub)

Permanent notes created (5):

- `system-design-patterns` — 8 core patterns: consistent hashing, CAP theorem, sharding, caching strategies, CDN, partitioning, message queues, indexes
- `cassandra-kubernetes-ha` — DC = AZ mapping; StatefulSet per AZ với nodeAffinity `topology.kubernetes.io/zone`; NetworkTopologyStrategy; seed nodes; Spot viable vì HA design
- `t-shaped-developer` — I-shaped vs T-shaped; cross-functional Agile teams; benefits; path (học + thời gian + kiên nhẫn + chia sẻ)
- `aws-lambda-cold-start` — Cold vs warm start lifecycle; 15-min execution context reuse; keep-alive HTTP, /tmp caching, move heavy objects outside handler
- `git-object-model` — blob/tree/commit DAG; SHA-1 content addressing; refs/ + logs/ structure; recovery by copying objects/ into fresh clone

Pages touched: 5 permanent notes (mới), index.md (cập nhật), log.md (append)

---

## [2026-04-10] ingest | Proxmox DBaaS Lab — Day 1 to Day 4.5

Source: `/Users/sonht2.gmo/git/openstack-101/lab/proxmox-dbaas-lab/` (command packs + live journal)
Owner emphasis: Trove + Swift + PG HA replication, Kolla-Ansible real incidents, OVS bridge pitfall

Permanent notes created (7):

- `kolla-ansible-deployment-patterns` — hostname resolution trap (127.0.1.1 / Erlang); RAM requirements; nova cell_v2 discover_hosts; kolla_toolbox snapshot gotcha
- `ovs-bridge-management-nic-pitfall` — OVS L2 takeover làm mất SSH; recovery via Proxmox noVNC; rule provider NIC only
- `openstack-provider-net-routing` — static route từ tools-1 qua ctrl-1; gateway IP phải trên br-ex, không raw NIC; no floating IP pattern
- `packer-openstack-image-pipeline` — Packer + OpenStack plugin; use_floating_ip=false; image lifecycle candidate→approved; Glance metadata tagging
- `openstack-trove-guest-agent-connectivity` — OVN logical IP vs Linux IP boundary; RabbitMQ dual listener; DNAT Keystone; SNAT internet; subnet gateway phải dùng br-ex IP
- `openstack-trove-postgresql-ha` — primary/replica via --replica-of; Swift mandatory; security group không auto-attach replica; backup_docker_image explicit; trove-guestagent.conf immutable; failover timeline 2 limitation
- `swift-single-node-setup-kolla` — loopback disk; GPT label KOLLA_SWIFT_DATA; XFS label d0; ring builder part_power=10 replicas=1; swap prerequisite; systemd persistence

Literature note created (1):

- `proxmox-dbaas-lab-day1-to-day4.5` — summary toàn bộ lab experience với 5+ incidents documented

Pages touched: 7 permanent notes (mới), 1 literature note (mới), index.md (cập nhật), log.md (append)

---

## [2026-04-10] ingest | PostgreSQL HA — Patroni + pgpool-II on Ubuntu

Source: https://medium.com/@joaovic32/demystifying-high-availability-postgresql-with-patroni-and-pgpool-ii-on-ubuntu-428c91a55b1a

Key concepts added:

- `patroni` — HA orchestrator dùng Raft consensus qua etcd; automatic failover; REST API health check
- `pgpool-ii` — connection proxy: pooling, read load balancing, write routing; sr_check awareness
- `postgresql-ha-patroni-pgpool-combo` — combined pattern; phân chia trách nhiệm không overlap; so sánh với Trove HA

Permanent note updated (1):

- `openstack-trove-postgresql-ha` — thêm link so sánh sang postgresql-ha-patroni-pgpool-combo

Pages touched: 3 permanent notes (mới), 1 permanent note (updated), 1 literature note (mới), index.md (cập nhật), log.md (append)

---

## [2026-04-11] ingest | Nhà Minh — tài chính sụp đổ (research tổng hợp)

Source: research tổng hợp từ nhiều nguồn học thuật (Ray Huang, Flynn & Giráldez, Von Glahn, Atwell)

Key concepts added:

- `ming-tax-base-erosion` — guiji (詭寄)/touxian (投獻); scale: đất chịu thuế giảm ~50% trong 140 năm; Single Whip Reform và giới hạn cơ cấu
- `ming-silver-inflation` — Manila Galleon; arbitrage bạc toàn cầu; bẫy Nhất điều tiên; cú sốc kép 1630s; tranh luận Atwell vs. Von Glahn

Fleeting note updated (1):

- `2026-04-11-minh-trieu-tai-chinh-sup-do` — thay [!warning] bằng [!info] promoted, link sang 2 permanent notes

Pages touched: 2 permanent notes (mới), 1 fleeting note (updated), index.md (cập nhật), log.md (append)

---

## [2026-04-25] capture | Gươm / Kiếm / Đao — Taxonomy vũ khí lạnh Đại Việt

Key concepts added: guom-kiem-dao-vu-khi-dai-viet
Pages touched: 1 permanent note (mới), index.md (cập nhật)

Highlights:

- Đao (刀) = single-edged broad curved → 1:1 với Chinese dao; không nhầm lẫn
- Chinese jian (劍) → bifurcation trong tiếng Việt: **kiếm** (thẳng 2 lưỡi) + **gươm** (cong 1 lưỡi)
- Gươm từ nguyên: Proto-Vietic *t-kɨəm ← Old Chinese 劍 *s.kr[a]m-s; tiền âm tiết \*t- → lenition /k/→/ɣ/; bằng chứng: tiếng Rục "təkɨəm"
- Kiếm = âm Hán-Việt (mượn thời Đường+); gươm = âm Việt cổ (mượn sớm hơn)
- Trực-Kiếm Đại Việt: mũi vếch Câu-Kiếm-Phong — đặc thù không thấy ở jian TQ hay tachi Nhật
- Biến thể vùng: Bắc (TQ), Trung (Nhật+ĐNA), Nam (Cham/Khmer/Xiêm), thế kỷ 19 (+Pháp)
- Hồ Gươm + Gươm Thần Thuận Thiên: gươm = từ dân gian gần gũi hơn kiếm trong văn hóa
- Nguồn học thuật chính: Vetyukov V. (2015), WHJ №2, pp.12–27

---

## [2026-04-25] capture | Y Bát (衣鉢) — thuật ngữ Phật giáo

Key concepts added: y-bat
Pages touched: 1 permanent note (mới), index.md (cập nhật)

Highlights:

- Y bát = áo cà sa (y) + bình bát (bát) — biểu tượng giới luật, truyền thừa, giản dị
- Kế thừa y bát: nghi thức trao truyền từ thầy sang trò — 3 lớp ý nghĩa: chánh pháp, lãnh đạo Tăng đoàn, tâm ấn thiền tông
- Thiền tông: câu chuyện Huệ Năng — Hoằng Nhẫn là ví dụ kinh điển nhất
- Ngoài đạo Phật: kế thừa tinh thần/phong cách/di sản của người thầy trong mọi lĩnh vực

---

## [2026-05-06] query | Bổ sung danh sách đầy đủ Kẻ + Xá vào permanent note

Bổ sung vào permanent note `danh-xung-lang-bac-bo`:

- Bảng Kẻ mở rộng: 29 địa danh (từ 8 → 29), có đầy đủ tên chữ + địa điểm + nghề truyền thống
- Bảng Xá: thống kê đầu TK XIX (Nguyễn Xá 36, Hoàng Xá 35, Lê Xá 21...), ước tính >200 địa danh toàn miền Bắc
- 8 làng Xá tiêu biểu gắn sự kiện/nhân vật lịch sử
- Ghi chú Tàm Xá — trường hợp Xá không theo họ người
- Kết luận phân bố: Xá gần như độc quyền Bắc Bộ + Bắc Trung Bộ

Pages touched: 1 permanent note (updated), log.md (append)

---

## [2026-05-06] ingest | Danh xưng làng Bắc Bộ — Kẻ, Xá và biến thiên lịch sử

Sources: Đức An (QDND — "Hà Nội: Vẫn còn đó những Kẻ nổi tiếng") · PGS Bùi Xuân Đính (Tạp chí Thế giới Di sản — "Tên làng và những nổi chìm lịch sử")

Key concepts added:

- `danh-xung-lang-bac-bo` — 3 lớp tên: Kẻ (tiền Bắc thuộc, từ Nôm cổ), Xá (Bắc thuộc, tên họ khai khẩn), Hán-Việt (TK X+, song song với tên Nôm)
- Nguyên tắc "càng vô nghĩa càng cổ" cho nhóm Kẻ
- Bảng đối chiếu Kẻ → tên chữ + nghề truyền thống
- Phân bố địa lý: Bắc Hà Nội dày, Nam thưa (biển lấn)
- Biến động sau 1945: đại xã, tên cách mạng, hợp tác xã → đứt gãy ký ức địa danh
- Note để ngỏ phần "Các lớp tiếp theo" để chủ nhân bổ sung

Pages touched: 1 permanent note (mới), 1 literature note (mới), index.md (cập nhật)

---

## [2026-05-06] ingest | OpenStack Disk QoS — Cinder Volume QoS và Nova Ephemeral Disk Throttling

Source: Research session — Cinder 2026.1 docs, Nova Rocky spec, OpenStackClient docs, nova/virt/libvirt/config.py

Status: Fleeting + Literature notes tạo trước; permanent notes chưa tạo (owner chưa có ý kiến rõ về phần quan trọng nhất)

Key topics captured:

- Token bucket mechanics trong Cinder QoS: `total_iops_sec` / `total_iops_sec_max` / `size_iops_sec`
- `consumer=front-end` vs `back-end` — enforcement point và trade-offs
- Nova flavor extra specs cho ephemeral disk (6 keys, không có burst)
- libvirt `<iotune>` XML mapping từ cả hai nguồn
- Capacity-based QoS (`*_per_gb`, `*_per_gb_min`)

Pages touched: 1 literature note (mới), 1 fleeting note (mới), index.md (cập nhật)

---

## [2026-04-11] ingest | Tứ giác nước — mô hình đô thị sông nước Việt Nam (research so sánh)

Source: Research session — Wikipedia tiếng Việt (Tứ giác nước), TS Lê Vĩnh An (Tạp chí Kiến trúc 01-2025), GS Trần Quốc Vượng, TU Delft (Chang'an water systems), Wikipedia EN (Bern, Metz, Wrocław, Koblenz)

Key concepts added:

- `tu-giac-nuoc` — định nghĩa, các kinh đô VN tiêu biểu, bốn chức năng; so sánh với 背山面水 Trung Quốc và mô hình 1 mặt sông châu Âu; các analog châu Âu (Bern, Metz, Wrocław, Koblenz)

Pages touched: 1 permanent note (mới), 1 literature note (mới), index.md (cập nhật), log.md (append)

---

## [2026-06-22] capture | Claude Certified Architect (CCA-F) — Section 1 FINISH

Cross-concept extraction from L1-L6. 3 new permanent notes (agentic-loop, tool-use-lifecycle, workflow-patterns), 1 updated (workflow-first-agentic-architecture), all backlinked from literature note, catalogs updated.

Pages touched: 3 permanent notes created, 1 permanent note updated, 1 literature note updated (Links), permanent-notes.md, literature-notes.md, log.md

## [2026-05-06] ingest | Làng Kẻ ở Phú Thọ và Vĩnh Phúc — mở rộng permanent note

Source: Research session — phanduykha.wordpress.com (Phan Duy Kha, _Nhìn về thời đại Hùng Vương_, 2013), DanViet (Kẻ Rưng/Tứ Trưng), donghodao.vn (Kẻ Nổi/Phượng Lâu), tinbds.com (Kẻ Mỏ/Yên Lạc), Wikipedia (Di chỉ Phùng Nguyên, Vĩnh Lại)

Key additions:

- 5 làng Kẻ tại Phú Thọ: Kẻ Khống (Chu Hóa), Kẻ Nội (Phùng Nguyên), Kẻ Nổi (Phượng Lâu), Kẻ Chịnh (Trịnh Xá), Kẻ Giáp (Tứ Xã — cần xác minh)
- 3 làng Kẻ tại Vĩnh Phúc: Kẻ Cánh (Hương Canh), Kẻ Rưng (Tứ Trưng), Kẻ Mỏ (Yên Lạc)
- Luận điểm Phan Duy Kha: 3 trung tâm phân bố Kẻ tương ứng vùng lõi Đông Sơn
- Tranh luận học thuật: Hoàng Thị Châu (1967) vs An Chi (1996) về nguồn gốc "kẻ"

Pages touched: 1 permanent note (danh-xung-lang-bac-bo.md — cập nhật), log.md (append)

## [2026-05-22] filter-ingest | OpenStack DBaaS Work Wiki — Batch 1 (enrich existing notes)

Source: Work wiki — OpenStack/DBaaS project (filtered — customer/org content excluded)

Key concepts enriched:
- postgresql-ha-patroni-pgpool-combo: split-brain protection (DCS quorum fencing), etcd sizing, phased migration path, operational commands
- kolla-ansible-deployment-patterns: RHOSO comparison table, decision framework, updated aliases
- openstack-neutron-overlay-protocols: hybrid topology pattern (Provider VLAN + GENEVE), Floating IP rejection rationale, GENEVE VNI limitation in OVN

Pages touched: 3 permanent notes updated, 1 literature note created, 3 catalog pages updated

## [2026-05-22] filter-ingest | OpenStack DBaaS Work Wiki — Batch 2 (new notes + enrich)

Source: Work wiki — OpenStack/DBaaS project (filtered — customer/org content excluded)

Key concepts added:
- rhoso-deployment-model: RHOSO 18 architecture, OCP-based control plane, EDPM, NIC patterns, PRD checklist
- openstack-validation-methodology: V-item structured PoC, T1/MUST classification, finding taxonomy, bare-metal revalidation

Key concepts enriched:
- patroni: 4 production gotchas (arping nohup, allowed-address-pairs, callback config, SIGHUP), etcd v2 API dependency
- ovs-bridge-management-nic-pitfall: RHOSO NIC pattern B, AWS nested-virt OVS constraints, jumbo frames dependency

Pages touched: 2 permanent notes created, 2 permanent notes enriched, catalog updated

---

## [2026-06-23] capture | Claude Certified Architect (CCA-F) - Section 2, Lecture 6

Extracted core concepts from lecture notes on Handling Ambiguity and Incomplete Specifications. Added details to the existing Section 2 literature note.
