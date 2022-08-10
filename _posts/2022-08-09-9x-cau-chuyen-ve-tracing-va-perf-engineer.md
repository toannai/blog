---
layout: post
title: "Câu chuyện về performance tracing và Performance Engineering,"
author: "remi"
categories: job
tags: [Việc]
image: assets/img/2022/08/10/tracing_intro.jpg
---

>Từ ngày Cococ tổ chức syscon năm nào cũng đăng ký và cố gắng sắp xếp đi (đâu đó chỉ 1 năm đăng ký mà ko đi được vì bận việc gấp). Trước đây thì có nhưng công việc hiện tại thấy cũng không liên quan nhiều. Nhiều cái thỉnh thoảng đi như thói quen. Đến hoặc là nghe mấy ae chém gió cho vui hoặc là cơ hội gặp lại mấy người quen/đồng nghiệp cũ ngày xưa (Mấy người mà cũng hay đi như mình).

Nói vu vơ quá, quay lại chủ đề chính. Vẫn nhớ hồi đi làm không ít lần ngồi hàng giờ trên màn hình top/htop/free/iostats chỉ để cố tìm lý do tại sao hệ thống lại bị cao tải máy chủ bị treo? - Nhiều khi không gõ được cả lệnh luôn đành phải reboot. Cái khốn là nhiều khi cái nhìn thấy lại là cái ảo tưởng, chỉ là hệ quả của nguyên nhân khác (Lấy vd: CPU cao nhiều khi gốc dễ là do I/O latency hoặc Network low perf).

Hồi đấy hệ thống product cũng hoạt động chồng chéo. Ứng dụng này gọi một đống service kia (Cũng kiểu multi services). Nhiều lần dịch vụ treo không biết tại sao? Chả còn cách nào ngoài việc tắt dần các backend services để debug nguyên nhân tại đâu =)). Rồi sau nghĩ ra trò gắn time execute label để debug (Một dạng kiểu tương tự tracing bây giờ - Cơ mà hồi đấy tracing làm gì đã phát triển).

Cá nhân làm thì cứ hay tự hỏi, ước ước mơ mơ. Và ước mơ hồi ấy là để trả lời câu hỏi cái gì đang diễn ra bên trong follow xử lý yêu cầu người dùng khi gửi tới ứng dụng? Cụ thể là từng function đang tiêu tốn tài nguyên thế nào? tốn bao thời gian để thực hiện các function đó? Sau này công việc không còn liên quan nhiều nữa, không làm ở mức low level nữa nên câu trả lời nhìn chung vẫn chưa trả lời cặn kẽ. 

 Với kinh nghiệm cá nhân và các kỹ thuật tracing hiện tại nếu quay lại "những năm tháng ấy" mình tin là chắc chắn sẽ khác. Thế hệ ngày nay chs cứ thấy nhiều bạn làm sys/ops chỉ thích tập trung vào các hot trend như CI/CD, AWS, GCP không thích các thứ kiểu low level như vậy làm mình cảm thấy mình kiểu già VCH. Vậy nên 2 mùa syscon gần đây thấy Mr.TungDam khơi gợi lại câu hỏi năm xưa =)) thấy thân thuộc quá. Chắc đó là lý do hay đi syscon. 



