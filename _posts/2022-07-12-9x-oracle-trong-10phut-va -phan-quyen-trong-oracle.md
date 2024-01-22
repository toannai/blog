---
layout: post
title: "Quick QuickStart về Oracle DB và phân quyền trong Oracle DB,"
author: "remi"
categories: job
tags: ['Unknown']
image: assets/img/2022/07/12/intro.png
---

Bản thân không phải là DBA - là tờ giấy trắng về Oracle DB thì đúng hơn. Tuy nhiên vì yêu cầu công việc vẫn muốn hiểu sơ sơ về các khái niệm **cơ bản** trong Oracle đủ để "Hiểu về cơ chế phân quyền của Oracle" là lý do của bài viết này. Ai muốn mỳ ăn liền - Quick QuickStarted với Oracle và Hiểu về cơ chế phân quyền trên Oracle thì nên đọc bài viết này. 

>P/S: Nhiều anh em cứ bảo rảnh thế có thời gian viết blog nhưng sự thật bận sml (vài tháng mới post được một bài). Chỉ là Note là thói quen tốn thời gian nhưng vẫn muốn giữ vì nhiều lúc đọc xong không note quên luôn sau cần lại mất công đọc lại. Notion Collection cá nhân còn rất nhiều thứ nhưng không có thời gian biên tập thành post trên blog. 

![Notion]( {{site.url}}/assets/img/2022/07/12/notion.png)

# Các khái niệm căn bản trong OracleDB

Để nói về Oracle tôi thấy người ta hay chia ra kiến trúc Process - MEM - File store. Cũng điểm qua để xem nó là cái chi. 

## Process trong Oracledb

Oracle chạy theo mô hình client -> Server. Trên client sẽ chạy 1 process gọi là **Client Process**. Khi người dùng (user) đã kết nối tới được server (authenticated), client process này sẽ giao tiếp với **Server Process** thông qua các **Listener**. Ở mức Network Listener này sẽ được lắng nghe trên một **Port** xác định (Thường là TCP/1521). Mỗi một lần Client Process --giao tiếp--> Server Process dựa trên danh tính của một người dùng nhất định gọi là một **Session**. Dĩ nhiên cũng như các session của các protocol thông thường khác, một Session có thể bao gồm 1 hoặc một vài **Connection** (Thường là vài chứ không mấy khi là 1).

Mỗi server process được cấp 1 không gian bộ nhớ trên server gọi là **PGA** (Program Global Area).

Vừa nói tới PGA(Program Global Area) là vùng nhớ được cấp cho 1 server process nhằm phục vụ một hoặc một vài session. Nói tới đây ta sẽ tự hỏi, Vậy giữa các PGA có các nào để chúng có khả năng **chia sẻ/tương tác** với nhau không? Dĩ nhiên là có. Phần vùng nhớ giúp việc chia sẻ/tương tác gọi là SGA (System Global Area).

Ngoài ra trên server còn có các **Background Process** – các process xử lý nội tại của oracle bao gồm: DBWn, CKPT, LGWR, ARCn, .. với nhiều mục đích: có cái thì ghi dữ liệu từ Memory xuống đĩa cứng, có cái thì dọn dẹp bãi chiến trường mỗi khi Server process hoạt động xong, có cái thì đứng ngoài ghi chép lại các thay đổi do Server process sinh ra. Chúng âm thầm, liên tục hoạt động đằng sau Database, để giữ cho Database luôn vận hành trơn tru.

Tất cả các thành phần Process nằm ở server bao gồm SGA và Backgroup Process sẽ được gom lại thành một tổ chức hoàn chỉnh, được quản lý gọi là **Oracle Instance**.
 
## Cấu trúc bộ nhớ (MEM)

Phần này không nói nhiều vì căn bản cũng đã mô tả kha khá ở trên. Không quá chi tiết nhưng đủ hiểu - mà không phải DBA nên cũng chỉ cần vậy thôi là đủ. Chỉ xin post cái hình, khỏi giải thích nhiều. 

![Process]( {{site.url}}/assets/img/2022/07/12/process.png)


## Các loại file trong OracleDB

Cơ sở dữ liệu dùng để lưu trữ => Cốt yếu dữ liệu chắc chắn sẽ cần phải lưu trữ persistence dưới dạng các file vật lý trên ổ đĩa. Oracle có một số loại file quan trọng sau sau:

* Datafiles: Các database datafiles chứa toàn bộ dữ liệu trong database.
* Redo Log Files: Chức năng chính của redo log là ghi lại tất cả các thay đổi đối với dữ liệu trong database. Redo log files được sử dụng để bảo vệ database khỏi những hỏng hóc do sự cố. Các thông tin trong redo log file chỉ được sử dụng để khôi phục lại database trong trường hợp hệ thống gặp sự cố và không cho phép viết trực tiếp dữ liệu trong database lên các datafiles trong database
* Control Files: Control file chứa các mục thông tin quy định cấu trúc vật lý của database như: Tên của database, Tên và nơi lưu trữ các datafiles hay redo log files, Time stamp (mốc thời gian) tạo lập database. 


## Database và Instance thứ cần nói trước khi nói các phần sau

>Hồi ĐH ae hay được học về MySQL. Trong MySQL thì hay quen có nhiều Database. Oracle DB thì hơi khác chút - thật ra là rắc rối hơn ^^

Một Oracle Instance sẽ chỉ quản lý một Database duy nhất. Tuy nhiên một Database thì lại có thể được quản lý bởi một (single-instance) hoặc nhiều Oracle Instance (multi-instance).

![Instance]( {{site.url}}/assets/img/2022/07/12/instance.png)

Cập nhật: Từ phiên bản 12c. Oracle đưa ra một kiến trúc mới. Gọi là Multitenant. Trong kiến trúc Multitenant này, chúng ta sẽ có 1 Database mẹ, hay còn gọi là Container Database (CDB). Database này là Database khởi thủy, bắt buộc phải có trong kiến trúc. Nó có nhiệm vụ lưu trữ, quản lý các thông tin điều khiển chung. Nó cũng sẽ có các background process để xử lý các công việc chung. Ngoài ra, chúng ta sẽ có các Database con, gắn vào Database mẹ, gọi là các Pluggable database (PDB). Đây mới chính là nơi lưu trữ các dữ liệu thực sự của người dùng. 

![Multi tenant]( {{site.url}}/assets/img/2022/07/12/multi_instance.png)

>Thực ra, CDB cũng có thể lưu trữ dữ liệu thực sự, nhưng Oracle khuyến nghị không nên lưu dữ liệu trong CDB để tối ưu cũng như tránh các xung đột ảnh hưởng đến hiệu năng. Đối với người dùng, PDB hoàn toàn giống với các Database bình thường, từ cách kết nối đến các đối tượng bên trong như user, bảng, view, procedure. 

Đối với người dùng, PDB hoàn toàn giống với các database bình thường, từ cách kết nối đến các đối tượng bên trong như user, bảng, view, procedure. Còn xét sâu hơn ở kiến trúc phía dưới, các PDB sống dựa vào CDB. CDB cung cấp các process, memory để giữ cho PDB hoạt động.

Có một PDB đặc biệt được sinh ra khi mới tạo CDB, nó gọi là PDB hạt giống (PDB$SEED). Nó giống như 1 mẫu (hay template) để mỗi khi cần tạo ra một PDB mới, Oracle sẽ sử dụng PDB$SEED này để làm mẫu.

![Sed DB]( {{site.url}}/assets/img/2022/07/12/sed_db.png)


## Cấu trúc logical OracleDB

Tôi tạm chia phần này thành 2 thứ gọi là cấu trúc lưu trữ và cấu trúc tổ chức dữ liệu. 

* Cấu trúc lưu trữ: Mô tả việc lưu trữ các thành phần logic thế nào trên ổ cứng – Là một phép link giữa các thành phần logic và cách lưu trữ trên ổ cứng. 
* Cấu trúc tổ chức: Mô tả việc tổ chức các thành phần/đối tượng logic được tổ chức thế nào – Cái nào bao cái nào. 

### Nói về cấu trúc lưu trữ trước

![storage arch]( {{site.url}}/assets/img/2022/07/12/storagearch.png)

Một database có thể được phân chia về mặt logic thành các đơn vị gọi là các **Tablespaces**.  Tablespaces thường bao gồm một nhóm các thành phần có quan hệ logic với nhau. Khi tạo Tablespace sẽ cho ta lựa chọn nơi lưu trữ các **Data file**, có thể chọn lưu trữ một hoặc nhiều data file. Mỗi table trong csdl sẽ được lưu trữ trong 1 **Segment** nằm trong 1 tables space. Mỗi row trong table được lưu trữ  trong một hoặc nhiều **Block**. Mỗi block thuộc các extends. Nói cho có chứ cũng không cần quan tâm chi tiết tới block, extend, segment làm gì vì phần này căn bản oracledb care cho ta hết rồi chỉ cần quan tâm tới mức Tablespace và Datafile thôi.

### Nói về cấu trúc tổ chức có một số khái niệm như sau

* User: Người dùng trên oracle. Mỗi user có một username.
* Schema: Là tập các đối tượng được (tables, views, …) mà thuộc về một user.

Khi tạo user bằng lệnh create user  oracle sẽ tự động tạo một schema “rỗng” cho user tên schema mặc định là username luôn. Sau này ta sử dụng lệnh grant để thêm các đối tượng vào schema của user.

Nói hoài về database. Vậy database là gì? Database là thứ bao gồm tất cả users và dữ liệu của các users (table, view, index, …). 

Từ các định nghĩa trên thấy mối quan hệ giữa các đối tượng đó: Database là thứ bao gồm tất cả. Một database có nhiều user mỗi user gắn với một schema là wrapper của các đối tượng user được phép sử dụng. Database có nhiều table, view, procedure, view, index, … và đương nhiên schema cũng có thể có nhiều table, view, procedure, view, index theo định nghĩa trên – Nghe na ná nhau nhưng qua mô tả trên rõ ràng là khác nhau mà. P/S: Ai người đi từ mySQL sang chắc sẽ rất hay confuse giữa database và schema :v 

# Nói về vấn đề phân quyền trên Oracle DB

Đây là phần tôi nghĩ là quan trọng ít nhất với những người như tôi người mà không phải DBA chỉ là System Security culi.

## Nói về user

Một user sẽ có 1 username và phương thức xác thực nhất định (password hoặc key). Thông thường người dùng đều được gán vào một Profile. Profile sẽ định nghĩa các chính sách áp dụng cho một user: VD Độ dài và độ phức tạp password, lockout account, … thậm trí là giới hạn bộ nhớ sử dụng cho user. Khi mới tạo ra, người dùng auto sẽ được gán vào Profile mặc định (hầu như không có chính sách gì), sau này ta có thể tạo một Custome Profile cấu hình các chính sách giới hạn theo ý mình rồi rồi gán User vào Custome Profile đó để thiết quân luật (Dĩ nhiên ta phải có đủ quyền làm việc đó).

Oracle có tạo sẵn kha khá một số user khi cài xong (SYS; SYSTEM; DBSNMP; OUTLN; MDSYS; CTXSYS; ANONYMOUS; XDB; ORDSYS; APEX_050000; APEX_PUBLIC_USER; DVF; LBACSYS; ORDDATA; OJVMSYS; ORDPLUGINS; WMSYS; OLAPSYS; EXFSYS; DMSYS)  tuy nhiên phần lớn là bị Lock. Trong đó có thường chỉ có 2 User được Open có thể connect được là SYS và SYSTEM. (Dùng `SELECT username, account_status FROM dba_users WHERE account_status = 'OPEN';` để liệt kê toàn bộ user active có thể Connect)

+ SYS: Người dùng SYS là một trong những người dùng quan trọng nhất trong Oracle. Nó có quyền cao nhất trong hệ thống và được sử dụng để quản lý cơ sở dữ liệu, thực hiện các tác vụ quản trị và cài đặt.
+ SYSTEM: Người dùng SYSTEM cũng có quyền quản trị, tuy nhiên, thường được sử dụng cho các tác vụ quản lý hàng ngày và không nên được sử dụng cho các tác vụ quản trị hệ thống như SYS.
 
## Nói về quyền

Quyền (privilege/Permission) là sự cho phép thực hiện 1 câu lệnh SQL nào đó hoặc được phép truy xuất đến một đối tượng nào đó (vd: quyền tạo bảng CREATE TABLE, quyền connect đến cơ sở dữ liệu CREATE SESSION, quyền truy vấn SELECT,…).

Quyền có thể gán trực tiếp cho user hoặc gián tiếp thông qua **Role** - nói sau (User được gán vào role, role định nghĩa tập quyền)

Có 2 loại quyền cơ bản là quyền hệ thống và quyền đối tượng.

![sys perm]( {{site.url}}/assets/img/2022/07/12/overviewperm.png) 
  
### Quyền hệ thống (System Privilege)

* Là các quyền đặc biệt mà người dùng có thể có để thực hiện các hoạt động quản lý toàn hệ thống. Quyền hệ thống thường được cấp cho các người dùng quản trị cơ sở dữ liệu hoặc các người dùng có nhiệm vụ thực hiện các tác vụ quản lý cơ bản. 

* Oracle database có khoảng 80 quyền hệ thống và con số này đang tiếp tục tăng lên. Các quyền hệ thống có thể chia ra như sau:

	* Các quyền cho phép thực hiện các thao tác mức độ rộng trên hệ thống ví dụ như: CREATE SESSION, CREATE TABLESPACE.
	* Các quyền cho phép quản lý các đối tượng thuộc về một user ví dụ: CREATE TABLE
	* Các  quyền cho phép quản lý các đối tượng trong bất cứ một schema nào ví dụ câu lệnh: CREATE ANY TABLE.

* Có thể điều khiển các quyền bằng cách câu lệnh GRANT hay REVOKE.

* Gán quyền hệ thống cho một user:
>User có thể cấp 1 quyền hệ thống nếu có một trong các điều kiện sau:
>	-	User đã được cấp quyền hệ thống với tùy chọn WITH ADMIN OPTION.
>	-	User có quyền GRANT ANY PRIVILEGE. 

![sys perm]( {{site.url}}/assets/img/2022/07/12/sysperm.png) 

Cú pháp chi tiết:

```
GRANT {system_priv|role}
	[, {system_priv|role} ]...
	TO {user|role|PUBLIC}
	[, {user|role|PUBLIC} ]...
	[WITH ADMIN OPTION]

Trong đó
+ system_priv:  Chỉ định quyền hệ thống sử dụng
+ role: Chỉ định tên chức danh được gán
+ PUBLIC: Gán quyền hệ thống cho tất cả các user
+ WITH ADMIN OPTION:  Cho phép user được gán quyền có thể gán quyền hay chức danh đó cho user khác.
```

Ví dụ:       

```
GRANT CREATE SESSION, CREATE TABLE
TO user1;
Hoặc
GRANT CREATE SESSION TO scott
WITH ADMIN OPTION;
```                      

* Một số chú ý khác khi gán quyền hệ thống
	* Để có quyền hệ thống, user cần được gán quyền có kèm thêm tuỳ chọn  WITH ADMIN OPTION.
	* Người được gán với tuỳ chọn WITH ADMIN OPTION có thể tiếp tục gán cho một user khác quyền hệ thống với WITH ADMIN OPTION.
	* Bất cứ một user nào có quyền GRANT ANY ROLE có thể gán bất kì quyền nào trong database.
	* Người được gán với tuỳ chọn WITH ADMIN OPTION có thể gán quyền hay lấy lại các quyền từ  bất cứ user hay role nào trong database.

* Oracle có một số quyền quản trị hệ thống (administrative privileges) đặc biệt cần biết:

![special perm]( {{site.url}}/assets/img/2022/07/12/specialrole.png) 

###	Quyền đối tượng Quyền đối tượng (Object Privilege)

Là quyền thực hiện một hành động cụ thể trên một đối tượng cụ thể, dùng để quản lý việc truy xuất đến các đối tượng của schema cụ thể nào đó.User có thể cấp 1 quyền đối tượng nếu có một trong các điều kiện sau:

* User có thể cấp bất kỳ quyền đối tượng trên bất kỳ đối tượng nào thuộc sở hữu của mình cho user khác.
* User có quyền GRANT ANY OBJECT PRIVILEGE.
* User được cấp quyền đối tượng đó với tùy chọn WITH GRANT OPTION.
 
![object perm]( {{site.url}}/assets/img/2022/07/12/objectperm.png) 

### Nói về Role

Oracle cũng hỗ trợ RBAC tạo ra các role định nghĩa một tập các quyền. User khi được gán vào role nào thì sẽ có quyền tương ứng nằm trong tập quyền của role

Oracle cũng định nghĩa sẵn một số "build-in" role
 
![buildin role]( {{site.url}}/assets/img/2022/07/12/buildinrole.png) 

Sẽ không nói về grant quyền là thế nào vì đó chỉ là vấn đề search google khi đã hiểu cơ bản những nội dung trên.