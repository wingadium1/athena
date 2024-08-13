---
title: "Kingman's Formular"
alias:
  - The Kingman Equation
  - Kingman’s Approximation
tags: [ type/term, Agile, Scrum ]
---

> [!info]
> được sử dụng để ước tính thời gian chờ trung bình trong hàng đợi (cụ thể là trong hàng đợi phục vụ đơn lẻ với biến số là thời gian đến và thời gian phục vụ).

Nó áp dụng cho hàng đợi [[https://en.wikipedia.org/wiki/M/G/1_queue|M/G/1]], viết tắt của:
* M: Markovian (or memoryless) arrival process (Poisson arrival process).
* G: General service time distribution.
* 1: One server.

Công thức Kingman

$$$
W_q \approx \frac{\rho}{1 - \rho} \cdot \frac{C_a^2 + C_s^2}{2} \cdot \frac{1}{\mu}
$$$

Trong đó

* $W_q$: Average time spent waiting in the queue.
* $ρ$: Utilization rate of the system (arrival rate λ divided by service rate μ).
* $C_a$: Coefficient of variation of the inter-arrival times.
* $C_s$: Coefficient of variation of the service times.
* $μ$: Service rate (the rate at which the server completes service).

Keypoint:
* Nó dành riêng cho các hàng đợi đơn có biến số về thời gian đến và thời gian phục vụ.
* Nó cung cấp giá trị xấp xỉ thay vì giá trị chính xác.
* Nó làm nổi bật tác động của biến số đến thời gian chờ:số lượng request đến lớn hơn và/hoặc thời gian xử lý lớn hơn làm tăng thời gian chờ.

Nhìn chung [[refs/little_s_law]] cung cấp cái nhìn chung cho bất kì hệ thống ổn định nào, trong khi đó công thức Kingman thì là công thức, tính gần đúng, chi tiết hơn cho thời gian chờ với biến số là số lượng request đến và thời gian xử lý