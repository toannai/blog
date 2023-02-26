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

Khi cần xác định các host trong mạng hoặc một host còn sống (alive/online) ta có thể sử dụng một số chiến lược/kỹ thuật như sau.

![host alive overview]({{site.url}}/assets/img/2023/02/26/host_alive_overview.PNG)

Chú ý: Do chỉ cần xác định host alive không quan tâm tới port, dịch vụ nên ta thường xuyên sử dụng option `-sn`: Bỏ qua scan port chỉ scan host sống hay không để giảm thời gian scan.

Có cái nhìn tổng quan rồi, bây giờ giả sử muốn sử dụng chiến lược dùng ARP scan ta đi hỏi chat gpt thôi.

![chat gpt]({{site.url}}/assets/img/2023/02/26/host_alive_arp.PNG)

Thêm option load from file nào

![chat gpt arp and load list]({{site.url}}/assets/img/2023/02/26/combine_list_arp.PNG)

Với các chiến lược còn lại ta làm tương tự thôi. Cái này easy quá phải không


# Nmap Port scan ~ Xác định các port được mở trên host

Trước khi bắt đầu ta cần hiểu một số trạng thái của port mà nmap có thể trả lại khi ta thực hiện scan port. Các trạng thái có thể xảy ra bao gồm:

+ `Open` (mở): Điều này có nghĩa là một ứng dụng đang lắng nghe kết nối trên cổng đó.
+ `Closed` (đóng): Điều này có nghĩa là cổng không có ứng dụng nào lắng nghe kết nối.
+ `Filtered` (được lọc): Điều này có nghĩa là cổng đó đang bị chặn bởi tường lửa hoặc bởi các thiết bị bảo mật khác.
+ `Unfiltered` (không được lọc): Điều này có nghĩa là Nmap không thể xác định trạng thái của cổng.
+ `Open|Filtered` (mở hoặc được lọc): Điều này có nghĩa là Nmap không thể xác định chắc chắn liệu cổng đó có được mở hay không do bị chặn bởi tường lửa hoặc các thiết bị bảo mật khác.

Khi cần xác định một/các port được mở trên 1 host ta có thể sử dụng một số chiến lược/kỹ thuật scan như sau:

![Port scan]({{site.url}}/assets/img/2023/02/26/port_scan.png)

TCP là giao thức truyền tải hướng kết nối. Có quá trình bắt tay 3 bước để khởi tạo kết nối và thông báo khi đóng kết nối mô tả bằng hình bên dưới (Cái này quá quen rồi). Các chiến lược TCP scan hầu hết thực hiện bằng việc gửi một hoặc 1 số gói tới đích cần scan và **nghe ngóng** kết quả phàn hồi để xác định port mở hay đóng. Chi tiết việc **gửi** và **ngóng** được mô tả khá dễ hiểu bằng các hình sau trong cheetsheet:

![Tacstic Port scan]({{site.url}}/assets/img/2023/02/26/tacstic_scan.PNG)

Còn với UDP thì khác, không thực hiện bắt tay khi mở kết nối nên xác định trạng thái port có thể phức tạp hơn và đôi khi là mất nhiều thời gian hơn. Chiến lược scan của nmap trong trường hợp này thường là cố gắng gửi một hoặc một số gói tin UDP tới các cổng của mục tiêu cần nhắm tới. Với hầu hết các cổng các gói tin gửi đi này thường không có nội dung (payload) ngoại trừ một số cổng thông thường thì có thể gửi thêm payload. Sau đó tcpdump sẽ chờ phản hồi từ phía target. Dựa vào việc phản hồi hay không mà nó xác định trạng thái của cổng.

![Udp Port scan]({{site.url}}/assets/img/2023/02/26/udp_response.PNG)

Tới đây ta vẫn tiếp tục áp dụng phương án sử dụng chat gpt để tìm hiểu chi tiết khi đã biết được tổng quan về các chiến lược scan như trên.

# Scan OS, service và sử dụng NSE (Nmap Scripting Engine)

### Scan OS

Nmap cũng hỗ trợ ta xác định OS của target dựa trên finger sprint của OS. Có thể sử dụng cú pháp sau để sử dụng tính năng này

`nmap -O <target>` 


### Scan service

Ngoài nmap cũng hỗ trợ ta xác định các dịch vụ (Dịch vụ là gì VD: http,https,ftp,...) và thông tin liên quan của dịch vụ (version) đang sử dụng port open trên target. Sử dụng option `-sV` cho tính năng này

`sudo nmap -sV --version-intensity 9 <target>` 

Trong đó: `-sV`: Là option chỉ ra rằng nmap sẽ scan detect service .`--version-intensity [0-9]` là option để tăng mức độ chi tiết của thông tin liên quan tới dịch vụ ta thực hiện scan. Chỉ số càng cao mức độ chi thông tin chi tiết càng nhiều. 


### NSE scrip trong nmap

Script trong Nmap là các chương trình viết bằng ngôn ngữ Lua, được sử dụng để tự động hóa các nhiệm vụ trong quá trình quét mạng. Các script này được sử dụng để thực hiện các tác vụ như phát hiện lỗ hổng bảo mật, thu thập thông tin từ các dịch vụ mạng và xác định các thiết bị trong mạng

nmap có rất nhiều script mặc định lưu tại vị trí sau `/usr/share/nmap/scripts/` . Để gọi toàn bộ script tại vị trí này ta sử dụng option `-sC` lúc này nmap sẽ tự động load và sử dụng toàn bộ các script. Ví dụ: `nmap -sC 192.168.1.1 -p 80`

Ngoài ra ta cũng có thể tự gọi script cụ thể bằng option sau

`nmap -sV -script=<script name> <target>` . Ví dụ: `nmap -sV -script=http-title,http-headers 192.168.1.1 -p 80`


# Lưu kết quả nmap ra file

Mặc định nmap sẽ đẩy kết quả scan ra màn hình console. Ta có thể đẩy kết quả này ra file với một số định dạng như sau:

![OUtput Port scan]({{site.url}}/assets/img/2023/02/26/output.PNG)

# Hãy nhớ

Bài viết khá dài và nhiều nội dung nhưng tôi nghĩ điều quan trọng là bạn nắm được các tính năng của nmap một cách có hệ thống thôi, còn lại chi tiết hãy để Google và chat GPT lo. Cái đầu của chúng ta quá nhỏ bé để nhớ tất cả mọi thứ phải không.

Thks bạn đã đọc bài viết của tôi. 

