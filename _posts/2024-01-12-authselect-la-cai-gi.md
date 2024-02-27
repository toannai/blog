---
layout: post
title: "Ghi chú về authselect trên Linux,"
author: "remi"
categories: job
tags: ['Linux']
image: assets/img/2024/01/12/intro.png
---

Đợt này đang có một số việc trên linux có gặp một toy khá mới gọi là authselect. Search thì thấy vietnam chưa ông nào viết về cái này thôi thì biên tập lại cái note trên notion của mình về authselect post lên đây để google nó index cho trang và cũng hy vọng là nhỡ có ngày có ai cần nó thì sao :) cũng OK mà.

## PAM (Pluggable Authentication Modules) thứ tương đối là quen thuộc,

Nói về authselect thì phải nói tới PAM (Pluggable Authentication Modules) trên linux. Mà nói về PAM thì khá dài dòng đấy! nhưng chỉ xin mô tả ngắn gọn: "Hình dung nó nó như project để centralize việc xác thực trên linux" như hình mô tả sau.

![01 pam]( {{site.url}}/assets/img/2024/01/12/01_pam.png){:width="600px"}

>Khi app/toy chạy trên linux, thay bằng việc mỗi ông phải code/xây dựng một cơ chế xác thực cho app/toy của mình thì ổng có thể sử dụng luôn PAM để xác thực luôn - PAM thành chỗ xác thực tập trung. Việc xác thực tập trung tại PAM nó có nhiều lợi ích như: Muốn thêm/sửa/xóa hoặc disable một ông user trên hệ thống thì ổng chỉ cần xử tại một chỗ mà thôi khỏi cần làm tại nhiều chỗ. Tương tự muốn áp dụng chính sách như mật khẩu mạnh, lockout mật khẩu nếu nhập sai, ... vân vân và mây mây cũng chỉ cần làm tại PAM mà không phải làm tại nhiều chỗ khác (Những chỗ khác tựu được áp dụng).

Ai làm việc PAM chắc hẳn đều biết thư mục ```/etc/pam.d/``` thần thánh để cấu hình điều chỉnh hoạt động của PAM. Riêng mình vẫn nhớ mấy lần đầu mở mấy file ở thư mục này ra đọc chả hiểu cái mọe gì :'(. Thời làm Viettel có lần cấu hình sai ở đây còn không vào được server luôn, phải phi sang tận sang DC ở Giang Văn Minh để recorver. Dính phốt nên mỗi lần cấu hình ở đây khá là rén.

Quay lại với authenselect: Để cái việc này nó dễ dàng tiếp cận hơn chút thì Redhat nó có cái toy gọi là "authselect" nhằm hỗ trợ việc cấu hình này cho nó đơn giản và an toàn hơn. Theo định nghĩa của chatgpt xin phép trích xuất:

>Authselect là một công cụ trong hệ thống Linux được sử dụng để quản lý cấu hình của các dịch vụ xác thực như PAM (Pluggable Authentication Modules) trên hệ thống. Authselect giúp đơn giản hóa quá trình cấu hình xác thực bằng cách cung cấp các hồ sơ (Profile) cài đặt được đặc trưng cho môi trường xác thực cụ thể.
>
>Mục tiêu chính của Authselect là đơn giản hóa quá trình cài đặt và quản lý các cấu hình xác thực trên hệ thống Linux, đồng thời đảm bảo tính linh hoạt và tùy chỉnh. Authselect thường được sử dụng trong các bản phân phối Linux như Fedora và CentOS để quản lý xác thực hệ thống.

## Profile điểm khởi đầu của authenselect

Đầu tiên hãy nhớ đoạn này trong định nghĩa **Authselect giúp đơn giản hóa quá trình cấu hình xác thực bằng cách cung cấp các hồ sơ (Profile) cài đặt được đặc trưng cho môi trường xác thực cụ thể,**

Ý tưởng chính của Authselect đó là tập các thiết lập của PAM (VD: Chính sách yêu cầu độ phức tạp của mật khẩu, lockout khi nhập mật khẩu sai quá nhiều lần, ...) được tổ chức thành các Profile <=> Mỗi profile bao gồm các cấu hình nhất định cho PAM. 

Đương nhiên trên một hệ thống linux ta có thể có nhiều Profile cùng lúc (Dĩ nhiên hai Profile có thể giống y hệt nhau về tập các thiết lập miễn là tên khác nhau là nó khác nhau). Việc switch qua switch lại giữa các profile đã định nghĩa tương ứng với việc chọn tập các cấu hình định nghĩa sẵn (Pre Define) cho PAM. 

Nói tạm vậy giờ động tay vào gõ luôn cho dễ nhớ. Trước khi băt đầu cần nhớ: **Để làm việc với authselect sử dụng lệnh `authselect`**. Lệnh này hỗ trợ một loạt Option như sau:

![02 option]( {{site.url}}/assets/img/2024/01/12/02_option.png){:width="600px"}

Có nhiều option nhưng có một số là hay dùng nên nhớ thôi, còn số ít hay dùng thì thôi gg khi sử dụng vậy!

* Liệt kê toàn bộ các Profile trên hệ thống:

```authselect list```

![03 authselect list]( {{site.url}}/assets/img/2024/01/12/03_authselect_list.png){:width="600px"}

Mặc định trên hệ thống Redhat có định nghĩa sẵn một vài Profile cho ta: Các Profile này là gì thì đi hỏi chatgpt cho nhanh

![03 authselect list gpt]( {{site.url}}/assets/img/2024/01/12/03_authselect_list_chatpgt.png){:width="600px"}

* Kiểm tra xem mình đang xài profile nào thì sao? Thì sử dụng 

```authselect current```

![04 authselect current]( {{site.url}}/assets/img/2024/01/12/04_authselect_current.png){:width="600px"}

Như trên hình thì hiện ta đang sử dụng Profile **sssd** với 2 feature được enable: **with-fingerprint** và **with-silent-lastlog**. Điều này tương ứng với việc nó sẽ load thêm 2 module **fingerprint** và **silent-lastlog** - Hai module khá là nổi tiếng của PAM.

* Chuyển qua chuyển lại giữa các profile sử dụng 


```authselect select <profile name>```

* Mổ sẻ hơn về cấu trúc file của một **Profile**

Ban đầu vào thư mục cấu hình quen thuộc của PAM ```\etc\pam.d\```

![05 authselect pam.d]( {{site.url}}/assets/img/2024/01/12/05_authenselect_pam.d.png){:width="600px"}

Ố ồ ồ, ta thấy các file cấu hình quen thuộc được tạo link sang ```/etc/authselect/``` điều đó có nghĩa gì???? Nghĩa là lúc này authenselect sẽ quản lý cấu hình của PAM.

Tiếp tục chui vào đây 

![06 authselect dir]( {{site.url}}/assets/img/2024/01/12/06_authselect_dir.png){:width="600px"}


## Làm thế nào để tạo 1 Profile của mình

Đọc qua thì không thấy ai tạo Profile từ đầu cả, cách hướng dẫn toàn là tạo Base/Clone từ một cái Profile rồi sửa theo ý mình. OK giờ theo hướng dẫn tạo 1 cái custom-Profile xem sao

### Bước 1: Chạy lệnh sau để tạo một Profile base trên sssd (System Security Services Daemon) - Một Profile mặc định trên Redhat

```#authselect create-profile custom-profile -b sssd --symlink-meta```

Với option -b <=> base trên, --symblink-meta <=> Link meta data (Thông tin ghi chú) của Profile mới với meta data của Profile base.

Lúc này thử list thư mục ```/etc/authselect``` ta sẽ thấy có tạo ra một thư mục con trong thư mục trong custom

![06 authselect custome]( {{site.url}}/assets/img/2024/01/12/06_authselect_custom.png){:width="600px"}

Hãy chú ý phần bôi màu đỏ, ở đây ta cũng thấy Meta data (Thông tin ghi chú về profile) cũng được link sang thư mục meta data của base Profile **sssd**. Ngoài tra trong đây cũng chứa một số file với tên quen thuộc như system-auth, password-auth, nsswitch.conf. Mở thử một file ra thì ta thấy chúng ở dạng template khá dễ hiểu

![06 authselect template]( {{site.url}}/assets/img/2024/01/12/07_authselect_template.png){:width="600px"}

File này có một số phần mở ngoặc dạng ```{include/exclude ...}``` xác định đoạn cấu hình sẽ được load/unload với điều kiện module nào đó được load hay không.

Dĩ nhiên là ở đây muốn sửa ta sẽ sửa thêm các đoạn cấu hình PAM mà ta mong muốn để tạo lập một Profile cho riêng mình. Sau khi sửa theo đúng ý mình ta sẽ đi đến bước 2 là chọn và apply Profile của mình vào hệ thống. *PM: "Đoạn test sửa tôi sẽ thực hiện ở Bước 3"*.


### Bước 2: Chọn Profile và apply Profile đã cấu hình

Đầu tiên list ra toàn bộ Profile trên hệ thống

```authselect list```

Chọn Profile đã định nghĩa

```authselect select custom/custom-profile```

Apply Profile đã chọn

```authselect apply-changes```

### Bước 3: Thử sửa một cấu hình trong Profile xem cơ chế hoạt động thế nào

```nullok``` là option cấu hình cho module ```pam_unix.so``` trong PAM được sử dụng để quyết định cho phép password của user được phép rỗng hay không. Nếu có option này thì password được phép rỗng ngược lại password không được phép rỗng. Cấu hình này được thiết lập trong file ```/etc/pam.d/system-auth``` với đoạn cấu hình

```password    sufficient                                   pam_unix.so sha512 shadow  nullok use_authtok```

Qua ChatGPT thấy ý nghĩa các option như sau

![08 giaithich thay doi]( {{site.url}}/assets/img/2024/01/12/08_giaithich.png){:width="600px"}

Bây giờ sẽ sử dụng authselect để thiết lập cấu hình này. Để dễ hiểu tôi mô tả các bước thực hiện như sau: 

* Bước 1: Kiểm tra cấu hình option ```nullok``` trước update 

* Bước 2: Sửa đổi cấu hình nhưng chưa thực hiện chạy ```authselect apply-changes```  => Kiểm tra cấu hình option ```nullok```

* Bước 3: Thực hiện chạy ```authselect apply-changes```  => Kiểm tra cấu hình option ```nullok``` sau update

Thực hiện chi tiết:

* Bước 1: Kiểm tra cấu hình option ```nullok``` trước update  

Kiểm tra nội dung file ```/etc/authselect/custom/custom-profile/system-auth``` đoạn cấu hình ```nullok``` (Cho phép password null)

![08 before audit]( {{site.url}}/assets/img/2024/01/12/08_before_01.png){:width="600px"}

Kiểm tra nội dung ```/etc/pam.d/system-auth``` (Cũng là nội dung của /etc/authselect/system-auth do /etc/pam.d/system-auth là softlink của /etc/authselect/system-auth)

![08 before audit 2]( {{site.url}}/assets/img/2024/01/12/08_before_02.png){:width="600px"}

=> Ta thấy 2 file ```/etc/authselect/custom/custom-profile/system-auth``` và ```/etc/pam.d/system-auth``` đoạn cấu hình ``nullok`` tương tự nhau => Điều này là dễ hiểu vì từ trên đã nói ```/etc/authselect/custom/custom-profile/system-auth``` là template để sinh ra ```/etc/pam.d/system-auth```

* Bước 2: Sửa đổi cấu hình nhưng chưa thực hiện chạy ```authselect apply-changes```  => Kiểm tra cấu hình option ```nullok```

Lúc này audit file /etc/authselect/custom/custom-profile/system-auth bỏ cấu hình ```nullok``` <=> cho phép password rỗng

![08 after audit1]( {{site.url}}/assets/img/2024/01/12/08_after_01.png){:width="600px"}

**Chưa** chạy ```authselect apply-changes``` và kiểm tra file ```/etc/pam.d/system-auth```

![08 after audit2]( {{site.url}}/assets/img/2024/01/12/08_after_02.png){:width="600px"}

==> File template thay đổi nhưng cấu hình thực tế của PAM chưa thay đổi

* Bước 3: Thực hiện chạy ```authselect apply-changes```  => Kiểm tra cấu hình option ```nullok``` sau update

Chạy Update cấu hình Profile vào hệ thống:

```authselect apply-changes```

Kiểm tra lại thì thấy cấu hình của file cấu hình PAM thực sự được Update

![08 after audit2]( {{site.url}}/assets/img/2024/01/12/08_after_03.png){:width="600px"}

**Tổng hợp ngắn gọn lại!**: Như vậy để thiết lập cho một Profile ta sẽ sửa trong file template tương ứng của Profile nằm tại ```/etc/authselect/custom/<profile_name>``` rồi chạy apply. Lệnh này sẽ sinh ra file cấu hình của PAM tại ```/etc/authselect```. File này được link vào thưc mục cấu hình của PAM tại ```/etc/pam.d/```


