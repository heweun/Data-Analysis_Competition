# Data-Analysis_Competition
- 분석 목적 : 졸업 후 부동산 거래를 하게 될 청년층의 상황 파악
- 분석 목표
  1. 로지스틱 회귀 분석을 통해 매매여부에 영향일 미치는 변수 파악
  2. 공간적 패턴이 있는지 파악

## 결론
1. 초기 9개의 변수에서 5개의 변수로 줄여 예측력 3% 증가시킴
2. 매매 가능한 아파트는 질이 떨어지고, 중심지와 거리가 멀다    
3. 현재 부동산의 흐름은 미래에 대한 기대감을 갖기 어려워 보임

<img src="https://user-images.githubusercontent.com/109562023/228751016-5d3fa10b-5868-4939-9cab-030dc34e2018.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

#### 제안
단순히 가격에만 집중된 정책이 아닌,    
거주공간의 질과 대출규제에 대한 새로운 고려가 필요하다.   

<img src="https://user-images.githubusercontent.com/109562023/228745753-ff02f6ad-4755-4f5f-96f3-a3f5deeafc45.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
    
--- 
## Lesson Learn
1. 최신 데이터 호출, 부동산 정책에 따라 전처리를 통해 현실적인 분석결과 도출
2. 이항변수의 비율이 비슷해 유의미한 분석 결과 도출
3. 중첩 분석을 통해 따로 분석하지 않고 한번에 분석 가능

---

## 분석 과정
Open API R dplyr ggplot Word Powerpoint
1. 공공데이터 API 활용 (2020.01~2020.10, 211,819개)
2. 부동산 정책 기반하여 불필요한 변수, 결측치 데이터 전처리
3. ggplot으로 데이터 시각화

### 데이터 파싱 날짜
- 22년 10월 29일 (2020년 1월~10월)

### 데이터 정보
|  |  변수명 | 설명 |
| --- | --- | --- |
|  | 지역코드 | 자치구별 법정동 코드 |
|  | 건축년도 | 아파트 건축년도 |
|  | 전용면적 | 아파트 면적 |
|  | 층 | 아파트 층 수 |
|독립변수  | 일자 | 아파트 거래일자 |
|  | 주소 | 아파트 주소 |
|  | 위도 |  |
|  | 경도 |  |
|  | 전세점수 | 전세 가능 점수 |
|종속변수| 매매여부 | 매매 가능여부 |

### 전처리
1. 지역코드, 법정동, 아파트를 ‘주소’변수로 합침. Google API를 이용해 위도, 경도 정보 추가
2. 년, 월, 일을 ‘날짜’변수로 합침. Date형태로 변경
3. LTV개념 도입을 통한 종속변수 선정
4. 버팀목 전세대출을 통한 변수 생성

<img src="https://user-images.githubusercontent.com/109562023/228749489-f98a3b3c-226d-4eed-b39c-32e72f994495.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
<img src="https://user-images.githubusercontent.com/109562023/228749506-b44089a0-4921-45e2-80a6-000d0ccd23d1.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
<img src="https://user-images.githubusercontent.com/109562023/228749528-c4a261ec-6dba-46d9-8164-37567ace98c6.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
<img src="https://user-images.githubusercontent.com/109562023/228749579-2e852d40-d92f-4583-af67-c8b4c4b3e1fe.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

### 분석 방법
- EDA를 통한 데이터 분석
- 잔차 패턴을 분석한 공간 분석
- 로지스틱 회귀 분석

#### 1. 중첩 분석
<img src="https://user-images.githubusercontent.com/109562023/228749752-7595b364-ed15-4591-b7c9-555cb12e62be.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

###### 전세와 매매 데이터 중첩 분석 결과 상당부분 동일하기 때문에 위도, 경도를 기준으로 데이터 합침

#### 2. EDA
<img src="https://user-images.githubusercontent.com/109562023/228750149-9da2e1cb-db93-4a15-bbd9-8adc6f7dc508.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

###### 거래가 최근일 수록 매매 불가능한 아파트가 더 많아짐

#### 3. 1차 로지스틱 회귀분석
<img src="https://user-images.githubusercontent.com/109562023/228750466-b59124dd-4e97-4b8c-a8c1-dcb3a1e9c926.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

#### 4. 군집분석
<img src="https://user-images.githubusercontent.com/109562023/228750530-0f7d0ad8-540d-4c1e-8ffa-047618540ae3.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
<img src="https://user-images.githubusercontent.com/109562023/228750635-d106e303-e0ea-42a7-a626-a7f9753bbd94.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

###### 군집에 따라 공간상에 나타낸 결과가 1차 로지스틱 회귀분석 공간상 잔차 패턴과 유사하다.

#### 5. 2차 로지스틱 회귀 분석
<img src="https://user-images.githubusercontent.com/109562023/228750832-cb9f514b-c5b5-4ecc-8da3-32b0b48f9514.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

###### 군집을 명목형 변수로 입력해 다시 적용
