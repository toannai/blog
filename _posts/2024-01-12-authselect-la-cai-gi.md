---
layout: post
title: "Ghi chú về authselect trên Linux,"
author: "remi"
categories: job
tags: ['Linux']
image: assets/img/2024/01/12/intro.png
---

Đợt này đang có một số việc trên linux có gặp một toy khá mới gọi là authselect. Search thì thấy vietnam chưa ông nào viết về cái này thôi thì biên tập lại cái note trên notion của mình về authselect post lên đây để google nó index cho trang và cũng hy vọng là nhỡ có ngày có ai cần nó thì sao :) OK mà.

Nói về authselect thì phải nói tới PAM (Pluggable Authentication Modules) trên linux. Mà nói về PAM thì khá dài dòng nhưng chỉ xin mô tả ngắn gọn: "Hình dung nó nó như project để centralize các việc xác thực trên linux" như hình mô tả sau.

![01 pam]( {{site.url}}/assets/img/2024/01/12/01_pam.png){:width="600px"}

>Khi app/toy chạy trên linux, thay bằng việc mỗi ông phải code/xây dựng một cơ chế xác thực cho app/toy của mình có thể sử dụng luôn PAM để xác thực luôn - PAM thành chỗ xác thực tập trung. Việc xác thực tập trung tại PAM nó có nhiều lợi ích như: Muốn thêm/sửa/xóa hoặc disable một ông user thì chỉ cần xử tại một chỗ mà thôi khỏi cần làm tại nhiều chỗ. Muốn áp dụng chính sách như mật khẩu mạnh, lockout mật khẩu nếu nhập sai, ... vân vân và mây mây cũng chỉ cần làm tại PAM mà không phải làm tại nhiều chỗ khác.

Ai làm việc PAM chắc hẳn đều biết thư mục ```/etc/pam.d/``` thần thánh để cấu hình điều chỉnh hoạt động của PAM. Vẫn nhớ mấy lần đầu mở mấy file ở thư mục này ra đọc chả hiểu cái mọe gì :'(. Thời làm Viettel có lần cấu hình sai ở đây còn không vào được server luôn, phải phi sang tận sang DC ở Giang Văn Minh để recorver. Dính phốt nên mỗi lần cấu hình ở đây khá là rén.

Để cái việc này nó dễ dàng tiếp cận hơn chút thì Redhat nó có cái toy gọi là "authselect" nhằm hỗ trợ việc cấu hình này cho nó đơn giản và an toàn hơn. Theo định nghĩa của chatgpt xin phép trích xuất:

>Authselect là một công cụ trong hệ thống Linux được sử dụng để quản lý cấu hình của các dịch vụ xác thực như PAM (Pluggable Authentication Modules) trên hệ thống. Authselect giúp đơn giản hóa quá trình cấu hình xác thực bằng cách cung cấp các hồ sơ cài đặt được đặc trưng cho môi trường xác thực cụ thể.
>
>Mục tiêu chính của Authselect là đơn giản hóa quá trình cài đặt và quản lý các cấu hình xác thực trên hệ thống Linux, đồng thời đảm bảo tính linh hoạt và tùy chỉnh. Authselect thường được sử dụng trong các bản phân phối Linux như Fedora và CentOS để quản lý xác thực hệ thống.

Đoạn giới thiệu khá là dài phải không? Ae bắt tay vào gõ lệnh thôi,

## Profile điểm khởi đầu

Ý tưởng chính của PAM đó là các thiết lập được tổ chức thành các Profile. Mỗi profile có thể bao gồm các cấu hình nhất định. Trên hệ thống linux ta có thể switch qua switch lại giữa các profile đã định nghĩa để chọn cấu hình cho máy chủ :) Nghe mềm rẻo đấy!

Nói tạm vậy giờ động tay vào gõ luôn cho dễ nhớ. Trước khi băt đầu cần nhớ: **Để làm việc với authselect sử dụng lệnh `authselect`**. Lệnh này hỗ trợ một loạt Option như sau:

![02 option]( {{site.url}}/assets/img/2024/01/12/02_option.png){:width="600px"}

Liệt kê toàn bộ các Profile trên hệ thống:

```authselect list```

Ủa vậy xem mình đang xài profile nào thì sao? Thì sử dụng ```authselect current```

Chuyển qua chuyển lại giữa các profile sử dụng ```authselect select <profile name>```