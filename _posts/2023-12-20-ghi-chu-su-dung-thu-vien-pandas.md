---
layout: post
title: "Ghi chú của toàn về việc sử dụng thư viện pandas trong xử lý file excel, csv,"
author: "remi"
categories: job
tags: ['Python']
image: assets/img/2023/12/20/01_intro_pandas.png
---

Công việc chính hàng ngày của mình thì không phải là Developer cũng không phải DA/DS/DE gì gì đó để phải thường xuyên thao tác xử lý dữ liệu nhiều. Tuy nhiên trong công việc nhiều khi cũng cần sử dụng python để xử lý file có định dạng theo raw-column như csv/excel sau đó xuất ra định dạng theo ý muốn. Trước giờ cũng đã thử nhiều thư viện python để xử lý các file có định dạng kiểu này. Tuy nhiên thử tới thử lui thì nhận ra Pandas thực sự là chân ái. Mình hay có thói quen take note trên Notion. Nay cũng rảnh biên tập lại post lên đây một số đoạn code pandas hay sử dụng hy vọng "ngày nào ấy" có ai đó search GG, đọc bài viết của mình thì thật là "vui".

## Series & DataFrame sự khởi nguồn,
Trước khi bắt đầu làm việc với Pandas thì cần phải hiểu 2 khái niệm quan trọng nhất của nó là Series và DataFrame. Lấy tạm một cái hình cho dễ hình dung 2 khái niệm này là gì.

![02 series data frame]( {{site.url}}/assets/img/2023/12/20/02_series_dataframe.png){:width="600px"}

Có thể thấy rằng Series là một list các số integer, string, double, ... Ta vẫn hay hình dung list ở dạng dòng ~ nằm ngang nhưng với series thì hãy hình dung nó ở dạng cột ~ nằm dọc. Mỗi series có Label và Index của từng phần tử trong series.

Tập các Series ghép lại thành DataFrame. Sau này khi làm việc với Pandas ta sẽ chủ yếu làm việc ở mức DataFrame là chính. Trong các tutorial trên mạng hay có đoạn code `df = pd.pandas.read_csv(...)` thì df là viết tắt của DataFrame đó.

Bây giờ bắt đầu với việc thử tạo Series rồi sau đó là DataFrame nào. Mình thì hay dùng VisualstudioCode vừa đẹp lại Free. Kết hợp với Plugin Jupiter Notebook thì quá tuyệt vời.

#### Tạo thử Series từ List và Dict

![03 Create series]( {{site.url}}/assets/img/2023/12/20/03_create_series.png){:width="600px"}

Text code cho đoạn sample bên trên (dành cho người muốn copy để chạy)
```
import pandas as pd

#Tao tu List
a = [1, 7, 2]
sr = pd.Series(a)
print(sr)
print(sr[0]) # In ra phan tu dau tien. Index duoc tu dong sinh ra bat dau tu 0
sr= pd.Series(data, index = ["x", "y", "z"]) #Them index cho Series
print(sr)

#Tao tu Dict
calories = {"day1": 420, "day2": 380, "day3": 390}
sr= pd.Series(calories)
print(sr)
print(sr['day1']) # In ra phan tu co index la day1 Key trong dict dong vai tro la index cua Series
```
