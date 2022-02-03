# Dacon JobCare Recommend algorithm compitition

## 주제 : 유저의 데이터를 활용한 채용 공고 추천 알고리즘

## 배경
잡케어는 일자리를 탐색하는 구직자에게 구직자의 이력서를 인공지능 기술로 직무역량을 자동 분석하여 훈련, 자격, 일자리 상담에 활용할 수 있도록 지원하는 시스템이다.
한국고용정보원은 구인구직 빅데이터 기반으로 커리어 관리 서비스인 잡케어 서비스를 구축하고 있다. 
본 대회를 통해 데이터를 기반으로 개인별 맞춤형 컨텐츠 추천 모델을 만들고자 한다.

## 평가지표 : F1-Score

## 분석 도구 : Python

## 분석 기법
* Logistic Regression
* LightGBM
* CatBoost

## EDA


## 분석 및 분석결과
### 1) Logistic Regression
* Pre-Processing
  * Code D, Code H, Code L 레프트 조인
  * Code D, H, L의 코드, 대분류, 중분류, 소분류, 세분류 매치 여부 컬럼 생성
  * E code 매치 여부 컬럼 생성
  * E code 차이의 절댓값 컬럼 생성
  * 각 콘텐츠 속성 코드의 target이 1일 확률 컬럼 생성
  * 콘텐츠 열람 일시 : 월/일/시 각 컬럼 생성 후 삭제
  * 콘텐츠 속성 코드 컬럼 삭제
  * ID, person_rn, contents_rn 컬럼 삭제
  * 값이 1개뿐인 컬럼 삭제 : person_prefer_g, person_prefer_f
  * Dimention Reduction
    * Stepwise Logistic Regression
    * Permutation Importance
* 분석
  * Logistic Regression
    * F1-Score : 0.592827619098087
  * Stepwise Logistic Regression
    * F1-Score : 0.6083703316792897
  * Stepwise Logistic Regression + Permutation Importance
    * F1-Score : 0.6103575190854587
### 2) LightGBM
* Pre-Processing
  * Code D, Code H, Code L 레프트 조인
  * Code D, H, L의 코드, 대분류, 중분류, 소분류, 세분류 매치 여부 컬럼 생성
  * E code 매치 여부 컬럼 생성
  * E code 차이의 절댓값 컬럼 생성
  * 콘텐츠 열람 일시 : 월/일/시 각 컬럼 생성 후 삭제
  * ID, person_rn, contents_rn 컬럼 삭제
  * 값이 1개뿐인 컬럼 삭제 : person_prefer_g, person_prefer_f
  * Dummy Feature 변환 : (2 < unique 값 < 10)인 컬럼만
  * Dimention Reduction
    * Permutation Importance
* 분석
  * LightGBM
    * F1-Score : 0.6352570908539361
  * LightGBM + Permutation Importance
    * F1-Score : 0.6340498416771478
  * LightGBM + Permutation Importance * Dummy Feature
    * F1-Score : 0.6339096148013346
  * LightGBM + Permutation Importance * Dummy Feature * Hyper Parameter Tuning
    * F1-Score : 0.6369016769999551
### 3) CatBoost
* Pre-Processing
  * Code D, Code H, Code L 레프트 조인
  * Code D, H, L의 코드, 대분류, 중분류, 소분류, 세분류 매치 여부 컬럼 생성
  * E code 매치 여부 컬럼 생성
  * E code 차이의 절댓값 컬럼 생성
  * 콘텐츠 열람 일시 : 월/일/시 각 컬럼 생성 후 삭제
  * ID, person_rn, contents_rn 컬럼 삭제
  * 값이 1개뿐인 컬럼 삭제 : person_prefer_g, person_prefer_f
  * Dimention Reduction
    * Permutation Importance
* 분석
  * CatBoost
    * F1-Score : 0.6429228645913203
  * CatBoost + Permutation Importance * Hyper Parameter Tuning
    * F1-Score : 0.670941337787087
### 결과
* F1-Score가 가장 높은 CatBoost 모델 선택
* train을 K-fold한 값의 평균을 구하다 보니 예측값의 극단값이 작아짐
  * 임계값을 조정하여 최적값 설정 : 0.379
* 대회 결과 : 약 70.2의 점수로 
