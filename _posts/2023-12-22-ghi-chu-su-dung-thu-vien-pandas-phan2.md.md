---
layout: post
title: "[Pandas-P2] Ghi chú về việc sử dụng thư viện pandas trong xử lý file excel, csv,"
author: "remi"
categories: job
tags: ['Python']
image: assets/img/2023/12/22/00_pandas_intro.png
---

Phần trước tôi đã giới thiệu qua cho mọi người một số khái niệm cơ bản cần biết trước khi tiếp cận sử dụng Pandas. Phần này sẽ tập trung vào ghi chú một số **Code Snippet**  Pandas hay sử dụng.

### Phần khởi tạo
Để dễ theo dõi toàn bộ các đoạn Code Snippets phía sau được sử dụng để thao tác một DataFrame chung như sau.

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

#### Không thích thì có thể import từ csv hoặc excel vào quá dễ dàng
```
import pandas as pd

df = pd.read_csv('data.csv') #Doc tu csv

df = pd.read_excel('data.xlsx', sheet_name='trang1') #Doc tu excel voi sheet trang1
```
![read excel]( {{site.url}}/assets/img/2023/12/22/02_pandas_excel.png){:width="900px"}



