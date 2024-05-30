---
title:  "F*** the people, who do not add Newline at End of File"
tags: [Git, F***, Coding, type/blog]
---

F*** the people, who do not add Newline at End of File
====

Nghe có vẻ cứt bò, tuy nhiên sự thật là việc không add một trong trống ở cuối file thực sự là một tội ác


# No Newline at End of File

Bạn đã bao giờ nhìn thấy dòng chữ ở git

![alt text](../assets/img/git_diff_no_line_eof.png)

hay ở vim

![alt text](../assets/img/vim_no_line_eol.png)

Ờ thế tại sao git lại báo thế, nó đến từ một quyết định trong quá khứ của Unix

> Một tệp nguồn không trống sẽ kết thúc bằng một ký tự dòng mới, không được đặt ngay trước ký tự dấu gạch chéo ngược (backslash).
>
> Vì đây là mệnh đề, nên chúng ta sẽ kiểm tra việc vi phạm quy tắc

Vì vậy, nó chỉ ra rằng, theo POSIX, mọi tệp văn bản (bao gồm các tệp nguồn Ruby và JavaScript) phải kết thúc bằng một ký tự ***\n***, hoặc mới newline (không phải là một dòng mới). Điều này đóng vai trò là eol, hoặc kết thúc của dòng nhân vật. Nó là một dòng ***Terminator***.

Bây giờ thì gần như mọi chương trình text editor đều hỗ trợ việc tự động thêm 1 dòng trống vào cuối file, ví dụ:
* vim: mặc định rồi, nên là cứ để nó mặc định thôi
* VSCode: dùng setting **Files: Insert Final Newline**
* Sublime: dùng setting **ensure_newline_at_eof_on_save**

Tóm lại là anh em nên thêm 1 dòng vào cuối file code nếu chưa có thì hãy sửa như sau:

```bash
git ls-files -z | while IFS= read -rd '' f; do tail -c1 < "$f" | read -r _ || echo >> "$f"; done
```

ref: https://unix.stackexchange.com/questions/31947/how-to-add-a-newline-to-the-end-of-a-file
