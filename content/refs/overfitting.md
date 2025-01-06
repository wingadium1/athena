---
title: "Overfitting - Underfitting"
tags: [ Machine Learning ]
---

> [!info] Overfitting
> Một loại hiện tượng trong [[refs/machine_learning|Machine Learning]], khi đó các mô hình đưa ra thì chính xác cho tập dữ liệu đào tạo (train set) nhưng lại sai với dữ liệu khác (mới, test).

> [!info] Underfitting
> Một loại hiện tượng trong [[refs/machine_learning|Machine Learning]], khi đó các mô hình không thể xác định mối quan hệ có ý nghĩa giữa dữ liệu đầu vào và đầu ra

### Tại sao lại tồn lại Overfitting
Hiện tượng quá khớp xảy ra do một số nguyên nhân, chẳng hạn như:
 * Kích thước dữ liệu đào tạo quá nhỏ và không chứa đủ mẫu dữ liệu để thể hiện chính xác tất cả các giá trị dữ liệu đầu vào khả thi.
 * Dữ liệu đào tạo chứa một lượng lớn thông tin không liên quan, được gọi là dữ liệu nhiễu.
 * Mô hình đào tạo quá lâu trên một tập dữ liệu mẫu duy nhất.
 * Do có độ phức tạp cao, mô hình học cả phần nhiễu trong dữ liệu đào tạo.

### Làm thế nào để phát hiện

Với một tập dữ liệu train, thay vì sử dụng toàn bộ, chúng ta sẽ dành một phần dữ liệu để kiểm thử.
#### K-fold cross-validation

Bằng cách chia dữ liệu train thành K tập con, dùng 1 tập để train và kiểm thử trên K-1 tập còn lại, liên tục quan sát dữ liệu điều chỉnh mô hình và đánh giá hiệu năng của mô hình