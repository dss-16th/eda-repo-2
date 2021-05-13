# 제주도 재난지원금 데이터를 통한 소비 분석

#### __EDA PROJECT__


<br />
<br />

## 1. Intro



#### 1-1. Intro
* 재난지원금
: 코로나19로 인한 소비심리 위축 등으로 경제적 타격을 입은 국민들의 생계와 소득을 보장하기 위해 정부에서 시행한 현금 지원 대책을 말한다. 
* 제주도는 신종 코로나바이러스 감염증 불안감 확산으로 관광시장 등 경기 침체를 겪고있어, 국가균형발전 특별법상의 '산업위기 대응 특별지역' 건의가 검토되고 있다.
* 따라서 제주도라는 공간정보를 바탕으로 제주도 내에서 재난지원금의 소비행태를 파악하여 재난지원금이 목적에 맞게 잘 사용되고 있는지를 확인해 보고자 한다.

<br />

#### 1-2. purpose
- 제주도의 공간 정보를 활용해 재난지원금이 제주도의 소상공인과 서민들의 가계 경제에 어떤 영향을 끼치는지 확인하고자 한다.
- 제주도의 전반적 소비와 재난지원금 사용의 비교분석

<br />

#### 1-3. Conclusion


 <img src="https://user-images.githubusercontent.com/75352728/108820102-0a02d480-75ff-11eb-9e76-ebfd594a1493.PNG" width="80%" height="80%">


- 재난 지원금은 5월에 가장 많이 사용한다.
- 시군구 월별 사용금액과 소상공인 사용금액에서 차이가 없었으며, 시간별 또한 동일하게 나타났다. 
- 상위 5개 업종별 재난지원금 이용 건수 또한 차이가 없었다.
- 총 사용 금액에서 보았을 때 제주도민이 많이 사용 했던 상위 5개와 재난지원금 사용 금액 상위 5개는 거의 동일 -> 재난 지원금이 도민의 가계에 도움이 되고 있다고 보인다!
- 기사에서 보았던 재난지원금의 남용은 대부분의 사람에게 해당되는 일이 아니다!!
- 재난지원금의 취지와 알맞게 재난지원금은 도민의 가계에 도움이 된다.

<br />

#### 1-4. comment & limitations

- 추후 좌표를 이용한 읍면리를 통해 재난 지원금 사용 금액 추정 예정
- 재난지원금은 각 시도별 주민만 사용하는 것으로 제주도 또한 제주도민만 사용 할 수 있어 관광이 주 사업이 제주도의 지역에 도움이 되는지 알기 어렵다.

<br />
<br />

****

#### 1-5. 인원
- 기간 : 2021.02.02-2021.02.18
- 인원 : 2명
- 정민주 : 주제설정, readme 작성(구조 등), 전처리 및 시각화(7,8월 data), PPT 작성
    - GitHub address : [https://github.com/meiren13](https://github.com/meiren13)
- 이주영 : 주제설정, readme 작성(코드, 시각화 자료등), 전처리 및 시각화(5,6월 data), 변수 변환(classification), 발표, 코드 
    - GitHub address : [https://github.com/leekj3133](https://github.com/leekj3133)


<br />
<br />


#### 1-5. Dataset

##### [공간정보 탐색적 데이터 분석 경진대회](https://dacon.io/competitions/official/235682/data/)

* data.zip(27.8MB)
    * KRI-DAC_Jeju_data5.txt(37MB)
    * KRI-DAC_Jeju_data6.txt(38MB)
    * KRI-DAC_Jeju_data7.txt(41.6MB)
    * KRI-DAC_Jeju_data8.txt(37.4MB)


****

<br />


 
<br />

****

<br />

<br />

## 3. Proess

<br />

### 3-1. Variables Setting


<br />

#### 1. Variables

<br />

* 데이터 정의
    * YM : 기준년월
    * SIDO : 지역대분류명
    * SIGUNGU : 지역중분류명
    * FranClass : 소상공인구분
    * Type : 업종명
    * Time : 시간대
    * TotalSpent : 총사용금액
    * DisSpent : 재난지원금 사용금액
    * NumOfSpent : 총 이용건수
    * NumOfDisSpent : 총 재난지원금 이용건수
    * POINT_X, POINT_Y : X,Y 좌표
   
<br />

#### 3-2. Details
 1. 제주도 소비 전반적 시각화
    * 기간별/업종별 시각화
    * 월별 소비 상위 5개 업종 분석(이용건수 기준)
    * 시간별 소비 상위 5개 업종분석
    * 업종별 시간대 사용금액(이용건수) 추이분석
 2. 재난지원금 분석 - 기간별
 3. 재난지원금 분석 - 업종별
 4. 재난지원금 - 소상공인 구분 
    * 재난지원금 어떤 규모의 소상공인에게 소비 활성화


<br />

### 3-3. Process

<br />

#### 1. 데이터 전처리

<br />

#### 1-1. 모듈설정

<br />

```
%config InlineBackend.figure_format = 'retina'
%matplotlib inline
# 전처리  
import numpy as np
import pandas  as pd
# 위도 경도 바꿔줌 
from pyproj import CRS
from pyproj import Proj
from pyproj import Transformer
import geopandas
# 시각화
import missingno as msno
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np
# url 불러옴 
import requests; from urllib.parse import urlparse
import json
import urllib

# 폰트 설정
from matplotlib import font_manager
from matplotlib import rc
plt.rcParams['axes.unicode_minus'] = False
f_path= "C:/Windows/Fonts/malgun.ttf"
font_name= font_manager.FontProperties(fname=f_path).get_name()
rc('font', family =font_name)
plt.rc('font', family='Malgun Gothic')
```

<br />

#### 1-2. 저장된 데이터 불러오기

<br />

```
raw_data_6 = pd.read_csv('./data/KRI-DAC_Jeju_data6.txt', sep=',')
raw_data_6.tail(2)
```
<br />

#### 1-3. 데이터 다른 변수로 선언 및 결측치 확인

<br />

```
df_6 = raw_data_6.copy()
msno.matrix(df_6)
```
 <img src="https://user-images.githubusercontent.com/75352728/108266471-79b33280-71ad-11eb-916c-1910877b178b.png" width="80%" height="80%">


##### 결측치가 없는 완벽한 데이터!!


<br />

#### 1-4. 시간 -> 시간대로 변경(무승인 거래(별도 승인 없이 결제되는 건(SMS자동결제, 기내 면세점 등))


<br />

```
# int 와 if function하기전 불필요한 '시' 제거
df_5['Time'] = df_5['Time'].str.replace('시','')
df_5.head(1)
# 새벽 2-6 오전 6-11 점심 11-15 오후 15-18  저녁 18-22 심야 22-02 무승인거래 
# 무승인 거래(별도 승인 없이 결제되는 건(SMS자동결제, 기내 면세점 등))

# 함수 생성
def time_zone(time):
    if '02' <= time <'06':
        return '새벽'
    elif '06' <= time <'11':
        return '오전'
    elif '11' <= time <'15':
        return '점심'
    elif '15' <= time <'18':
        return '오후'
    elif '18' <= time <'22':
        return '저녁'
    else:
        return '심야'

# 함수를 이용해서 시간대로 변경
df_5['time_zone'] = df_5['Time'].transform(time_zone)
df_5['time_zone'] = df_5['Time'].str.replace('x','무승인거래')
```
 <img src="https://user-images.githubusercontent.com/75352728/108269537-7ae65e80-71b1-11eb-8287-bb9d6951e81a.PNG" width="70%" height="70%">
 
<br />

#### 1-5. 업종 분류

<br />

##### * 업종은 약 200여개로 같은 항목끼리 연결하여 새로운 컬럼을 생성
##### * 약 13개의 컬럼으로 묶어준 후 새로운 데이터 프레임으로 생성


<br />

```
df_5_1 =  df_5[['Type']]

df_5_1.replace(dict.fromkeys({'외국어학원', '보습학원', '유아원', '기능학원', '기타교육', '독서실', '학원(회원제형태)', '초중고교육기관', '대학등록금', '컴퓨터학원', '문구용품', '기타서적문구', '학습지교육', '예체능학원','완구점', '전문서적', '출판인쇄물', '서적출판(회원제형태)', '산후조리원', '과학기자재'}, '교육/학원'), inplace=True)
df_5_1.replace(dict.fromkeys({'노래방', '문화취미기타', '볼링장', '티켓', '영화관', '상품권','악기점', '일반서적', '화랑', '수족관'}, '문화/오락'), inplace=True)
df_5_1.replace(dict.fromkeys({'약국', '의원', '종합병원', '의료용품', '기타의료기관및기기', '한의원', '한약방', '치과의원', '치과병원', '병원', '제약회사', '건강진단'}, '의료'), inplace=True)
df_5_1.replace(dict.fromkeys({'피부미용실', '안마스포츠마사지','미용원' '화장품', '이용원', '미용재료'}, '미용'),inplace=True)
df_5_1.replace(dict.fromkeys({'인터넷종합Mall', '악세사리', '기타잡화', '면세점', '성인용품점', '가전제품', '스포츠의류', '정장', '가방', '기타가구', '옷감직물', '카메라', '양품점', '시계', '안경', '화방표구점', '소프트웨어', '인터넷Mall', 'DVD음반테이프판매', '기념품점', '민예공예품', '골동품점', '신발', '기타의류', '단체복', '아동의류', '컴퓨터', '기타사무용', '맞춤복점', '귀금속', '캐쥬얼의류', '제화점', 'CATV', '사무기기'}, '쇼핑'), inplace=True)
df_5_1.replace(dict.fromkeys({'편의점', '대형할인점', '슈퍼마켓', '주류판매점', '제과점', '농축수산품', '농협하나로클럽', '정육점', '구내매점','스넥', '기타음료식품', '기타건강식', '연쇄점', '인삼제품', '홍삼제품'}, '식료품'), inplace=True)
df_5_1.replace(dict.fromkeys({'콘도', '특급호텔', '2급호텔', '기타숙박업', '1급호텔', '항공사', '관광여행'}, '여행/숙박'),inplace=True)
df_5_1.replace(dict.fromkeys({'기계공구', '기타건축자재', '건축요업품','유리', '목재석재철물', '인테리어', '조명기구', '냉열기기', '보일러펌프', '페인트', '철제가구', '일반가구', '침구수예점', '기타연료', '기타광학품', '기타업종'}, '건축/기타'), inplace=True)
df_5_1.replace(dict.fromkeys({'단란주점', '주점','유흥주점','기타회원제형태업소', '칵테일바'}, '유흥/주점'),inplace=True)
df_5_1.replace(dict.fromkeys({'골프경기장', '헬스크럽', '기타레져업', '당구장', '레져업소(회원제형태)', '수영장', '테니스장', '기타대인서비스', '스포츠레져용품', '골프용품', '레져용품수리', '골프연습장', '종합레져타운'}, '레저/스포츠'), inplace=True)
df_5_1.replace(dict.fromkeys({'농축협직영매장', '비료농약사료종자', '미곡상', '농기계'}, '농업'), inplace=True)
df_5_1.replace(dict.fromkeys({'사우나','세탁소', '공공요금', '위탁급식업', '애완동물', '동물병원', '정수기', '기타전기제품', '주방용구', '카페트커텐천막', '기타직물', '내의판매점', '주방용식기'},'생활/인테리어'), inplace=True)
df_5_1.replace(dict.fromkeys({'주차장', '주유소', '렌트카', '기타자동차서비스', '자동차부품', '견인서비스', '자동차정비', 'LPG', '세차장', '자동차시트타이어', '택시', '중고자동차', '수입자동차', '유류판매', '카인테리어', '기타교통수단', '이륜차판매', '윤활유전문판매'}, '교통/자동차'), inplace=True)
df_5_1.replace(dict.fromkeys({'화원', '화물운송', '사진관', '보관창고업','사무서비스', '가례서비스', '기타대인서비스', '기타수리서비스', '법률회계서비스', '사무서비스(회원제형태)','조세서비스',  '기타용역서비스', '부동산분양', '기타유통업', '종합용역', '기타운송', '사무통신기기수리', '가정용품수리', '중장비수리', '부동산중개임대', '신변잡화수리', '손해보험', '정기간행물', '건강식품(회원제형태)','기타보험', '손해보험', '기타비영리유통', '통신기기'}, '서비스'), inplace=True)
df_5_1.replace(dict.fromkeys({'일반한식', '서양음식', '일식회집', '중국음식'}, '외식'), inplace=True)
```
<br />



 <img src="https://user-images.githubusercontent.com/75352728/108270244-8b4b0900-71b2-11eb-8bdf-b338f21d87e7.PNG" width="20%" height="10%">
 
<br />

##### 새로운 데이터 프레임 셍성!
##### 원래 데이터 프레임에 데이터 병합 필요.



<br />

#### 1-6. 데이터 병합

<br />

```
df_5 = pd.merge(df_5,df_5_1,right_index=True,left_index=True)
df_5.head(2)
# 컬럼 명 바꾸기
df_5 = df_5.rename(columns = {'Type_x':'Type','Type_y':'classification'})
df_5.head()
```
<br />

#### 1-7. 데이터 csv파일로 저장

<br />

```
df_5.to_csv('./data/df_5.csv', sep=',', encoding='euc-kr')
```

<br />
<br />
<br />

#### 2. 시각화

<br />

****


<br />

#### 2-1. 재난지원금 사용 비율 비교

<br />

```
df_5 = raw_data_5
sigu_5 = df_5.groupby('SIGUNGU').sum()
```
<br />

 <img src="https://user-images.githubusercontent.com/75352728/108288368-d32c5900-71cf-11eb-9596-666790f7c55b.PNG" width="50%" height="50%">

<br />

##### 결과값을 대입해 주면 됨

<br />

```
# 5월
ratio = [168687712199,24180094624]
labels = ['TotalSpent','DisSpent']
# 8월
ratio = [51576733826,150977596]
labels = ['TotalSpent','DisSpent']

```
<br />

 <img src="https://user-images.githubusercontent.com/75352728/108288246-9d877000-71cf-11eb-8bec-7cfc33bcf793.PNG" width="70%" height="70%">

<br />

##### 5월과 8월을 비교했을 때 5월에 비해 8월이 사용량이 적은 걸 볼 수 있음

<br / >

#### 왜 이런 현상이?

##### 역시 돈을 받으면 빨리 써야지! 
##### 5월에 대부분 사용해서 8월에는 사용량이 적다!!

<br />

*****

<br />

#### 2-2. 월별 총 사용금액 비교

<br />

*****

<br />

##### 1. 월별 시군별 총 사용금액

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108288419-e5a69280-71cf-11eb-91bb-111edb6c8c93.PNG" width="70%" height="70%">

<br />

##### 제주시 서귀포시에 총 사용금액은 월과 상관이 없다!

<br />

*****

<br />

##### 2. 월별 시군별 재난지원금 사용 금액

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108288422-e63f2900-71cf-11eb-9fde-2d4bbb9a2941.PNG" width="70%" height="70%">

<br />

##### 제주시 서귀포시에 재난지원금 사용금액 역시 월과 상관이 없다!

<br />

*****

<br />

##### 3. 월별 상위 5개 업종별 총 이용 건수

<br />

```
jeju_type_6 = df_6.groupby(['classification'], as_index=False).mean()
norm_jeju_type_6 = jeju_type_6.copy()
col = ['NumofSpent', 'NumofDisSpent']
col
num_jeju_type_6 = (jeju_type_6[col]) / (jeju_type_6[col].max())
norm_jeju_type_6 = jeju_type_6.copy()
norm_jeju_type_6

sns.barplot(x='classification', y='NumofSpent', data=norm_jeju_type_6.nlargest(5, 'NumofSpent'), palette='coolwarm').set_title('6월 상위 5개 업종별 총 이용 건수')
sns.barplot(x='classification', y='NumofDisSpent', data=norm_jeju_type_6.nlargest(5, 'NumofDisSpent'), palette='coolwarm').set_title('6월 상위 5개 업종별 재난지원금 이용 건수')
```

<br />

##### 월별 상위 5개 업종별 총 이용 건수 비교

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108289441-bf81f200-71d1-11eb-8983-4c4a43a6bbb1.PNG" width="70%" height="70%">
 
<br />

##### * 총 이용 건수에는 공통적으로 농업, 식료품, 의료, 교통/자동차, 쇼핑 등에 사용함.
##### 과일 재배가 많아서 농업에서 높게 나타나나?




##### 월별 상위 5개 업종별 재난지원금 이용 건수 비교

<br />



 <img src="https://user-images.githubusercontent.com/75352728/108289447-c446a600-71d1-11eb-8f12-c531e0c62b0a.PNG" width="70%" height="70%">
 
<br />

##### * 재난지원금 이용 건수에는 공통적으로 농업, 의료, 식료품, 기타농업관련, 교통/자동차 등에 사용함.


##### * 제주도민의 재난지원금 사용은 도민들이 총 사용 하는 것과 거의 일치하다.
##### * 정부의 정책인 재난지원금이 도민들의 경제 생활 안정을 위해 잘 사용 되고 있다!

<br />

*****

<br />

#### 2-3. 소상공인 재난지원금액 비교

<br />

##### 월별로 소상공인 총 사용 금액 비교


<br />

 <img src="https://user-images.githubusercontent.com/75352728/108290525-f0fbbd00-71d3-11eb-8146-648551cc777b.PNG" width="70%" height="70%">

 <br />
 
##### * 5,6 월 : 일반 > 중소2 > 중소1 > 중소 > 영세 순
##### * 7,8 월 : 일반 > 영세 > 중소2 > 중소1 > 중소 순

##### 월별로 소상공인 재난지원금 금액 비교

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108290528-f1945380-71d3-11eb-834e-e4082b960a65.PNG" width="70%" height="70%">
 
<br />

##### * 5,6 7,8 월 : 일반 > 영세 > 중소1 > 중소 , 중소2  순으로 볼 수 있다.

##### * 재난지원금은 월별로 소상공인의 사용 금액 차이가 거의 없다.

<br />

##### 비율을 비교

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108288424-e7705600-71cf-11eb-8879-fc3579a4bd4a.PNG" width="70%" height="70%">
 
<br />

##### 비율을 표로 보았을 때 각 소상공인의 사용 금액은 별다른 차이가 없다.

<br />

*****

<br />

#### 2-4. 월별 시간별 사용 금액 


 <img src="https://user-images.githubusercontent.com/75352728/108291500-b7c44c80-71d5-11eb-9a49-21f74c348870.PNG" width="70%" height="70%">
 
##### 왼쪽은 총 사용 금액으로 월별 새로 순으로, 오른쪽은 재난지원금 사용 금액으로 월별 새로 순 으로
##### 일반 사람의 생활 패턴과 총 사용 금액과 재난지원금 사용 금액이 비슷하게 나타난다.
##### 23시- 5시까지 제일 사용을 적게 한다.
##### 이 시간에 대부분 영업할 시간이 아니니 당연한 결과이지 않을까??

<br />

*****

<br />

#### 2-5. 월별 시간대별 사용 금액



<br />

 <img src="https://user-images.githubusercontent.com/75352728/108292145-da0a9a00-71d6-11eb-95d9-03015c2524c2.PNG" width="70%" height="70%">

<br />

##### 새벽, 심야 시간 대에 제일 사용이 적다.
##### 즉 23시 -5시에 제일 사용이 적다.


##### 시간대별 비율

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108292407-61580d80-71d7-11eb-8c3a-e57877699aa9.PNG" width="70%" height="70%">
 
<br />

##### * 시간대별 비율은 5,6,7,8 월 모두 심야, 새벽이 적은 비율로 나타난다.

<br />

*****

<br />

#### 2-6. 상위 5개 업종 사용 금액 비교

<br />

##### 1. 상위 5개 업종 총 사용 금액

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108819425-2fdba980-75fe-11eb-95dc-18f6a66d7a05.PNG" width="70%" height="70%">

<br />

##### * 월별 상위 5개 업종을 보았을 때 5,6,7,8월 모두 식료품, 외식, 쇼핑, 교통/자동차가 동일하게 보인다.
##### * 8월에 여행, 숙박 비용이 늘어난 것으로 보인다.

<br />

##### 1-1. 월별 외식 총 사용 금액 비교

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108819430-310cd680-75fe-11eb-9bde-29e11c6d2d37.PNG" width="70%" height="70%">

<br />


##### 1-2. 월별 쇼핑 총 사용 금액 비교

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108819434-323e0380-75fe-11eb-9a07-b62a8f3b0acd.PNG" width="70%" height="70%">

<br />


##### 1-3. 월별 교통/자동차 총 사용 금액 비교

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108819437-32d69a00-75fe-11eb-89e5-b51a5a1c0d88.PNG" width="70%" height="70%">
 
<br />


##### 1-4. 월별 의료 총 사용 금액 비교<br />


 <img src="https://user-images.githubusercontent.com/75352728/108819441-3407c700-75fe-11eb-899f-7ee00877762b.PNG" width="70%" height="70%">
 
<br />


##### 1-5. 월별 여행/숙박 총 사용 금액 비교

<br />



 <img src="https://user-images.githubusercontent.com/75352728/108819445-3538f400-75fe-11eb-894c-d0475a3e60cd.PNG" width="70%" height="70%">
 
<br />

##### * 여행/숙박을 제외한 5가지 업종이 시간별 비슷한 패턴을 보인다.




<br />

##### 2. 상위 5개 업종 재난지원금 사용 금액

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108820102-0a02d480-75ff-11eb-9e76-ebfd594a1493.PNG" width="70%" height="70%">
 
<br />

##### * 월별 상위 5개 업종을 보았을 때 5,6,7,8월 모두 순위는 조금 다르지만 식료품, 외식, 쇼핑, 교통/자동차, 농업이 동일하게 보인다.



<br />

##### 2-1. 월별 외식 총 사용 금액 비교

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108820108-0b340180-75ff-11eb-90b6-2f0fb726151b.PNG" width="70%" height="70%">
 

<br />

##### 2-2. 월별 쇼핑 총 사용 금액 비교

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108820856-20f5f680-7600-11eb-9f83-de341277cf05.PNG" width="70%" height="70%">

<br />

##### 2-3. 월별 교통/자동차 총 사용 금액 비교

<br />


 <img src="https://user-images.githubusercontent.com/75352728/108820122-125b0f80-75ff-11eb-92d4-dc0168770098.PNG" width="70%" height="70%">
 
<br />

##### 2-4. 월별 의료 총 사용 금액 비교

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108820127-138c3c80-75ff-11eb-943f-3053c6ceb6bc.PNG" width="70%" height="70%">
 
<br />



##### 2-5. 월별 여행/숙박 총 사용 금액 비교

<br />

 <img src="https://user-images.githubusercontent.com/75352728/108820130-1424d300-75ff-11eb-81cc-0820a424cb5d.PNG" width="70%" height="70%">
 
<br />

##### * 총 사용 금액에서 많이 사용 되는 업종이 재난지원금에서 또한 많이 사용되는 것으로 나타난다.
##### * 제주도민은 생활패턴과 일치하게 재난지원금을 잘 이용하고 있다!


<br /><br />

****

#### reference
- Yenabeam (2020.09.01). 제주도 사용금액 데이터를 통한 소비행태 및 재난지원금 효과 분석.
URL: [GitHubBlog](https://github.com/Yenabeam/JejuEda_DACON)
- 진순현 (2020.08.24). 제주관광 ‘붕괴’...‘산업위기대응 특별지역’ 지정 촉구. <제주도민일보>. 
URL:[기사](https://www.jejudomin.co.kr/news/articleView.html?idxno=127679)
- 김차경 (2020.12.23). 위기에서 도약으로… 코로나19 극복을 위한 우리의 노력[2020, 위기를 넘어 희망을 쓰다] ④ 코로나19 극복 경제지원. <정책뉴스>. URL:[기사](https://www.korea.kr/news/policyNewsView.do?newsId=148881628)

[출처] 대한민국 정책브리핑(www.korea.kr)
