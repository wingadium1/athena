# Wiki Index

_Last updated: 2026-04-09 (query: OpenStack Neutron external-to-tenant network connectivity)_

Catalog của toàn bộ Athena Wiki. AI agent cập nhật file này sau mỗi lần ingest.

---

## Permanent Notes

| Note                                                                                                       | Domain            | Summary                                                                        |
| ---------------------------------------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------ |
| [[permanent/llm-wiki-pattern\|LLM Wiki Pattern]]                                                           | PKM/LLM           | Pattern xây dựng knowledge base với LLM làm maintainer                         |
| [[permanent/rag-vs-compiled-knowledge\|RAG vs Compiled Knowledge]]                                         | PKM/LLM           | So sánh RAG (re-derive mỗi lần) vs compiled knowledge (tích lũy)               |
| [[permanent/zettelkasten\|Zettelkasten]]                                                                   | PKM               | Phương pháp atomic notes + linking của Niklas Luhmann                          |
| [[permanent/cip-counterpart-fund-viet-nam\|CIP và Counterpart Fund]]                                       | Lịch sử/Kinh tế   | Cơ chế ẩn lương QLVNCH trong viện trợ kinh tế Mỹ — 1955–1975                   |
| [[permanent/map-vien-tro-vu-khi-qlvnch\|MAP và EDA — Định Giá Thấp Vũ Khí Viện Trợ]]                       | Lịch sử/Kinh tế   | Structural underreporting vũ khí viện trợ qua cơ chế EDA book value            |
| [[permanent/lau-nam-goc-qua-giam-sat-quoc-hoi\|Lầu Năm Góc và Giám Sát Quốc Hội]]                          | Lịch sử/Chính trị | Bức tranh tổng thể về khoảng trống oversight của Quốc Hội Mỹ trong Vietnam War |
| [[permanent/openstack-overlay-networks\|OpenStack Overlay Networks (VXLAN/GRE/GENEVE)]]                     | Infrastructure    | Overlay tunnels giải quyết VLAN limit; VNI allocation và OVS flow pipeline     |
| [[permanent/openstack-external-network-mapping\|OpenStack External Network Mapping]]                        | Infrastructure    | Provider networks kết nối vào tenant overlays qua OVS bridge + router namespace |
| [[permanent/openstack-dvr-architecture\|DVR — Distributed Virtual Router]]                                  | Infrastructure    | Phân tán L3 routing ra compute nodes; east-west + floating IP north-south      |
| [[permanent/openstack-floating-ip-nat\|Floating IP NAT Mechanism]]                                          | Infrastructure    | DNAT/SNAT iptables rules; centralized vs. DVR models                           |
| [[permanent/openstack-bgp-evpn-external\|BGP/EVPN External Connectivity in OpenStack]]                      | Infrastructure    | Dynamic routing at scale: neutron-dynamic-routing, BGPVPN, ovn-bgp-agent      |

---

## Literature Notes

| Note                                                                          | Source                    | Date Read  |
| ----------------------------------------------------------------------------- | ------------------------- | ---------- |
| [[literature/karpathy-llm-wiki\|LLM Wiki — Karpathy]]                         | Andrej Karpathy (gist)    | 2026-04-08 |
| [[literature/tetsu-kasuya-46-method\|Phương pháp 4:6 — Tetsu Kasuya]]         | Project Barista           | 2026-04-08 |
| [[literature/dang-phong-21-nam-vien-tro-my\|21 Năm Viện Trợ Mỹ ở VN]]         | Đặng Phong (sách)         | 2026-04-08 |
| [[literature/openstack-neutron-external-connectivity\|OpenStack Neutron Docs]] | OpenStack official docs   | 2026-04-09 |

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
