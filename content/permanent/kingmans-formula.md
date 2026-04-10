---
title: "Kingman's Formula"
aliases: ["Kingman's formula", "Kingman equation", "Kingman's approximation", "VUT equation"]
tags: [flow, queuing-theory, metrics, agile, math]
created: 2026-04-10
updated: 2026-04-10
---

Kingman's Formula (còn gọi là VUT equation) là công thức ước tính thời gian chờ trung bình trong hàng đợi M/G/1 — hàng đợi có một server, arrival process ngẫu nhiên (Markovian), và service time tổng quát (General). Đây là công cụ toán học giải thích tại sao một hệ thống gần đầy tải lại có thời gian chờ tăng phi tuyến tính.

$$W_q \approx \frac{\rho}{1 - \rho} \cdot \frac{C_a^2 + C_s^2}{2} \cdot \frac{1}{\mu}$$

## Các biến số

| Biến   | Ý nghĩa                                                        |
| ------ | -------------------------------------------------------------- |
| $W_q$  | Thời gian chờ trung bình trong hàng đợi                        |
| $\rho$ | Utilization rate = arrival rate $\lambda$ / service rate $\mu$ |
| $C_a$  | Coefficient of variation của inter-arrival times               |
| $C_s$  | Coefficient of variation của service times                     |
| $\mu$  | Service rate                                                   |

## Insight cốt lõi

Ba yếu tố quyết định thời gian chờ — đây là lý do gọi là VUT:

- **V**ariability ($C_a$, $C_s$): biến động càng cao, chờ càng lâu
- **U**tilization ($\rho$): khi $\rho \to 1$ (gần 100% utilization), $\frac{\rho}{1-\rho} \to \infty$ — wait time tăng không giới hạn
- **T**ime (service time $1/\mu$)

**Implication thực tế**: đừng chạy hệ thống ở 100% utilization. Ở 80% utilization, một nhóm vẫn đang chịu thêm 4x thời gian chờ so với 50%. Đây là lý do SAFe khuyến nghị giữ IP (Innovation & Planning) iteration và không load team 100%.

## So sánh với Little's Law

Little's Law cung cấp mối quan hệ tổng quát giữa WIP, throughput, và cycle time cho _bất kỳ_ hệ thống ổn định nào. Kingman's Formula chi tiết hơn: nó xấp xỉ _thời gian chờ cụ thể_ với biến động của arrival và service time.

## Connections

- [[permanent/little-s-law]] — Little's Law là nền tảng tổng quát hơn; Kingman đi sâu vào queue wait time
- [[permanent/flow-framework]] — Flow Load và Flow Efficiency chịu tác động trực tiếp của các yếu tố Kingman mô tả
- [[permanent/cost-of-delay]] — utilization cao → wait time cao → Cost of Delay tăng

## Sources

- [[refs/kingman_s_formular]]
