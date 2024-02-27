---
layout: post
title: "Tàu nhanh về auditing trong Oracle DB,"
author: "remi"
categories: job
tags: ['Unknown']
image: assets/img/2024/01/22/00_auditing.png
---

Gần đây có một task liên quan tới việc kiểm tra Auditing trong cơ sở dữ liệu Oracle. Chưa làm việc với Oracle DB bao giờ (tờ giấy trắng về Oracle DB) nên cũng mất kha khá thời gian để tìm hiểu và tổ chức lại một cách hệ thống để sau này gặp lại đỡ mất công mò mẫm. Tiện thể cũng biên tập lại lên đây.


Auditing trong oracle cố gắng thực hiện ghi lại các hoạt động của người dùng (User Activity) lên cơ sở dữ liệu và các đối tượng trên trên đó. Các hoạt động này bao gồm cả đọc, sửa, xóa (SELECT, CREATE, INSERT, UPDATED/ALTER, DELETE). Hoạt động/sự kiện được ghi lại có thể toàn bộ hoặc dựa trên một điều kiện nhất định <=> Ghi lại sự kiện nếu .... Mục đích có thể là để đảm bảo tuân thủ, phát hiện bất thường (Security) hoặc sử dụng để phân rõ trách nhiệm sau khi vấn đề xảy ra.

![Y nghia]( {{site.url}}/assets/img/2024/01/12/01_ynghia.png){:width="600px"}

Tính năng Auditing trong Oracle đã có từ các phiên bản khá cổ. Theo Oracle quá trình phát triển có thể được tổng hợp bằng cái hình sau:

![Developement]( {{site.url}}/assets/img/2024/01/12/02_developement.png){:width="600px"}

