---
layout: post
title: "Xác thực Kerberos trong Active Directory,"
author: "remi"
categories: job
tags: [Việc]
image: assets/img/2023/04/05/krb_intro.jpg
---

Kerberos là một giao thức xác thực quan trọng được sử dụng chính trong xác thực Active Directory. Người làm việc với Active Directory nói riêng và xài hàng của hãng Microsoft nói chung chắc chắn cần hiểu về nó. Buổi hôm nay sẽ mô tả sơ qua về Kerberos ~ không cố gắng đi vào chi tiết tinh mom tổ chấy, chỉ muốn tóm lược đủ để có thể hiểu để khi làm việc với Kerberos: Khi cấu hình biết mình đang làm cái gì và thỉnh thoảng giúp ích cho quá trình troubleshoot khi gặp lỗi.

## Kerberos là gì?

> Kerberos is an authentication protocol that is used to verify the identity of a user or host

Kerberos là một giao thức xác thực mạng được thiết kế để định danh (identify) user. Dĩ nhiên bắt đầu quá trình định danh này người dùng vẫn cần cung cấp một thông tin xác thực (username và password) hợp lệ. 

Kerberos được sử dụng trong Active Directory để cung cấp thông tin về đặc quyền (Privileges) của mỗi user, tuy nhiên nó không thực hiện việc authorization. Việc xác định quyền truy cập vào tài nguyên được thực hiện bởi mỗi Service và Kerberos không xác thực (validate) tài nguyên hoặc dịch vụ nào mà người dùng có thể truy cập. Có thể hình dung việc này như sau (Ví dụ na ná để dễ mường tượng thôi): Kết thúc quá trình làm việc với AD sẽ xác định được user thuộc group nào còn quyền group đó được làm gì trên service thì do service quyết định.

## Kerberos được sử dụng khi nào?

Lúc học ĐH học về Kerberos chả hiểu để làm gì mãi đến khi đi làm dùng mới hiểu tại sao phải dùng nó. Về cơ bản Kerberos được sử dụng rộng rãi vì mang lại nhiều lợi ích, như vài cái cụ thể dưới đây:

- **Secure:** Kerberos không truyền passwords trên mạng (network)
- **Single-Sign-On:** Kerberos duy nhất yêu cầu user cung cấp password một lần duy nhất khi authen
- **Trusted third-party:** Kerberos được sử dụng như centralized authentication server được biết như là Key Distribution Center (KDC) nơi mà tất cả các devices trong network trust by default. Điều này đảm bảo rằng các thông tin nhạy cảm không lưu trữ trên local machine => Đây chắc là lý do quan trọng nhất cần sinh ra kerberos
- **Mutual authentication:** Trong  Kerberos, cả 2 bên giao tiếp communication với nhau phải được authenticated trước khi quá trình communication được thực hiện.

## Thành phần chính của Kerberos arch

Dưới đây là một số thành phần core của Kerberos schema authentication protocol

- **Kerberos Realm**:  Là một logical network, hiểu nó tương tự như một vùng Domain, trong đó Kerberos authentication server có quyền xác thực (authenticate) một user, host hoặc service
- **Key Distribution Centre (KDC)**: thành phần này bao gồm Authentication Server (AS) và Ticket Granting Service (TGS). Chức năng chính của KDC là trung gian giữa  AS và TGS. Cấp phát một ticket-granting ticket (TGT) sau đó gửi cho TGS để được mã hóa. The KDC trong một domain nằm trên **Domain Controller**
- **Authentication Server (AS)**: Một client xác thực với AS sử dụng username và password để login. Sau khi xác thực AS sẽ chuyển tiếp username tới KDC để trả lại một TGT.
- **Ticket Granting Service (TGS)**:  Khi một client muốn truy cập một service, họ phải cung cấp TGT của họ cho cho TGS.
- **Service Principal Name (SPN):** Một định danh **(**identifier) được cấp cho một service instance để associate một service instance với một service account trong domain.

Các thành phần được mô tả bằng hình vẽ sau

![krb component]({{site.url}}/assets/img/2023/04/05/krb_components.png)


## Kerberos Authentication trong môi trường AD diễn ra như thế nào?

Quá trình Kerberos authentication trải qua một vài bước tuy nhiên việc này thực hiện rất nhanh (gần như realtime). Trong Active Directory các bước này được tóm gọn như sau:

1. Khi một user đăng nhập vào Active Directory, user sẽ xác thực  (**authenticates) với Authentication Server (AS) đặt trên** Domain Controller (DC) sử dụng user’s password - các DC lưu trữ thông tin password (hash) của password này tùy cấu hình của quản trị viên.
2. Sau quá trình xác thực, DC gửi trả lại user một **Ticket Granting Ticket (TGT)** Kerberos ticket cho realm Kerberos có quyền xác thực. TGT được cached trên users computer cho sử dụng sau này và cung cấp (presented) TGT này cho DC bất kỳ để chứng minh việc đã authentication cho Kerberos service tickets.
3. Khi user muốn truy cập  (access) Skype service, Máy trạm user tìm kiếm **Service Principal Name (SPN)** của Skype Service.
4. Khi SPN được xác định, máy tính liên lạc lại với DC và trình bày TGT của user với SPN để yêu cầu Ticket Granting Service truy cập tài nguyên mà người dùng cần giao tiếp.
5. DC trả lời với Ticket Granting Service (TGS) Kerberos service ticket.
6. Máy trạm của người dùng gửi TGS cho máy chủ Skype để truy cập dịch vụ.
7. Skype connects successfully.

Các bước được mô tả bên dưới đây

![krb steps]({{site.url}}/assets/img/2023/04/05/krb_steps.png)

## Các loại Kerberos tickets

Quá trình xác thực Kerberos khá phức tạp tuy nhiên quá trình này quẩn quanh 2 loại ticket chính (cần nhớ):

- **TGS** (Ticket Granting Service): là  ticket  mà user sử dụng để authenticate với một service. Nó được encrypted với service key.
- **TGT** (Ticket Granting Ticket): là ticket được cấp bởi KDC sau khi client thực hiện thành công việc authenticated. Nó được cung cấp cho KDC để request TGSs và được encrypted với KDC key.

Như ban đầu giới thiệu về Kerberos chi tiết chắc có khi phải cần 1 bài dài lê thê cơ mà đã thống nhất viết thật ngắn gọn bởi vậy chỉ xin dừng tại đây. Rất hy vọng mọi người đọc và hiểu những gì mình định viết.


