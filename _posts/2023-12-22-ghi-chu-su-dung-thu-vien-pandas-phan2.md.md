---
layout: post
title: "[Pandas-P2] Ghi chú về việc sử dụng thư viện pandas trong xử lý file excel, csv,"
author: "remi"
categories: job
tags: ['Python']
image: assets/img/2023/12/22/00_pandas_intro.png
---

Phần trước tôi đã giới thiệu qua cho mọi người một số khái niệm cơ bản cần biết trước khi tiếp cận sử dụng Pandas. Phần này sẽ tập trung vào ghi chú một số **Code Snippet**  Pandas hay sử dụng.

## Phần khởi tạo
Để dễ theo dõi toàn bộ các đoạn Code Snippets phía sau được sử dụng để thao tác một DataFrame, cũng quy ước luôn tên nó là **df** chung như sau.

```
import pandas as pd
  
data = {
    'author' : ['Jitender', 'Purnima', 'Arpit', 'Jyoti'],
    'article' : [210, 211, 114, 178]
}
df= pd.DataFrame(data) #Toi day da co mot DataFrame voi ten la df
```

Với hình dung thực tế như sau
![Init dataframe]( {{site.url}}/assets/img/2023/12/22/01_pandas_init.png){:width="900px"}

Không thích thì có thể import từ csv hoặc excel vào quá dễ dàng
```
import pandas as pd

df = pd.read_csv('data.csv') #Doc tu csv

df = pd.read_excel('data.xlsx', sheet_name='trang1') #Doc tu excel voi sheet trang1
```
![read excel]( {{site.url}}/assets/img/2023/12/22/02_pandas_excel.png){:width="900px"}

Phê nhất là khi ghi xuống file khỏi cần suy nghĩ chỉ cần với một đoạn code như sau:
```
df.to_csv("output.csv") #Ghi ra file csv output.csv
df.to_excel("output.xlsx") #Ghi ra file excel ouput.xlsx
```
## Thao tác với row
### Lấy ra row nào đó (Dựa trên index hoặc Label)
Có thể sử dụng attribute `iloc[]` nếu lấy ra row theo Index hoặc dùng attribute `loc[]` nếu lấy theo label của row (Không nhớ index với label là gì đọc lại bài viết trước. P/S: loc chắc là viết tắt của locate chăng???

Nếu lấy ra theo index:
```
df.iloc[start:stop:step] 

Với start: index của dòng đầu cần lấy; stop: index của dòng cuối cần lấy; step: Khoảng cách/bước nhảy giữa các index cần lấy (default là 1). Chú ý đây là index luôn bắt đầu từ 0 nghĩa là index=1 sẽ là phần tử thứ 2, index=2 là phần tử thứ 3, ...
```

Ví dụ:
```
# Select Rows by Integer Index
df1 = df.iloc[2]     # Select Row by Index
df1 = df.iloc[[2,3,6]]    # Select Rows by Index List
df1 = df.iloc[1:5]   # Select Rows by Integer Index Range
df1 = df.iloc[:1]    # Select First Row
df1 = df.iloc[:3]    # Select First 3 Rows
df1 = df.iloc[-1:]   # Select Last Row
df1 = df.iloc[-3:]   # Select Last 3 Row
df1 = df.iloc[::2]   # Selects alternate rows
```

Nếu lấy theo Label
```
df.loc[start:stop:step] #Lay ra phan tu dua tren i
```
Ví dụ:
```
# Select Rows by Index Labels
df1 = df.loc['r2']          # Select Row by Index Label
df1 = df.loc[['r2','r3','r6']]    # Select Rows by Index Label List
df1 = df.loc['r1':'r5']     # Select Rows by Label Index Range
df1 = df.loc['r1':'r5']     # Select Rows by Label Index Range
df1 = df.loc['r1':'r5':2]   # Select Alternate Rows with in Index Labels
```
### Lẩy ra row dựa trên điều kiện nhất định


