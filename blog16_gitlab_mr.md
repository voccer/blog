Đối với dân lập trình, git không phải là thứ gì xa lạ. Chúng ta làm việc hằng ngày trên git, nhưng để sử dụng git 1 cách hiệu quả thì không phải 1 newbie có thể dễ dàng làm được, đặc biệt là thao tác Merge Request.

## Merge Request là gì?

Trong git flow chúng ta được giới thiệu về các branch, trong đó có 1 nhánh đặc biệt là main/master. Để tránh việc lập trình viên thay đổi code ở nhánh chính trước khi được review code hay qua các test thì chúng ta cần tạo các nhánh khác nhau. MR hiểu đơn giản là tạo 1 **request** để **merge** 1 nhánh vào 1 nhánh khác.

## Cách tạo 1 MR
sau đây mình xin giới thiệu 1 vài cách tạo MR:
- Tạo MR bằng command line:
  Đây là cách phổ biến và mình cũng hay dùng nhất, đơn giản chỉ cần sử dụng,
  Tạo branch mới
  ```shell
  git checkout -b feature/new_feature
  ```
  Commit những thay đổi của bạn
  ```shell
  git add file1
  ```
  ```shell
  git commit -m "first message"
  ```
  ```shell
  git push <remote> <branch-name>
  ```
  sau khi push xong trên terminal sẽ hiện đường dẫn để tạo MR, ấn ctrl + click để truy cập lên UI

- Nếu sử dụng UI để push như sử dụng vscode thì sẽ ko hiện đường dẫn, khi đó chúng ta mặc định phải vào UI trên gitlab để tạo MR.

## Cách đặt tên branch:

Không có 1 ràng buộc nào trong việc đăt tên branch trong git, nhưng trong 1 team hay trong 1 tổ chức, doanh nghiệp cần thống nhất một quy chuẩn trong đặt tên để tránh bị lẫn lộn.

Tạo branch cho feature: feature/branch_name
Tạo branch cho sửa lỗi: hotfix/branch_name

Nếu lỡ tay đặt sai tên branch hoặc khi có nhu cầu đổi tên branch, chúng ta có lệnh sau:
```shell
git branch -m new_name (nếu đang ở branch muốn đổi)
```
hoặc:
```shell
git branch -m old_name new_name (nếu đang ở branch khác)
```

## Cách options khi sử dụng MR

from `feature/tutorial_1` into `main` 
Biểu thị tạo request từ nhánh `feature/tutorial_1` tới nhánh `main`.
Để thay đổi chúng ta ấn vào `change branches`.
Title: Title của MR
Đặt `Draft:` phía trước title để thể hiện rằng MR này chưa hoàn thiện cần bổ sung thêm.

Description: Miêu tả thông tin của MR, chúng ta có 1 vài **patern design** để link tới issues hay để cc tới một người nào đó.

Asignee: thường sẽ assign tới người tạo MR hoặc cũng có thể assign tới người sẽ review, ở bản enterprise sẽ có thể asignee cho nhiều người. 

Reviewer: assigne tới PM hoặc team leader hoặc một người nào đó trong team chịu trách nhiệm review
Milestone: Chọn milestone cho MR
Labels: chọn labels cần gán cho MR

Delete source branch when merge request is accpeted: Đây là tùy chọn mặc định sẽ xóa branch sau khi được merge, điều này khiến cho remote sạch sẽ hơn (sẽ không xóa branch trên local)

Chúng ta có thể compare sự thay đổi trước khi merge và sau khi merge để check code 1 lần nữa.

## Link tới issues

Như đã đề cập ở phía trên, trong description có  **patern design** cho phép dev có thể link tới issues.
Trên thực tế, ngoài description thì dev có thể sử dụng **patern design** này trong lúc commit.
Để closes issues nào đó, sử dụng: closes #n (trong đó n là số thứ tự tới issues)
để relate tới issues nào đó: related to #n


lưu ý vói closes thì issues chỉ close khi MR đươc merge và chức năng cho phép automatic close khi được merge trong cài đặt issue được bật.

## Tips
Trong quá trình tạo MR, rất dễ gặp lỗi conflict, trường hợp này gặp khi có 1 update khác ở nhánh chính xảy ra sau khi MR được tạo. Để tránh điều này xảy ra chúng ta cần đảm bảo tạo nhánh merge đươc checkout từ nhánh chính ở bản release cuối cùng.
Nếu xảy ra conflict, chúng ta cần resolve sớm nhất có thể.