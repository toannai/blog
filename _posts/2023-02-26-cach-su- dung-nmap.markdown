---
layout: post
title: "Học cách sử dụng nmap,"
author: "remi"
categories: job
tags: [Việc]
image: assets/img/2023/02/26/nmap_intro.PNG
---

Nmap là một công cụ khá quen thuộc với tất cả những ai làm trong lĩnh vực system, network hoặc security. Nmap hỗ trợ hàng tá các tính năng tương ứng với đó là list các option rất dài và rối. Điều này khiến cho bất cứ ai khi mới sử dụng nmap khá bối dối (Cả tôi cũng vậy), làm sao để nhớ đây???. Buổi ngày hôm nay sẽ note lại cách tiếp cận của tôi để việc làm việc với nmap trở nên hiệu quả hơn. 

# Các tính năng của nmap

Đầu tiên để mô tả các tính năng của nmap xin phép mượn cái hình sau để khái quát.

![nmap feature]({{site.url}}/assets/img/2023/02/26/nmap_feature.PNG)

Từ hình có thể thấy rằng tính năng của nmap có vẻ tập trung vào từng pha một của việc discovery thông tin trong mạng. Nmap có thể nhận đầu vào là một địa chỉ mạng (vd: 192.168.1.0/24)/một host (địa chỉ ip hoặc domain). Sau khi nhận được đầu vào này nó sẽ hỗ trợ ta xác định host nào trong mạng (trong t/h đầu vào là một địa chỉ mạng) hoặc host được nhập vào (trong t/h nhận vào là địa chỉ một host) vẫn còn sống (alive/online) hay không. Trong t/h host còn sống nmap có thể hỗ trợ ta xác định OS của host. Các port mở trên host. Các port này tương ứng với dịch vụ nào, version ra sao. Ngoài ra nmap còn cung cấp khả năng mở rộng bằng các script - tập lệnh tùy chọn áp dụng đối với 1 đối tượng (vd khi xác định được 1 port open thì thực hiện làm gì). Các kết quả scan/chạy script này có thể được lưu ra file với nhiều định dạng khác nhau.

Với mỗi công việc xác định thông tin nào đó (ta hay gọi là scan) nmap thường có các chiến lược (tatics)/kỹ thuật khác nhau. Lấy ví dụ để scan host còn sống nmap có thể sử dụng **các phương pháp khác nhau** VD: ARP scan, ICMP scan hoặc TCP/UDP scan. Tùy các chiến lược mà ta sử dụng các option khi chạy command khác nhau. Việc lựa chọn chiến lược nào (hoặc cũng có thể kết hợp nhiều chiến lược nào với nhau) tùy thuộc vào từng hoàn cảnh cụ thể của người sử dụng. Ví dụ scan trong LAN thì mới dùng được ARP hoặc sử dụng 1 số chiến lược SYN scan để bypass Firewall (Do một số loại FW có cơ chế hạn chế khả năng bị scan).

Sau khi hiểu được đại ý tính năng của nmap phần sau chủ yếu ta sẽ tập trung vào làm rõ cách sử dụng các option trong command sao cho hợp lý. Cách tiếp cận thông thường của tôi để xác định sử dụng option nào sẽ tham khảo từ cheetsheet (đây là cheetsheet mà tôi thấy có vẻ ok nhất tìm được trên mạng) [Link Cheetsheet NMAP]({{site.url}}/assets/img/2023/02/26/nmap_cheet_sheet_v7.pdf). Trong trường hợp cheetsheet không có thì tìm google hoặc hỏi ChatGPT thôi (Lựa chọn mới). Anw, thời buổi hiện nay làm gì cũng được miễn là sử dụng từ khóa tìm kiếm phù hợp mà thôi.

>Từ đây trở về sau bất cứ chỗ nào sử dụng sudo là cần quyền root để có thể thực hiện scan thành công. Lý do là với mốt số chiến lược nmap cần sử dụng đặc quyền cao để thực hiện một số tác vụ mà ở user thường OS không cho phép thực hiện.

# Một số option chung

Nmap có một số option chung mà ta có thể sử dụng ở tất cả các loại scan. Cụ thể như phần đầu của cheetsheet bên trên

![common_options]({{site.url}}/assets/img/2023/02/26/common_options.PNG)

Áp dụng: Lấy ví dụ thay bằng việc phải gõ vào từng subnet/host từ command ta có thể đặt chúng trong 1 file tên là **targets.txt** rồi sử option `-iL` để load file này vào nmap scan (Dĩ nhiên scan cái gì ta sẽ kết hợp thêm các options khác), mỗi host/subnet là một dòng. Ví dụ: để scan host alive bằng ARP(-PR) không scan port (-sn) trong mạng LAN ta sử dụng lệnh sau:

`sudo nmap -PR -sn -iL targets.txt`



# Nmap live host Discovery ~ Xác định host còn sống (alive/online) trong mạng

Khi cần xác định các host trong mạng hoặc một host còn sống (alive/online) ta có thể sử dụng một số phương pháp như sau.

![host alive overview]({{site.url}}/assets/img/2023/02/26/host_alive_overview.PNG)

Chú ý: Do chỉ cần xác định host alive không quan tâm tới port, dịch vụ nên ta thường xuyên sử dụng option `-sn`: Bỏ qua scan port chỉ scan host sống hay không để giảm thời gian scan.

Có cái nhìn tổng quan rồi, bây giờ giả sử muốn sử dụng chiến lược dùng ARP scan ta đi hỏi chat gpt thôi.

![chat gpt]({{site.url}}/assets/img/2023/02/26/host_alive_arp.PNG)

Thêm option load from file nào

![chat gpt arp and load list]({{site.url}}/assets/img/2023/02/26/combine_list_arp.PNG)

Với các chiến lược còn lại ta làm tương tự thôi. Cái này easy quá phải không


