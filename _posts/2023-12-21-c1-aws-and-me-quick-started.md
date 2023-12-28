---
layout: post
title: "Cloud&Me-Q&A, Part1 - Hành trình tới với Public Cloud của tôi,"
author: "remi"
categories: job
tags: ['Cloud']
image: assets/img/2023/12/21/cloud_intro.jpeg
---

Định viết loạt bài về "Tâm tư" cá nhân với Public Cloud (Đặc biệt là AWS), đặt cái tên kêu là "Cloud&Me-Q&A". Cũng không biết viết được tới đâu, hay cỡ nào, có ai đọc nữa không? cơ mà nay cứ đặt tay viết thôi vì "Không có điểm bắt đầu thì làm gì biết đi được tới đâu".

Trước khi bắt đầu giới thiệu chút về bản thân. Tôi xuất thân là một System Engineer sau đó chuyển sang làm System Security. Trước đó thuần làm ở Onpremse, cũng mới bắt đầu tìm hiểu về Cloud gần đây thôi (Tầm 1-2 năm). Move to Cloud hiện tại mọi người gọi là Trend, nhưng tôi tin đó là Xu thế của Công nghệ hạ tầng. Mà đã là Xu thế thì không ai có thể cản và muốn hay không thì cũng phải theo thôi. Mượn lời của người anh thân thiết "Anh cũng phải tìm hiểu về Cloud thôi, mình không biết thì mình thiệt" (PM: Tôi không muốn thiệt đâu). Hồi đầu thành thực mà nói không đánh giá cao hàm lượng tech trong việc sử dụng Public Cloud (dạng như AWS) nhiều nhưng cho tới bây giờ, sau một thời gian tiếp xúc "gần" suy nghĩ về Cloud của cá nhân cũng thấy thay đổi nhiều so với trước đây.

Nhìn lại cũng thấy tôi là người may mắn khi đi vào con đường **Cờ Lao** (Public Cloud). Nói may mắn vì nơi tôi làm cũng đang có chiến lược chuyển một phần hệ thống lên Cloud. Tôi được các Sếp tin tưởng cho join vào các dự án Migrate Infra To Cloud của Cơ quan - P/S một đơn vị đủ TO: TO để không phải cứ nói lên Cloud một phát là bốc lên luôn mà phải ngó trước ngó sau, tính toán thật kỹ thiệt hơn (Vượt qua đủ các rào cản từ kỹ thuật tới chi phí và đặc biệt là pháp lý) && TO đủ để AWS dedicate người Focus tập trung hỗ trợ/tư vấn một cách chuyên nghiệp, đủ "nhiệt tình" (Billing đủ to nếu sử dụng). Thời gian đầu thì tôi hay lặn lội lên Udemy tự học, đọc blog, join các khóa học Free của AWS dành cho đối tác tiềm năng. Trong quá trình cũng có được join các buổi "combat" - nghe các anh bàn luận này nọ nên cũng hiểu từ sơ sơ đến sâu sâu. Nhưng nếu chỉ tìm hiểu lý thuyết mà không sắn tay vào "DoIt" thì chắc chắn cũng sẽ quên thôi. Vấn đề bây giờ làm thế nào để **trải nghiệm** trong khi công việc chính hiện tại chủ yếu chỉ là Role tư vấn hoặc đưa ra yêu cầu???. Nói tới đây thì vẫn cứ thấy tôi may mắn "quá luôn" khi tới khoảng giữa năm tôi được mấy người ae thân thiết rủ tham gia cùng một số dự án Ops hệ thống trên Cloud. Sâu hơn tôi tham gia dưới vai trò là Ops team, join build lên hạ tầng AWS cho một Startup công nghệ từ Zero. Team của tôi đều là các ae có k/n chinh chiến nhiều năm trong mảng Cloud ở mọi mặt trận (P/S: Chắc tôi là ít kinh nghiệm nhất). Tới giờ thì tôi không còn làm việc cùng a/e do công việc chính quá bận không thể sắp xếp thời gian, sự tập trung & đặc biệt là sức khỏe để làm thêm việc khác nên đành xin nghỉ. Nhưng có cơ hội vẫn muốn cảm ơn a/e rất nhiều vì những gì học được trong vài tháng ngắn ngủi đó thật là quý giá vô cùng. Phần giới thiệu tới đây có vẻ hơi dài quá, nhưng cứ viết là "tâm tư" nó trỗi dậy :) Có dịp sẽ viết riêng sau. Loạt bài này chỉ tập trung nội dung kỹ thuật là chính (không sướt mướt). "AWS&Me-Q&A," - Nôm na là góp nhặt câu hỏi - câu trả lời dựa trên trải nghiệm về hành trình làm việc với Public Cloud (Hiện tại chủ yếu là AWS) của bản thân.

Theo thời gian có thể "trưởng thành" hơn <=> tư duy/nhìn nhận có thể thay đổi cơ mà tạm thời va đến đâu viết ra đến đó. Hy vọng những gì viết ở đây có ai đó đó đọc thì thật là vui,





