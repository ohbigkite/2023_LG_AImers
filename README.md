# 2023_LG_AImers 3기 (2023.07 ~ 2023.08) 👩🏻‍💻
## 1️⃣ Phase1. 온라인 AI 교육 
- 2023.07.01 ~ 2023.07.31

## 2️⃣ Phase2. 온라인 채널 제품 판매량 예측 AI 온라인 해커톤 
- 2023.08.01 ~ 2023.08.28
- https://dacon.io/competitions/official/236129/overview/description
- 결과 :
             Public  0.54059 80위 / 747
             Private 0.53146 61위/ 747 (상위 10% 이내) 
  ![image](https://github.com/ohbigkite/2023_LG_AImers/assets/122765534/77deacd6-abd3-4ab3-85ca-71c50239c65a)
- Domain : 시계열예측
- Task : 15890개의 상품의 판매량 예측 ( train data : 459일치 , test data : 21일치)

### 📂 Data

- train.csv : 상품별 대분류, 중분류, 소분류, 브랜드 코드, 판매량
- sales.csv : meta 정보 - 제품 판매금액 데이터
- brand_keyword_cnt.csv : meta 정보 - 브랜드 언급량 데이터
- product_info.csv : meta 정보 - 제품 특성 데이터
  
### 👩🏻‍💻 Model

- LSTM ⭐⭐
- Seq2Seq ⭐
- LGBM 

### 💡 Trials

- Multi Items Multivariate Timeseries Forecasting
- Window size 90일로, 90일의 과거 데이터를 이용해 추후 21일 예측하는 식으로 학습 진행
- 메타데이터 활용 → (1) 브랜드 언급량 feature 추가 (2) 브랜드 언급량 / 판매량 추이 데이터 이용해, timeseries k-means clustering 진행한 후, 몇번 유형의 군집인지 feature 추가해줌! 
- LSTM -> LSTM 이용 Seq2Seq 모델
- Seq2Seq window 90 < Seq2Seq window 120
- 시계열예측 문제를 회귀문제로 변환 후, LGBM 모델 적용 (xgboost, catboost와 비교했을 때, lgbm 성능이 우수했음)
- LGBM 회귀에서, label encoding으로 preprocessing 후 예측했을 때보다 mean encoding 으로 preprocessing 했을 때 성능 향상
- RMSE loss function -> MAE loss function 변경
- ![image](https://github.com/ohbigkite/2023_LG_AImers/assets/122765534/cf886573-492e-4521-bae1-fe828436510e)

### 📝 Conclusion

- 신호대 잡음비가 강한 시계열 데이터 → 간단한 딥러닝 모델인 lstm을 사용했을 때 가장 성능 good
- 딥러닝 모델에서, 메타데이터 활용 시 오히려 과적합 발생으로 성능 하락...
- 2023년 3월 급감하는 구간을 이상치로 판단했기에 장기주기성 포착을 위해 LSTM 기반 Seq2Seq을 사용 but 단순한 lstm의 성능이 우수했음 ( 모델의 복잡도 증가로 인한 성능하락으로 추측)
- Seq2Seq에서 window size를 90 → 120으로 늘려주었을 때 성능 증가(더 많은 과거 정보를 반영해줄 수록 성능이 증가한 것으로 보아, 급감구간은 이상치가 맞았던 것으로 추측됨)
- 트리기반 모델인 LGBM 사용 시, label encoding 보다 mean encoding에서 더 좋은 성능!
- 💡mean-encoding이란? label 값에 따른 예측값의 평균을 인코딩 값으로 설정해주는 것!
- label encoding에서의 encoding 값은 단순한 '구분'을 위한 장치였다면, mean encoding에서의 encoding값은 label에 따른 target 값의 평균이므로, target값과의 상관관계가 생겨 학습에 효과적!
