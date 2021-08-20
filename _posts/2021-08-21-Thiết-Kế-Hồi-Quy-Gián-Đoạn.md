---
layout: single
title: Thiết Kế Hồi Quy Gián Đoạn
excerpt: "60 GIÂY NHÂN QUẢ - KỲ 6"
toc: false
categories:
  - series 60 giây nhân quả
tags:
  - suy luận nhân quả
permalink: /causalgraph/ac06
sidebar:
    nav: causalgraph
---

**60 GIÂY NHÂN QUẢ - KỲ 6**

*Bài viết này thuộc chuỗi bài viết về “60 Giây Nhân quả”. Hãy cùng đọc thêm các bài viết có cùng chủ đề [tại đây](http://kinhtehocvohai.com/causalgraph/)*

*Để hiểu thêm kiến thức về Suy luận Nhân quả, hãy cùng tìm đọc chuỗi bài viết về “Suy luận Nhân quả với Python” [tại đây](http://kinhtehocvohai.com/pythoncausal/)*


-------

Chúng ta xem xét một can thiệp chính sách có khả năng tác động lên biến kết quả Y và muốn đánh giá độ lớn của tác động đó. Trong điều kiện lý tưởng, chúng ta chỉ cần nhìn vào mối tương quan giữa can thiệp và Y từ dữ liệu. Nhưng có nhiều lý do cản trở việc này. Ví dụ có một số nhóm người có biến kết quả Y cao hoặc thấp hơn người khác dù có tiếp nhận can thiệp của chính sách hay không. Thực tế trong trường hợp này, can thiệp được chỉ định dựa vào một Biến Chạy (X). Nếu giá trị của biến chạy vượt ngưỡng nhất định, người đó được chỉ định nhận can thiệp và không nếu thấp hơn. Tuy nhiên bản thân Biến Chạy có khả năng giải thích Y như biểu diễn trong biểu đồ dưới đây:

![image-center](/assets/images/animatedcausality/ac06/output_6_0.svg){: .align-center}


Ví dụ, Can thiệp là việc được nhận vào trường chuyên, Y là việc được nhận vào trường đại học và biến chạy là điểm thi vào trường chuyên. Thí sinh có điểm thi vượt 90 điểm (trên thang 100) được nhận vào trường chuyên và thí sinh dưới 90 không được nhận. Việc học trường chuyên có khả năng hỗ trợ bạn thi đại học tốt hơn nhưng học sinh có điểm thi vào trường chuyên cao vẫn có khả năng đỗ đại học cao hơn (dù các em có học trường chuyên hay không).

Khi điều này xảy ra, mối quan hệ giữa biến can thiệp và Y phản ánh 2 điều: tác động của Can thiệp lên Y (chúng ta quan tâm) và tác động chung của Biến Chạy lên Can thiệp và Y (chúng ta không quan tâm). Biến chạy mở ra lối đi cửa sau từ Can thiệp lên Y. Chúng ta có thể đi từ Can thiệp đến Y qua con đường trực tiếp từ Can thiệp → Y hoặc Can thiệp ← Vượt ngưỡng ← Biến chạy → Y.

Chúng ta có thể đóng lại cửa say bằng cách kiểm soát Biến Chạy. Cách tốt nhất để làm việc này là xem xét Biến chạy giải thích Y như thế nào rồi tập trung vào các điểm dữ liệu ngay xung quanh "ngưỡng". Điều này đảm bảo chúng ta thực sự so sánh những người nhận can thiệp và không nhận can thiệp có giá trị Biến Chạy xấp xỉ nhau, hay nói cách khác, chúng ta thực sự kiểm soát "Biến chạy".

Tại sao cần so sánh các điểm dữ liệu ngay xung quanh ngưỡng mà không trực tiếp kiểm soát biến chạy theo cách thông thường? Bởi vì trong số những người nằm ở ngay gần ngưỡng, việc họ nằm ngay trên hoặc ngay dưới ngưỡng gần giống việc được chỉ định ngẫu nhiên. Và chinh sự gần giống ngẫu nhiên này đóng lại các lối đi của sau nếu có cả khi chúng ta không nhận ra chúng. Ví dụ nếu biểu đồ nhân quả thực ra trông như sau:

![image-center](/assets/images/animatedcausality/ac06/output_8_0.svg){: .align-center}

Trong tình huống này, bằng việc tập trung vào khu vực ngay xung quanh ngưỡng, chúng ta có thể cô lập con đường Vượt ngưỡng → Tác động → Y và có thể bỏ qua con đường cửa sau Tác động ← W → Y.

Biểu đồ dưới đây giải thích cách thực hiện phương pháp này.

![image-center](/assets/images/animatedcausality/ac06/IV.gif){: .align-center}

Lưu ý nếu Biến Chạy là thời gian thì nó được gọi là Chuỗi Thời gian gián đoạn hoặc Nghiên cứu Sự kiện. Tất nhiên, trong các nghiên cứu này việc giới hạn lân cận ngưỡng không thực sự đóng lại hoàn toàn con đường Tác động ← Vượt ngưỡng ← Biến chạy → Y, vì thời gian trước và sau "ngưỡng" là khác nhau và nhiều sự vật, sự việc khác đã thay đổi!



## Tạo lập dữ liệu


```python

pip install linearmodels
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np

#Set size
size=200
np.random.seed(0)

# Tạo biến chạy
X=np.random.uniform(-1,1,size=size)

#Tạo lập biến kết quả
Y = .2*X+1.5*(X>0)-.1*X*(X>0)+np.random.standard_normal(size)

#Tạo lập khung dữ liệu
df=pd.DataFrame({'X':X,'Y':Y})

#Phân nhóm dữ liệu
df['g']=df.X.map(lambda x: int((x+1)/0.2))


```
