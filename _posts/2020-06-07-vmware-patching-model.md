---
layout: post
title: "Một số thuật ngữ liên quan tới patching trong các sản phẩm của VMWare,"
author: "remi"
categories: job
tags: [Việc]
image: assets/img/2019/05/31/wakeupintro.png
---

Patching/Update/Upgrade vốn đã là việc đau đầu, thỉnh thoảng còn đau đầu hơn vì liên quan tới chuyện này các ông lớn ông bé còn nghĩ ra đủ thứ khái niệm liên quan na ná nhau rất dễ nhầm lẫn nữa. Buổi ngày hôm nay sẽ là một bài tổng kết về các khái niệm có thể gặp liên quan tới vấn đề này của một ông khá nổi là VMWare - Ông chùm của ảo hóa.

# Software versioning

Trước khi bắt đầu nói sơ qua về một cái chủ để liên quan gọi là "Software versioning" - Quy tắc gán số version cho một sản phẩm phần mềm.

Nói tới đây cần biết khái niệm "Scheme" - Lược đồ đánh số phiên bản tạo ra để theo dõi các phiên bản khác nhau của một phần mềm.

Có 2 schemes phổ biến
    + Internal version number: Tăng nhiều lần trong 1 ngày hay revision control number
    + Release version: Ít thay đổi hơn, được biết đến nhiều là semantic versioning hay project code name.

# Semantic versioning

Semantic versioning là một **Scheme** được sử dụng rộng rãi. Cấu trúc chung quy ước gồm các thành phần chính:

![Sematic version]( {{site.url}}/assets/img/2019/06/07/SemanticVersioning.png)

Ngoài ra trong nhiều trường hợp ta còn thấy nhà phát triển thêm 1 số kí hiệu alpha" (a), "beta" (b), or "release candidate" (rc) gán thêm vào phiên bản phần mềm. Dễ hình dung lấy một ví dụ cho dễ hiểu ý nghĩa của các kí hiệu này: 0.5, 0.6, 0.7, 0.8, 0.9 → 1.0b1, 1.0b2 (with some fixes), 1.0b3 (with more fixes) → 1.0rc1 (which, if it is stable enough), 1.0rc2 (if more bugs are found) → 1.0.

Một số bổ sung thêm 1 số thông tin revision/build

```
major.minor[.build[.revision]] (example: 1.2.12.102)
major.minor[.maintenance[.build]] (example: 1.4.3.5249)
```

Đánh giá về risk của sự thay đổi version ảnh hưởng lên hệ thống có thể tạm xếp loại ***major number (high risk) => minor number (medium risk) => patch number (lowest risk)***

# VMware versioning scheme

>Cũng sử dụng Semantic versioning 

## Một số khái niệm cần phải biết

- OEMs là VMware partners. Eg: Dell, HPE, VMware Cloud on AWS
- Third-party software providers: là các providers cung cấp I/O filters, device drivers, CIM modules, và **so on**
- VIB(vSphere Installation Bundle): là một dạng file cài đặt trong vmware. Mỗi VIB sẽ bao gồm các thành phần sau:
    + A file archive: ontains the files that make up the VIB.
    + An XML descriptor file (Meta data): dependencies, any compatibility issues, and whether the VIB can be installed without rebooting, information about bulletins.
    + A signature file: the level of trust associated with the VIB
    
- Standalone VIB: là VIB độc lập không thuộc/depend một thành phần logic nào
- Bulletins: Một nhóm một hoặc một vài vài VIB. Bulletins thì được định nghĩa trong meta data của VIB. Có 2 loại Bulletins là patch và Roll-up:
    + Patch: Là một update nhỏ của phần mềm để fix bug hoặc cải thiện phần mềm hiện tại. Một patch có thể bao gồm một hoặc một vài VIB
    + Roll-up Bulletin: Là một collectoin các patches được nhóm lại với nhau để tạo điều kiện thuận lợi cho tải xuống vài triển khai
    + Extension: Là một bulletin định nghĩa một nhóm MIB thêm thành phần cho ESXi host. Extension thường do bên thứ ba phát triển


