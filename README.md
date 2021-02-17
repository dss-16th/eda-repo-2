# 제주도 재난지원금 데이터를 통한 소비 분석

#### __EDA PROJECT__

****
##### 기간 : 2021.02.02-2021.02.18
##### * 인원 : 2명
          * 정민주 : 주제설정, PPT 작성, 전처리 및 시각화(7,8월 data)
          * 이주영 : 주재설정, readme 작성, 전처리 및 시각화(5,6월 data), 변수 변환(time_zone, classification)
##### reference
Yenabeam (2020.09.01). 제주도 사용금액 데이터를 통한 소비행태 및 재난지원금 효과 분석.
URL: [GitHubBlog](https://github.com/Yenabeam/JejuEda_DACON)
진순현 (2020.08.24). 제주관광 ‘붕괴’...‘산업위기대응 특별지역’ 지정 촉구. <제주도민일보>. 
URL:[기사](https://www.jejudomin.co.kr/news/articleView.html?idxno=127679)
김차경 (2020.12.23). 위기에서 도약으로… 코로나19 극복을 위한 우리의 노력[2020, 위기를 넘어 희망을 쓰다] ④ 코로나19 극복 경제지원. <정책뉴스>. URL:[기사](https://www.korea.kr/news/policyNewsView.do?newsId=148881628)

[출처] 대한민국 정책브리핑(www.korea.kr)
****

## 1. Intro

#### 1-1. Intro
* 재난지원금
: 코로나19로 인한 소비심리 위축 등으로 경제적 타격을 입은 국민들의 생계와 소득을 보장하기 위해 정부에서 시행한 현금 지원 대책을 말한다. 
* 제주도는 신종 코로나바이러스 감염증 불안감 확산으로 관광시장 등 경기 침체를 겪고있어, 국가균형발전 특별법상의 '산업위기 대응 특별지역' 건의가 검토되고 있다.
* 따라서 제주도라는 공간정보를 바탕으로 제주도 내에서 재난지원금의 소비행태를 파악하여 재난지원금이 목적에 맞게 잘 사용되고 있는지를 확인해 보고자 한다.


#### 1-2. purpose
* 제주도의 공간 정보를 활용해 재난지원금이 제주도의 소상공인과 서민들의 가계 경제에 어떤 영향을 끼치는지 확인하고자 한다.


#### 1-3. Goals
* 제주도의 전반적 소비와 재난지원금 사용의 비교분석

    

#### 1-4. Dataset
##### [공간정보 탐색적 데이터 분석 경진대회](https://dacon.io/competitions/official/235682/data/)
* data.zip(27.8MB)
    * KRI-DAC_Jeju_data5.txt(37MB)
    * KRI-DAC_Jeju_data6.txt(38MB)
    * KRI-DAC_Jeju_data7.txt(41.6MB)
    * KRI-DAC_Jeju_data8.txt(37.4MB)


#### 1-5. Roles
* 정민주 : 주제설정, PPT 작성, 전처리 및 시각화(7,8월 data)
* 이주영 : 주재설정, readme 작성, 전처리 및 시각화(5,6월 data), 변수 변환(time_zone, classification)



## 2. Result : 완성된 리스트


## 3. Proess

#### 3-1. Variables

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

#### 3-3. Process

##### 1. 모듈설정
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
##### 2. 저장된 데이터 불러오기
```
raw_data_6 = pd.read_csv('./data/KRI-DAC_Jeju_data6.txt', sep=',')
raw_data_6.tail(2)
```
##### 3. 데이터 다른 변수로 선언 및 결측치 확인
```
df_6 = raw_data_6.copy()
msno.matrix(df_6)
```

##### 1. 모듈설정
##### 1. 모듈설정
##### 1. 모듈설정




## 4. Conclusion

