# Wiki Log

Append-only record of operations. Format: `## [YYYY-MM-DD] operation | description`

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
