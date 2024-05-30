---
title:  "Git from noob to master (Chapter 2)"
tags: [Git, type/write]
---

> * [Chapter 1](gitfromnoobtomaster.html)

Trong một vài lần chém gió về Git, thấy mọi người có vẻ chưa chú ý nhiều đến Git và sử dụng nó một cách hiệu quả.
Nhân đây xin mạn phép chém gió sơ qua để mọi người hoàn thiện.

Git command
------

### Merge và Rebase (cont.)
Tiếp theo lần trước, mình sẽ bắt đầu với ***merge***

Tại sao cần *"merge"*?.

Đơn giản là vì chúng ta lập trình cùng nhau, nếu bạn quen dùng SVN, sẽ thấy là khi 2 người cùng làm việc (tất nhiên là song song với nhau), sẽ nảy sinh nhu cầu tạo một phiên bản là sự kết hợp giữa 2 phiên bản source code của 2 người

Để merge được chúng ta cần 2 branch, dưới dây là một ví dụ

![alt text](../assets/img/git_branch_1.png)

Chúng ta có 2 issue 55 và 56 trên hệ thống tracking, một cách nào đó developer có thể giải quyết 2 issues bằng một commit nhưng không phải lúc nào cũng thế và đặc biệt là không phải lúc nào cũng tốt, như là khi 1 trong 2 issues đó chưa được giải quyết triệt để tức là commit đó có vấn đề, rất khó để trace.

Mọi người nên làm cách clear hơn, 1 hoặc 2 developer sẽ fix nó, ở các nhánh khác nhau.

```bash
git checkout -b issue55
git checkout -b issue56

# -b for new branch
```

![alt text](../assets/img/git_branch_2.png)

Họ modify source code ở mỗi nhánh và commit

![alt text](../assets/img/git_branch_3.png)

Giờ chúng ta cần đưa phần thay đổi của 2 branch issue55 và issue56 vào master.

```bash
git checkout master # change currrent branch to master
git merge issue55
...
git merge issue56
```

![alt text](../assets/img/git_branch_4.png)

![alt text](../assets/img/git_branch_5.png)

Như vậy mỗi khi merge, chúng ta kết hợp 2 branch (hoặc commit, phần 3 mình sẽ giải thích kỹ hơn tại sao lại là commit) vào một nhánh, hoặc nghĩ đơn giản hơn là đưa những thay đổi ở nhánh này vào nhánh kia bằng một commit mới (chứa toàn bộ thay đổi).

#### Ops!!! Git merge conflict

![alt text](../assets/img/git_conflict_1.png)

Tại sao conflict, đơn giản thôi, khi ở nhánh bạn merge vào, trong ví dụ issues55 là master bạn cũng có thay đổi. Ở ví dụ này là chúng ta có như sau

![alt text](../assets/img/git_conflict_3.png)

Mình có thay đổi ở master và issues1 cùng một dòng, git sẽ không biết chọn sự thay đổi nào. Nhiêu bạn sẽ sợ conflict và không biết làm thế nào và hỏi, câu trả lời là không thế nào cả, người merge sẽ là người quyết định. Có thể là chọn một trong 2, chọn cả 2 hoặc có thể phải làm lại cả đoạn đó.

![alt text](../assets/img/git_conflict_2.png)

***Nghe nè***: đừng bao giờ giải quyết conflict khi bạn không biết người khác vừa thay đổi cái gì, thật đấy.

#### Rebase

Cũng là một cách khác để đưa những thay đổi từ nhánh này sang nhánh khác nhưng khác hơn một chút: Bạn sẽ không tạo thêm commit mới mà đưa toàn bộ commit từ nhánh này sang nhánh kia.

Quay lại với ví dụ conflict ở bên trên. Khi merge **master** vào nhánh **issues1** chúng ta có

![alt text](../assets/img/git_rebase_1.png).

***Rebase*** thì sao:

```
git checkout issues1
git rebase master
.... # conflict and resolve
git add HelloWorldApp.java # add conflict file to tracking again
git rebase --continue
```

![alt text](../assets/img/git_rebase_2.png).

Như vậy có thể thấy là thay vì tạo commit mới, ***rebase*** sẽ đưa toàn bộ commit ở branch **issues1** từ lúc branch out đặt lên cuối branch **master**, như vậy coi như là chúng ta không branch out từ một điểm trong quá khứ mà branch out từ lastest commit của **master**. À quên, vẫn có conflict và giải quyết nó như bình thường nhé :D.

#### Advance Rebase (super strong)

Thế giờ nếu có một nhánh khác được branch out từ nhánh issues1 mình có rebase lên master được không.

Câu trả lời là có:

Giả sử chúng ta có nhánh **issues2** được branch out từ **master** và **issues3** được branch out từ **issues2**.

![alt text](../assets/img/git_rebase_3.png).

```
git rebase --onto master issues2 issues3
```
Về cơ bản nó có ý nghĩa như sau:

Hãy checkout nhánh **issues3**, và tìm các commit từ commit chung của nhánh **issues3** và **issues2**, sau đó apply chúng vào nhánh master.

![alt text](../assets/img/git_rebase_4.png).

***Chú ý***: không được ***rebase*** các commit mà bạn đã push lên repository. Nếu không ai cũng ghét và mọi người sẽ sỉ nhục và coi thường.

> Cơ bản là thế này, khi ***rebase*** bạn bỏ đi các commit và chuyển chúng sang chỗ khác. Khi bạn sửa các commit bằng ***rebase***, từ một nhánh bạn pull từ repository, và push lên. Mọi người sẽ phải apply lại commit của họ và sẽ xảy ra nhất nhiều conflict khi bạn pull change các commit đó của họ về local.
