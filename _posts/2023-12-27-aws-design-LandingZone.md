---
layout: post
title: "Cloud&Me-Q&A, Phần 2 - Dựng hạ tầng cho Org trên AWS và những tâm tư,"
author: "remi"
categories: job
tags: ['AWS', 'Cloud']
image: assets/img/2023/12/27/00_intro_landingzone.png
---

Ý tưởng thì khá rõ ràng & nội dung cũng có thể nói là có nhưng khi bắt tay vào viết lại thì khó phết - khó để tổ chức nó lại một cách logic & trình bày lại những gì mình nghĩ cho người khác hiểu. Nhưng không sao, tôi sẽ cố gắng, và cũng xin "rào" trước với người đọc đây là các ý kiến cá nhân ~ không bàn luận chuyện đúng sai (PM: Càng lớn càng nhận ra đúng / sai nó cứ là mong manh :d). Như tiêu đề bài viết, nay tôi sẽ tập trung ghi lại những Q&A của tôi trong thời gian đã va vào AWS. 

## Chuyện chọn Single/Flat hay Multi-Account/Hierarchy Structure?

Xây dựng mới hoặc chuyển hạ tầng lên Cloud thì vấn đề đầu tiên cần giải quyết là phải cân nhắc là lựa chọn chạy trên Single Account hay Multi-Account. Với một tổ chức lớn với dịch vụ phức tạp và không phải lo lắng chuyện tiền nong thì khỏi phải suy nghĩ nhiều cứ Multi-Account mà giã thôi. Nhưng với một công ty nhỏ (dạng một startup) khi mà nghiệp vụ không quá phức tạp nhiều khi workload không quá nhiều và đặc biệt là ít xèng thì câu chuyện dùng Multi - Account nó cũng là cái phải cân nhắc đấy. Việc AWS cứ hở ra là chạc tiền khiến cho việc chạy Multi-Account phát sinh nhiều loại chi phí đi kèm "Không đâu" như Transit Gateway Attachment (Mỗi VPC hơn tầm 50$ ~ hơn triệu), phí VPC <-> VPC traffic, công sức maintain đống route giữa các Account. Chưa kể với người có kinh nghiệm làm "Onpremse" chắc chắn sẽ muốn cái gì cũng Centralize (Monitor/log/security) làm cho lượng traffic này càng lớn và thật sự tốn kém. 

Cá nhân làm cái gì cũng không muốn chỉ dựa vào "Cảm tính" cố gắng tìm một cái gì đó làm cơ sở. Có tìm thấy aws viết [Tại đây](https://docs.aws.amazon.com/accounts/latest/reference/welcome-multiple-accounts.html). Bài viết khá dài nhưng tóm gọn lại việc lựa chọn Single hay Multi - Account dựa trên bảng sau:

![sigle or multi]( {{site.url}}/assets/img/2023/12/27/05_sigle_multi.png){:width="900px"}

Dựa trên bảng trên ban đầu ý của mình là lựa chọn Sigle Account tuy nhiên một số ae lại nhất quyết lựa chọn Multi - Account. Thế là trong nội bộ cũng có combat với nhau :'(. Cá nhân thì mình là người khá lành cũng không muốn combat với AE nên sau đó đã theo a/e chọn Multi - Account. Cơ mà sau khi xài mới thấy thực sự Multi - Account mang lại các lợi ích đáng kể.


Kinh nghiệm rút ra:

- Tìm hiểu rõ cấu trúc tổ chức làm đầu vào cho bài toán lựa chọn Sigle hay Multi Account và Multi thế nào
- Chọn multi account cho tôi nếu như tổ chức chỉ gồm một vài con VM hay cụm EKS
- Cuộc sống là sự ánh đổi lợi cái này thì được cái kia (Củng cố) không có cái gì hoàn hảo cả nha

## Chuyện lựa chọn số lượng Account và cách tổ chức cây Account (Hierarchy Structure)






