---
layout: post
title: "Ghi chú về authselect trên Linux,"
author: "remi"
categories: job
tags: ['Linux']
image: assets/img/img/2024/01/12/intro.png
---

Đợt này đang có một số việc trên linux có gặp một toy khá mới gọi là authselect. Search thì thấy vietnam chưa ông nào viết về cái này thôi thì biên tập lại cái note trên notion của mình về authselect post lên đây để google nó index cho trang và cũng hy vọng là nhỡ có ngày có ai cần nó thì sao :) OK mà.

Nói về authselect thì phải nói tới PAM (Pluggable Authentication Modules) trên linux. Mà nói về PAM thì khá dài dòng nhưng chỉ xin mô tả ngắn gọn: "Hình dung nó nó như project để centralize các việc xác thực trên linux" như hình mô tả sau.

![01 pam]( {{site.url}}/assets/img/2024/01/12/01_pam.png){:width="600px"}

>Khi app/toy chạy trên linux, thay bằng việc mỗi ông phải code/xây dựng một cơ chế xác thực cho app/toy của mình có thể sử dụng luôn PAM để xác thực luôn - PAM thành chỗ xác thực tập trung. Việc xác thực tập trung tại PAM nó có nhiều lợi ích như: Muốn thêm/sửa/xóa hoặc disable một ông user thì chỉ cần xử tại một chỗ mà thôi khỏi cần làm tại nhiều chỗ. Muốn áp dụng chính sách như mật khẩu mạnh, lockout mật khẩu nếu nhập sai, ... vân vân và mây mây cũng chỉ cần làm tại PAM mà không phải làm tại nhiều chỗ khác.

Hồi mới đi làm nói chung đọc về PAM cũng không hiểu lắm nhưng làm lâu thì thấy nó cứ như là mindset hiển nhiên, dễ hình dung hay gặp trên cái thế giới IT này. Nói hình dung về PAM thì dễ nhưng vào cấu hình PAM thì ngại phết: Đọc cũng hơi loằng ngoằng cho junior và khá run tay vì nhỡ cấu, sai nhẹ thì không sao nhưng sai mạnh có khi không vào được server (PM: Vì nó quản lý phần xác thực mà, xác thực fail thì vào sao được). 

Để cái việc này nó dễ dàng tiếp cận hơn chút thì Redhat nó có cái toy gọi là "authselect" nhằm hỗ trợ việc cấu hình này. Theo định nghĩa của chatgpt xin trích xuất:

>Authselect là một công cụ trong hệ thống Linux được sử dụng để quản lý cấu hình của các dịch vụ xác thực như PAM (Pluggable Authentication Modules) trên hệ thống. Authselect giúp đơn giản hóa quá trình cấu hình xác thực bằng cách cung cấp các hồ sơ cài đặt được đặc trưng cho môi trường xác thực cụ thể.
>
>Mục tiêu chính của Authselect là đơn giản hóa quá trình cài đặt và quản lý các cấu hình xác thực trên hệ thống Linux, đồng thời đảm bảo tính linh hoạt và tùy chỉnh. Authselect thường được sử dụng trong các bản phân phối Linux như Fedora và CentOS để quản lý xác thực hệ thống.

Ý tưởng chính của PAM đó là các thiết lập được tổ chức thành các Profile. MỖi profile có thể bao gồm các cấu hình nhất định. Nói tạm vậy giờ động tay vào gõ luôn cho dễ nhớ. Cơ mà trước khi băt đầu giới thiệu luôn: Để làm việc với authselect sử dụng lệnh `authselect` sau đó là loạt các option:

![02 option]( {{site.url}}/assets/img/2024/01/12/02_option.png){:width="600px"}

Khá dài phải không nhưng không sao. NÓi biết vậy từ từ khám phá sau:

##Đầu tiên

Liệt kê toàn bộ các Profile trên hệ thống:

```authselect list```

