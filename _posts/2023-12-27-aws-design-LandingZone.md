---
layout: post
title: "Cloud&Me-Q&A, Phần 2 - Dựng hạ tầng cho Org trên AWS và những tâm tư,"
author: "remi"
categories: job
tags: ['AWS', 'Cloud']
image: assets/img/2023/12/27/00_intro_landingzone.png
---

Ý tưởng thì khá rõ ràng & nội dung cũng có nhưng khi bắt tay vào tổ chức và viết lại thì khó phết - khó để trình bày lại những gì mình nghĩ. Nhưng không sao, tôi sẽ cố gắng, và cũng "rào" trước với người đọc đây là các ý kiến cá nhân ~ không bàn luận chuyện đúng sai (PM: Càng lớn càng nhận ra đúng / sai nó cứ là mong manh :d). Như tiêu đề bài viết, nay tôi sẽ tập trung ghi lại những Q&A của tôi trong thời gian đã va vào AWS. 

## Chuyện chọn Single hay Multi-Account?

Khi làm việc với AWS đặc biệt khi thiết kế cho Cloud Infras trên AWS ta hay gặp nhắc tới cái gọi là Landingzone. Theo aws thì họ định nghĩa 
>A landing zone is a well-architected, multi-account AWS environment that is scalable and secure. This is a starting point from which your organization can quickly launch and deploy workloads and applications with confidence in your security and infrastructure environment. Building a landing zone involves technical and business decisions to be made across account structure, networking, security, and access management in accordance with your organization’s growth and business goals for the future.

Từ định nghĩa trên thì Landingzone phải là **well-architected** + **multi-account** nhưng với mình thì mình vẫn thích hiểu nó theo nghĩa chỉ là **well-architected** - Một kiến trúc tố. Tố theo hướng nó phù hợp với người sử dụng và không nhất thiết là multi-account. 

Lúc mới bắt đầu mình cân nhắc rất nhiều với câu hỏi "Nên chọn sigle hay multi-account?" vì ai khi mới bắt đầu làm quen với AWS bít multi account sẽ gặp bài toán kết nối network giữa các Account/VPC khá khoai. Khoai ở chỗ là AWS cứ động đến cái gì là chạc tiền: Ra khỏi VPC là tiền traffic, Routing service (transit gateway) cũng lại là chạc tiền ~ tương đối là tốn kém. Để trả lời cho câu hỏi này cũng tìm kiếm google và thấy mấy ông đưa ra một số tiêu chí cụ thể như sau:

![sigle or multi]( {{site.url}}/assets/img/2023/12/27/05_sigle_multi.png){:width="900px"}

K/n rút ra: Nghe thì rất hợp lý cơ mà sau thời gian dùng thì thấy trừ khi hạ tầng quá nhỏ chỉ gồm vài con EC2 vài Service còn không cứ Multi Account cho khỏe nha.





