---
title: "DevOps - các Pipeline và Toolchain"
tags: [  DevOps ]
---

[[refs/DevOps]]

![alt text](../assets/img/devops_toolchain.png)

Nếu cắt đều những vòng lặp vô tận của toolchain thì chúng ta sẽ thấy cái nhìn cơ bản của pipeline.

| Plan | Create | Build | Verify | Package | Configure | Release | Verify | Monitor |
| ---- | ------ | ----- | ------ | ------- | --------- | ------- | ------ | ------- |

## Pipeline foundation

Gồm 2 bước: Plan, Create

### Planning with Agile project management tools

Để tracking tiến độ, xem chúng ta đang ở đâu từ request -> release.

### Viết code và tạo tài liệu

Version Control đóng vai trò quan trọng, ngày nay version control lưu trữ cả test script và tất cả những thứ liên quan đến thay đổi và release.

Document thì thường lưu trữ ở Wiki: requirements specifications, architectural  models, UI wireframes, and product and user documentation.

## [[refs/continuous_integration|Continuous integration]]

Gồm 3 bước: Build, Verify, Package

### Orchestrating the change.

### Verifing quality

Một trong những tính năng quan trọng nhất.
Shift-left philosophy, bring the test earlier and more often.

Phong trào DevSecOps ủng hộ việc kiểm thử tự động sớm hơn và thường xuyên hơn như một cách thiết lập bảo mật liên tục (Continuous Security). Có thể thực hiện kiểm thử sớm mà không cần phải thực thi mã trong các môi trường thông qua unit test.

#### Unit Test (TDD)

Nên sử dụng cùng với một test management software (XRay hoặc Zephyr)

#### Static Analysis

* Lint
* SAST
* Dependency scanning
* License scanning

### Packaging for deployment

## [[refs/continuous_deployment|Continuous Deployment]]

Gồm 4 bước: Configure, Release, Verify, Monitor

[[refs/continuous_deployment|Continuous Deployment]] tiếp tục từ đưa các image được tạo ra từ bước package vào các môi trường. Tự động hóa (Automation) có thể đóng vai trò trong việc thêm hoặc cập nhật tài nguyên trong các môi trường này.

Các công cụ IaC cho phép cấu hình các tài nguyên. Với source code chạy trên các môi trường, chúng ta có thể tiến hành test chi tiết hơn để tìm ra các vấn đề về chất lượng và bảo mật. Tại đây, các bài test cũng có thể xem xét cách các thay đổi ảnh hưởng đến hiệu suất và xác thực các thay đổi mong muốn. Khi các thay đổi được thêm vào môi trường, chúng ta cần nhận thức được tác động của những thay đổi này. Để đạt được mục đích đó, chúng ta sẽ đo lường hiệu suất của toàn bộ môi trường bao gồm storage và phân tích log.

### Cấu hình môi trường với IaC

### Release với việc quản lý cấu hình và feature flag

Các công cụ quản lý cấu hình chịu trách nhiệm xác định và thiết lập cấu hình cho môi trường dev -> production. Một pipeline có thể gọi công cụ quản lý cấu hình để đánh dấu rắng package đã được thông qua [[refs/continuous_integration|Continuous integration]] stage.

#### Feature flag

Ngay cả khi các source code change được chạy ở trên production, những thay đổi đó có thể không hiển thị (visible) với người dùng cuối hoặc ảnh hưởng đến chức năng hiện có. Điều này có thể là do các feature flag ngăn không cho khả năng hiển thị các thay đổi mã.

Điều này cho phép triển gradual rollout of changes (hoặc progressive rollout) ví dụ như canary release. Điều này cũng cho phép nhanh chóng rollback hệ thống về trạng thái trước đó bằng cách hủy các feature flag.

Các feature flag tool: LaunchDarkly, Flagsmith và CloudBees Feature Management.

### Additional verification through advanced testing in the  environment.

#### Functional and UI testing
* Sanity test
* Smoke test
* Regression test

Automation test có thể nên được triển khai

#### Load/performance testing

Load/performance testing không được thiết kế để đo chức năng chính xác. Thay vào đó, mục tiêu của kiểm thử hiệu suất là xác minh các Non-functional Requirement: reliability hoặc scalability.

Chúng ta muốn xem hệ thống, khi có bất kì thay đổi nào, có thể đáp ứng được nhu cầu sử dụng hay không.

#### Dynamic application security testing (DAST)

DAST thực hiện các bài kiểm thử tự động, trong đó giả lập các vụ tấn công vào môi trường cho web application để tìm kiếm các lỗ hổng.

```
# Sample dast
stages:
  - test

test-zap:
  image: owasp/zap2docker-stable:2.12.0
  stage: test
  allow_failure: true
  script:
    - echo "create a directory..."
    - mkdir /zap/wrk
    - echo "Execute the baseline scan..."
    - /zap/zap-baseline.py -t http://34.170.134.227:8080/ -g gen.conf -r testreport.html
  after_script:
    - echo "Copy report file to the artifact path..."
    - cp /zap/wrk/testreport.html .
  artifacts:
    when: always
    expire_in: 30 days
    paths:
      - testreport.html
```

#### IaC Scanning

Trivy

#### Container Scanning

Trivy

#### BDD (Acceptance Test)

### Monitoring

#### Performance monitoring/reporting

#### Log collection

#### Alerting

Khi sự cố phát sinh, điều quan trọng là phải thông báo kịp thời cho những người liên quan. Các công cụ cảnh báo có thể cung cấp nhiều kênh để thông báo, bao gồm email, tin nhắn SMS và tin nhắn trò chuyện IM. Chúng cũng có thể cung cấp cơ chế dung sai để ngăn chặn quá nhiều tin nhắn cảnh báo gửi đến nhân viên vận hành và tình trạng quá tải cảnh báo xảy ra. Các công cụ này cũng có thể tạo ra các vấn đề cho việc quản lý sự cố để các quy trình quản lý dịch vụ CNTT (ITSM) được tuân thủ.

