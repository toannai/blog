---
layout: post
title: "Chuyện cá nhân, chuyện performance tracing và syscon các thứ,"
author: "remi"
categories: job
tags: [Việc]
image: assets/img/2022/08/10/tracing_intro.jpg
---

>Từ ngày Cococ tổ chức syscon năm nào cũng đăng ký và cố gắng sắp xếp đi (đâu đó chỉ 1 năm đăng ký mà ko đi được vì bận việc gấp). Công việc trước đây thì có nhưng hiện tại thấy cũng không liên quan nhiều tới nội dung của syscon. Nhiều cái thỉnh thoảng nó như thói quen. Cứ thấy là đăng ký đi thôi.

Nói vu vơ quá, quay lại chủ đề chính. Vẫn nhớ hồi mới đi làm đi làm không ít lần ngồi hàng giờ trên màn hình top/htop/free/iostats chỉ để cố tìm lý do tại sao hệ thống lại bị cao tải và máy chủ bị treo? - Nhiều khi không gõ được cả lệnh luôn đành phải reboot. Cái khốn ở chỗ nhiều khi cái nhìn thấy lại là cái ảo tưởng, chỉ là hệ quả của nguyên nhân ở chỗ khác (Lấy vd: CPU cao nhiều khi gốc dễ là do I/O latency hoặc Network low perf).

Hồi đấy hệ thống product cũng hoạt động chồng chéo. Ứng dụng này gọi một đống service kia (Cũng kiểu multi services). Nhiều lần dịch vụ treo không biết tại sao? Chả còn cách nào ngoài việc tắt dần các backend services để debug nguyên nhân tại đâu =)). Rồi sau nghĩ ra trò gắn time execute label trong code ở mode debug để khi cần bật lên troubleshoot (Một dạng kiểu tương tự tracing bây giờ - Cơ mà hồi đấy tracing làm gì đã phát triển).

Cá nhân làm thì cứ hay tự hỏi, ước ước mơ mơ. Và ước mơ nhỏ nhoi trong công việc hồi ấy là làm sao để có thể trả lời câu hỏi cái gì đang diễn ra bên trong luồng xử lý yêu cầu của người dùng gửi tới ứng dụng? Cụ thể là từng components như api, sâu hơn là method/functions đang tiêu tốn tài nguyên thế nào? mất bao thời gian để xử lý từng component đó? Sau này công việc không còn liên quan nhiều nữa, không làm ở mức low level nữa nên vẫn chưa trả lời cặn kẽ câu hỏi năm nào. 

Tuy nhiên với những gì đã kinh qua của cá nhân và sự phát triển của các kỹ thuật/công cụ tracing hiện tại nếu quay lại "những năm tháng ấy" nếu tập trung mình tin là chắc chắn sẽ khác. 

Thế hệ các bạn làm sys/ops sau này có vẻ thích tập trung vào các hot trend như CI/CD, AWS, GCP không thích hoặc không để ý nhiều các thứ như vậy. Có lẽ cũng do mình là lớp cũ, kiểu khác biệt thế hệ - tự thấy mình già VCH. Vậy nên 2 mùa syscon gần đây mỗi khi thấy Mr.TungDam khơi gợi lại câu hỏi năm xưa =)) cứ nhớ mình của những ngày nào - thấy thân thuộc quá. Oh, chắc đó là lý do hay đi syscon. 

Chả biết thế nào nhưng nếu làm Ops chắc chắn mình sẽ dành nhiều thời gian hơn cho domain Tracing và Perf tunning. Cá nhân thấy rằng một hệ thống chạy thôi chưa đủ mà phải chạy ngon và mượt hơn từng ngày. 

P/S: Trước đang đọc dở cuốn Systems Performance Enterprise And The Cloud thấy cũng hay liên quan tới domain này, bạn nào quan tâm có thể tham khảo (Đọc rồi viết blog mình hóng với) :) 



