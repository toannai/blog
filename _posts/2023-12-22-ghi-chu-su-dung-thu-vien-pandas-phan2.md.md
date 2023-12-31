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

+ Nếu lấy ra theo index:

```
df.iloc[start:stop:step] 
```

Với **start**: index của dòng đầu cần lấy; **stop**: index của dòng cuối cần lấy; **step**: Khoảng cách/bước nhảy giữa các index cần lấy (default là 1). Chú ý đây là index luôn bắt đầu từ 0 nghĩa là index=1 sẽ là phần tử thứ 2, index=2 là phần tử thứ 3, ...

Ví dụ:
```
df1 = df.iloc[2]        # Chọn row có index=2
df1 = df.iloc[[2,3,6]]  # Chọn các row từ một index list
df1 = df.iloc[1:5]      # Chọn các row có index từ 1 -> 5
df1 = df.iloc[:1]       # Chọn row đầu tiên
df1 = df.iloc[:3]       # Chọn 3 row đầu tiên
df1 = df.iloc[-1:]      # Chọn row cuối cùng
df1 = df.iloc[-3:]      # Chọn 3 row cuối cùng
df1 = df.iloc[::2]      # Chọn các row theo bước nhảy là 2 từ 1 row đầu tiên tới row cuối cùng ~ index(N+1) = index(N)+2
```

+ Nếu lấy theo Label

```
df.loc[start:stop:step] #Lay ra phan tu dua tren i
```
Với ý nghĩa của start; stop; step tương tự trên chỉ khác cái ở đây là label

Ví dụ:

```
df1 = df.loc['r2']              # Chọn row có label là r2
df1 = df.loc[['r2','r3','r6']]  # Chọn các row có label nằm trong list r2,r3,r6
df1 = df.loc['r1':'r5']         # Chọn các row nằm trong khoảng index label từ r1->r5 (Nếu r5 < r1 thì kết quả là rỗng)
df1 = df.loc['r1':'r5':2]       # Chọn các row nằm trong khoảng index label từ r1->r5 với bước nhảy là 2 (2 là bước nhảy của index không phải label vì label là đoạn text thôi)
```

Đoạn này sample là label, df không có nên chạy thử đoạn code để ae cho dễ hình dung

![read label]( {{site.url}}/assets/img/2023/12/22/03_pandas_label.png){:width="900px"}

### Lấy ra row dựa trên điều kiện nhất định

Nhiều khi ta cần lấy ra các row có dựa trên một số điều kiện nhất định

+ Chọn Rows dựa trên giá trị của một cột

```
df[df["author"] == 'Arpit']        #Chọn row có giá trị author ='Arpit'
df.query("author == 'Arpit'")      #Chọn row có giá trị author ='Arpit'
df.loc[df['author'] == 'Arpit']    #Chọn row có giá trị author ='Arpit'
df.loc[df['author'] != 'Arpit']    #Chọn row có giá trị author != 'Arpit'

values = ['Arpit','Purnima']
df.loc[df['author'].isin(values)]  #Chọn row có giá trị nằm trong một list
df.loc[~df['author'].isin(values)] #Chọn row có giá trị nằm không nằm trong một list
```

+ Chọn Rows dựa trên điều kiện của nhiều cột

```
df1=df.loc[(df['article'] >= 114) & (df['article'] <= 178)]
```

+ Select columns that have no None & nana values

```
df1 = df.dropna()
```

+ Một số phương pháp chọn khác kết hợp nhiều method khác nhau

```
df1=df[df['author'].str.contains("Arp")]             #Chọn row sử dụng contain
df1=df[df['author'].str.lower().str.contains("Arp")] #Chọn row kết hợp nhiều method
df1=df[df['author'].str.startswith("A")]               #Chọn row bắt đầu bằng P
```

## Thao tác với Cột/Column hay chính là thao tác với Series

+ Chọn ra column

Sử dụng slice

```
selected_column = ["article"] #Danh sách các cột được chọn
df1 = df[selected_column] 
```
Một cách khác sử dụng loc và iloc

Việc sử dụng loc và iloc tương tự như trường hợp chọn ra row, Chỉ đảo chút ở vị trí tham số

```
df1 = df.loc[:,start:stop:step]  #Nếu dùng loc lọc theo label
df1 = df.iloc[:,start:stop:step] #Nếu dùng iloc lọc theo chỉ số
```
Trong đó **start**: index/label bắt đầu lấy; **stop**: index/label kết thúc lấy; **step**: Bước nhảy của giá trị được lấy

Một vài ví dụ với log và iloc

```
#Sử dụng loc ~ Sử dụng label
df1 = df.loc[:, ["article"]]        #Chọn một vài cột
df1 = df.loc[:,'author':'article']  # Chọn các cột giữa 2 cột
df1 = df.loc[:,'author':]           #Chọn toàn bộ cột bên phải author (bao gồm author)
df1 = df.loc[:,:'author']           #Chọn toàn bộ cột bên trái author
df1 = df.loc[:,::2]                 #Chọn toàn bộ các cột với bước nhảy index là 2 (Index bắt đầu từ 0)

#Sử dụng iloc ~ Sử dụng chỉ sổ
df1 = df.iloc[:,[1,3,4]]    #Chọn cột chỉ số là 1
df1 = df.iloc[:,0:1]        #Chọn cột khoảng từ 0 đến <1
df1 = df.iloc[:,2:]         #Chọn từ 1 -> hết
df1 = df.iloc[:,:2]         #Chọn 2 cột đầu
```


## Xử lý dữ liệu từng dòng với phương thức apply()

Đối với mình apply() mà một trong những phương thức hay sử dụng nhất vì kiểu gì không ít thì nhiều mình cũng phải thao tác lên dữ liệu. Điểm hay ho là apply() hỗ trợ cả thao tác lên dữ liệu theo dòng hoặc cột chỉ cần dựa trên việc thay đổi tham số axist

+ Apply cho Dòng/axist ⇒ Đối số axis = 1

```
def testFunct(row):
    print("==Dang xu ly du lieu dong: ")
    print(row)
    row['article'] = row['article'] + 10 #Sua doi du lieu truc tiep vao row 
    return row #Tra lai row sau khi xu ly

## Lay ra user khong login trong 3 thang gan nhat
df_result = df.apply(testFunct, axis=1)
```

Chạy thử

![loop row]( {{site.url}}/assets/img/2023/12/22/04_pandas_loop_row.png){:width="900px"}


+ Apply cho cột/Series ⇒ Đối số axis = 0

```
def testFunct(coln):
    print("==Dang xu ly du lieu cot: ")
    print(coln.name) #Truy cap in ra ten cot. Muon biet Colum/Series có attribute nao thi xin moi ae google
    return coln

## Lay ra user khong login trong 3 thang gan nhat
df_result = df.apply(testFunct, axis=0)
```

Chạy thử

![loop Column]( {{site.url}}/assets/img/2023/12/22/04_pandas_loop_column.png){:width="900px"}

**Note:** Chú ý chút chỗ truy cập row element và column element có chút khác nhau. Một bên truy cập như list một bên lại truy cập attribute (Cái này cũng dễ hiểu thôi một bên là list một bên là Object)

