---
layout: post
title: "Học cách sử dụng nmap,"
author: "remi"
categories: job
tags: [Việc]
image: assets/img/2022/08/10/tracing_intro.jpg
---

Nmap là một công cụ khá quen thuộc với tất cả những ai làm trong lĩnh vực system, network hoặc security. Nmap hỗ trợ hàng tá các tính năng tương ứng với đó là list các option rất dài và rối. Điều này khiến cho bất cứ ai khi mới sử dụng nmap khá bối dối (Cả tôi cũng vậy), làm sao để nhớ đây. Buổi ngày hôm nay sẽ note lại cách tiếp cận của tôi để việc làm việc với nmap trở nên hiệu quả hơn. 

# Các tính năng của nmap

Đầu tiên để mô tả các tính năng của nmap xin phép mượn cái hình sau để khái quát.

![nmap feature]({{site.url}}/assets/img/2023/02/26/nmap_feature.PNG)

Từ hình có thể thấy rằng tính năng của nmap có vẻ tập trung vào từng pha một của việc discovery thông tin trong mạng. Nmap có thể nhận đầu vào là một địa chỉ mạng (vd: 192.168.1.0/24)/một host (địa chỉ ip hoặc domain). Sau khi nhận được đầu vào này nó sẽ hỗ trợ ta xác định host nào trong mạng (trong t/h đầu vào là một địa chỉ mạng) hoặc host được nhập vào (trong t/h nhận vào là địa chỉ một host) vẫn còn sống (alive/online) hay không. Trong t/h host còn sống nmap có thể hỗ trợ ta xác định OS của host. Các port mở trên host. Các port này tương ứng với dịch vụ nào, version ra sao. Ngoài ra nmap còn cung cấp khả năng mở rộng bằng các script - tập lệnh tùy chọn đối với 1 đối tượng (vd khi xác định được 1 port open thì thực hiện làm gì). Các kết quả scan/chạy script này có thể được lưu ra file với nhiều định dạng khác nhau.

Với mỗi công việc xác định thông tin nào đó (ta hay gọi là scan) nmap thường có các chiến lược (tatics) khác nhau. Lấy ví dụ để scan host còn sống nmap có thể sử dụng **scác phương pháp khác nhau** vd: Arp scan, ICMP scan hoặc TCP/UDP scan. Tùy các chiến lược mà ta sử dụng các option khác nhau.

Sau khi hiểu được đại ý tính năng của nmap phần sau chủ yếu chỉ là các cheetsheet liệt kê các option phân theo từng tính năng. 

Tất cả các option sẽ tham khảo từ cheetsheet này 

[Cheetsheet NMAP]({{site.url}}/assets/img/2023/02/26/nmap_cheet_sheet_v7.pdf)


>Từ đây trở về sau bất cứ chỗ nào sử dụng sudo là cần quyền root để có thể thực hiện scan thành công. Lý do là với mốt số chiến lược nmap cần sử dụng đặc quyền cao để thực hiện một số tác vụ mà ở user thường OS không cho phép thực hiện.

## Nmap live host Discovery ~ Xác định host còn sống (alive/online) trong mạng

![host alive overview]({{site.url}}/assets/img/2023/02/26/host_alive_overview.PNG)

### Nmap Host Discovery Using ARP

`$ sudo nmap -PR -sn 10.10.210.6/24` 

+ `-PR`: Scan sử dụng arp
+ `-sn`: Không scan port

Cơ chế mô tả bằng hính sau:

![host alive arp]({{site.url}}/assets/img/2023/02/26/host_alive_arp.PNG)

### Nmap Host Discovery Using ICMP

#### Sử dụng ICMP echo request  (ICMP Type 8)

`sudo nmap -PE -sn 10.10.68.220/24`

+ `-PE`: Scan sử dụng echo request
+ `-sn`: Không scan port


Cơ chế mô tả bằng hính sau:

![host alive arp]({{site.url}}/assets/img/2023/02/26/host_alive_arp.PNG)

#### Sử dụng ICMP timestamp requests (ICMP Type 13)

`sudo nmap -PE -sn 10.10.68.220/24`

+ `-PE`: Scan sử dụng timestamp requests
+ `-sn`: Không scan port


Cơ chế mô tả bằng hính sau:

![host alive arp]({{site.url}}/assets/img/2023/02/26/host_alive_arp.PNG)


#### Sử dụng ICMP address mask queries (ICMP Type 17)

`sudo nmap -PM -sn 10.10.68.220/24`

+ `-PM`: Scan sử dụng mask queries
+ `-sn`: Không scan port


Cơ chế mô tả bằng hính sau:

![host alive arp]({{site.url}}/assets/img/2023/02/26/host_alive_arp.PNG)