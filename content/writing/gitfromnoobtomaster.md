---
title:  "Git from noob to master (Chapter 1)"
tags: [Git, type/write]
---

Git from noob to master - Chapter 1
====

Trong một vài lần chém gió về Git, thấy mọi người có vẻ chưa chú ý nhiều đến Git và sử dụng nó một cách hiệu quả.
Nhân đây xin mạn phép chém gió sơ qua để mọi người hoàn thiện.


Git là gì
------

Câu hỏi cơ bản và đơn giản nhưng thay vì trích dẫn bất kì định nghĩa nào mình sẽ quay sang từ khóa và hinh ảnh

* SCM - Source code management
* Linus Torvarlds (Linux)
* Thay thế BitKeeper trong dự án Linux Kernel

Cài đặt
------

Git cung cấp bộ cài cho tất cả các OS hệ POSIX: Linux, Windows, macOS trên [`git-scm`](https://git-scm.com/).

Sơ lược lịch sử
------

![alt text](../assets/img/git-fork-you.jpg){:height="445px" width="965px"}.

Torvalds thổ lộ rằng: ông chưa bao giờ muốn thực hiện quản lý source code, đó là công việc nhàm chán nhất trong thế giới máy tính (trừ database ra).
Torvalds cùng các đồng đội sử dụng rất nhiều SCM khác nhau cho đến khi họ thấy phù hợp với BitKeeper(BK), thậm chí BK là nhân tố quan trong trong việc phát triển nhanh chóng của Linux Kernel. Vấn đề duy nhất của BK là một phần mềm độc quyền, tất nhiên có bản miễn phí với các hạn chế cho các open-source project. Larry McVoy(CEO BitMover công ty phát triển BK) vừa muốn hỗ trợ các dự án phần mềm tự do vừa muốn bảo vệ lợi ích của BK cũng như BitMover tất nhiên chừng nào hội phần mềm tự do chưa đe dọa đến lợi ích.

Năm 2005, Andrew "Tridge" Tridgell đã dịch ngược BK, gần như ngay lập tức BK bỏ việc hỗ trợ Linux, ngược lại Linus cũng ngừng sử dụng BK để quản lý mã nguồn, tuy nhiên Linus chỉ có 3 tháng để chuyển toàn bộ source code sang công cụ mới khi BK chính thức ngừng hoàn toàn việc hỗ trợ. Điều đó nảy sỉnh việc phát triển Git.

Quá trình phát triển bắt đầu từ ngày 3 tháng 4 nằm 2005, Torvalds công bố dự án vào ngày 6, Git có thể cài đặt trên PC vào ngày 7 và tính năng merge branch được release vào ngày 18. Đến ngày 16/6 Git thực hiện quản lý việc release Linux Kernel v2.6.12


> Có lẽ việc ôn lại lịch sử sẽ kết thúc ở đây, phần tiếp theo sẽ quay lại thực tế sử dụng Git


Một số khái niệm trong Git
------

### Repository
* Repository là khái niệm cơ bản của Git tương đương với project trong GitLab, Gerrit
* Git có thể tạo bằng câu lệnh `git init`, thư mục trở thành *local repository* trong khi đó các repository online như trên GitLab được gọi là *remote repository*.
* Lịch sử làm việc đều được lưu trong thư mục khởi tạo git

### Remote
* Repository hosted in server. Remote repository chính thường được gọi là ***Origin***
* Các *local clone* đều liên kết đến **remote**.
* 1 local clone can have 0, 1 or many **remote**.

### Branch - nhánh
* Branch cũng là một khái niệm mới trong Git mà cần phải làm quen.
* Branch là các version song song với nhau của Repo.
* Người dùng làm việc trên các branch mà không làm ảnh hưởng đến ***live*** version.


#### Một vài branch đặc biệt
* master - branch mặc định
* HEAD - branch hiện tại mà người dùng đang làm việc
* origin/master, origin/xxx, origin2/xxx là các nhánh ở remote repository

### Checkout
* Checkout là cách người ta chuyển nhánh đề làm việc
* Ví dụ: Bạn lấy project trên GitLab về và cần chuyển qua nhánh master 2 để làm việc bạn cần checkout sang nhánh master 2: `git checkout master2`,
hoặc cũng có thể bạn cần tạo ra một nhánh mới hãy thêm flag `-b` trong câu lệnh: `git checkout -b dev_another_function`.

### Commit
* Nếu như Branch là nhánh cây, cành cây thì có thể coi commit chính là các nút (node) hoặc cũng có thể là một điểm nào đó trên nhánh.
* Người ta lưu lịch sử làm việc trong các commit, nói cách khác nếu bạn commit dữ liệu của bạn không thể mất trừ khi bạn xóa hoàn toàn project git.
* Cách tạo commit cũng đơn giản: `git commit -m "Commit Message"` hoặc cũng có thể đơn giản là [`git commit`](https://git-scm.com/docs/git-commit) sau đó làm theo hướng dẫn với giao diện text editor(thường là vi, vim, nano, etc, ..)
* Mỗi commit đều có message và một ID đặc biệt (SHA –hash), từ đó có thể trace được commit


> Nói đến đây mình đột nhiên nhiên phát hiện ra trình bay theo hướng này sẽ khiến mọi người khó hiểu một chút, vì vậy ngay sau đây mình sẽ trình bày về commit một cách rõ hơn, theo một cách nhin khác


### Git Concept

![alt text](https://git-scm.com/book/en/v2/images/lifecycle.png)

![alt text](../assets/img/git_local_operation.png)

2 hình trên đây đã trình bày cách sử dụng git ở local một cách cơ bản

Nhìn chung một file sẽ có 4 trạng thái như hình thứ nhất
* Untracked: Trong working directory nhưng git không quản lý, đó là nhưng file tạo mới, được copy vào hay đơn giản là bạn vừa chạy [`git rm fileName`](https://git-scm.com/docs/git-rm).
* Unmodified: Rất dễ hiểu, đó là file git quản lý, git ghi nhận rằng file đó chưa có sự thay đổi *so với* ***commit hiện tại*** (HEAD). Có 2 cách để chuyển file vào trạng thái này đó là thêm các file untracked [`git add`](https://git-scm.com/docs/git-add) hay [`git commit`](https://git-scm.com/docs/git-commit) tức là lưu sự thay đổi đó lại (file sẽ được ghi nhận là ko thay đổi so với commit mới :D, đôi chút confuse).
* Modified: Là file từ trạng thái unmodified và đã bị edit, tất nhiên khi Crtl + Z toàn bộ step, file sẽ trở lại unmodified.
* Staged: là trạng thái chuẩn bị được commit, thông thường người dùng không để ý đến trạng thái này, trạng thái này được trình bày khá rõ ràng ở hình thứ 2.
Một trạng thái kiểu như git đã ghi nhận sự thay đổi nhưng chưa tạo commit. Để chuyển sang trạng thái staged sử dụng câu lệnh [`git stage`](https://git-scm.com/docs/git-stage).


> Một tips nhỏ với stage, khi bạn cần commit lên nhánh A mà lại làm việc trên nhánh B thì bạn có thể stage toàn bộ file cần thay đổi và checkout sang nhánh A và commit. Một cách khác đa số mọi người sẽ áp dụng đó là commit trên nhánh B, cherry-pick(sẽ nói ở dưới đây) commit sang A, ngoài ra cũng có cách chuyên nghiệp hơn đó là tạo ***pack diff***.

### Merge và Rebase
Từ thực tế làm việc trong nhóm sẽ nảy sinh các trường hợp phải kết hợp code của nhiều developer để tạo nên một bản hoàn chỉnh thay vì tiếp tục phát triển song song mãi điều đó nảy sinh ra 2 khái niệm trong git là merge và rebase.