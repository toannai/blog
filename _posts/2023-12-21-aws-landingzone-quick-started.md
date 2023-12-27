---
layout: post
title: "Xây dựng AWS LandingZone dành cho công ty có hạ tầng từ Nhỏ tới To với nhiều trăn trở,"
author: "remi"
categories: job
tags: ['AWS', 'Cloud']
image: assets/img/2023/12/21/00_intro_landingzone.png
---

Trước khi bắt đầu giới thiệu chút về bản thân. Mình xuất thân là một System Engineer sau đó chuyển sang làm System Security. Trước đó thuần làm ở Onpremse, cũng mới bắt đầu tìm hiểu về Cloud gần đây thôi (Tầm 1-2 năm). Move to Cloud hiện tại mọi người gọi là Trend, nhưng mình tin đó là Xu thế của Công nghệ hạ tầng. Mà đã là Xu thế thì không ai có thể cản và muốn hay không thì cũng phải theo thôi. Mượn lời của người anh thân thiết "Anh cũng phải tìm hiểu về Cloud thôi, mình không biết thì mình thiệt". Cho tới bây giờ sau một thời gian tiếp xúc "gần" suy nghĩ về Cloud của cá nhân cũng thấy thay đổi nhiều so với trước đây.

Nhìn lại cũng thấy mình là người may mắn khi đi vào con đường **Cờ Lao** (Public Cloud). Nói may mắn vì nơi mình làm cũng đang có chiến lược chuyển một phần hệ thống lên Cloud. Mình được các Sếp tin tưởng cho join vào các dự án Migrate Infra To Cloud của Cơ quan - P/S một đơn vị đủ TO: TO để không phải cứ nói lên Cloud một phát là bốc lên luôn mà phải ngó trước ngó sau, tính toán thật kỹ thiệt hơn (Vượt qua đủ các rào cản từ kỹ thuật tới chi phí và đặc biệt là pháp lý) && TO đủ để AWS dedicate người Focus tập trung hỗ trợ/tư vấn một cách chuyên nghiệp, đủ "nhiệt tình" (Billing đủ to nếu sử dụng). Thời gian đầu thì mình hay lặn lội lên Udemy tự học, đọc blog, join các khóa học Free của AWS dành cho đối tác tiềm năng. Trong quá trình cũng có được join các buổi "combat" - nghe các anh bàn luận này nọ nên cũng hiểu từ sơ sơ đến sâu sâu. Nhưng nếu chỉ tìm hiểu lý thuyết mà không sắn tay vào "DoIt" thì chắc chắn cũng sẽ quên thôi. Vấn đề bây giờ làm thế nào để **trải nghiệm** trong khi công việc chính hiện tại chủ yếu chỉ là Role tư vấn hoặc đưa ra yêu cầu???. Nói tới đây thì vẫn cứ thấy mình may mắn "quá luôn" khi tới khoảng giữa năm mình được mấy người ae thân thiết rủ tham gia cùng một số dự án Ops hệ thống trên Cloud. Sâu hơn mình tham gia dưới vai trò là Ops team, join build lên hạ tầng AWS cho một Startup công nghệ từ Zero. Team của mình đều là các ae có k/n chinh chiến nhiều năm trong mảng Cloud ở mọi mặt trận (P/S: Chắc mình là ít kinh nghiệm nhất). Tới giờ thì mình không còn làm việc cùng a/e do công việc chính quá bận không thể sắp xếp thời gian, sự tập trung & đặc biệt là sức khỏe để làm thêm việc khác nên đành xin nghỉ. Nhưng có cơ hội vẫn muốn cảm ơn a/e rất nhiều vì những gì học được trong vài tháng ngắn ngủi đó thật là quý giá vô cùng. Phần giới thiệu tới đây có vẻ hơi dài quá, nhưng cứ viết là "tâm tư" nó trỗi dậy :) Có dịp sẽ viết riêng sau, việc bây giờ phải quay lại chủ đề chính "Xây dựng AWS LandingZone dành cho công ty có hạ tầng từ Nhỏ tới To với nhiều trăn trở," nôm na là chia sẻ những câu hỏi và trải nghiệm về hành trình lựa chọn áp dụng xây dựng AWS LandingZone.

Khi làm việc với AWS đặc biệt khi thiết kế cho Cloud Infras trên AWS ta hay gặp nhắc tới cái gọi là Landingzone. Theo aws thì họ định nghĩa 
>A landing zone is a well-architected, multi-account AWS environment that is scalable and secure. This is a starting point from which your organization can quickly launch and deploy workloads and applications with confidence in your security and infrastructure environment. Building a landing zone involves technical and business decisions to be made across account structure, networking, security, and access management in accordance with your organization’s growth and business goals for the future.

Từ định nghĩa trên thì Landingzone phải là **well-architected** + **multi-account** nhưng với mình thì mình vẫn thích hiểu nó theo nghĩa chỉ là **well-architected** - Một kiến trúc tố. Tố theo hướng nó phù hợp với người sử dụng và không nhất thiết là multi-account. 

Lúc mới bắt đầu mình cân nhắc rất nhiều với câu hỏi "Nên chọn sigle hay multi-account?" vì ai khi mới bắt đầu làm quen với AWS bít multi account sẽ gặp bài toán kết nối network giữa các Account/VPC khá khoai. Khoai ở chỗ là AWS cứ động đến cái gì là chạc tiền: Ra khỏi VPC là tiền traffic, Routing service (transit gateway) cũng lại là chạc tiền ~ tương đối là tốn kém. Để trả lời cho câu hỏi này cũng tìm kiếm google và thấy mấy ông đưa ra một số tiêu chí cụ thể như sau:

![sigle or multi]( {{site.url}}/assets/img/2023/12/22/05_sigle_multi.png){:width="900px"}

K/n rút ra: Nghe thì rất hợp lý cơ mà sau thời gian dùng thì thấy trừ khi hạ tầng quá nhỏ chỉ gồm vài con EC2 vài Service còn không cứ Multi Account cho khỏe nha.




