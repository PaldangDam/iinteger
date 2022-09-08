# iinteger

Dacon 팔당댐 안전 수위예측 경진대회를 진행하며 기록한 레포지토리입니다.



## 조사할 내용

* rainfall data의 장소와 예측해야하는 장소 간 관계
  
  * rainfall data의 각 장소와 대교간 거리
  
  * 각 장소는 [링크](http://www.hrfco.go.kr/sumun/rainfallList.do#)에서 조회할 수 있다고 함

* water data의 방류량과 대교 수위와의 관계
  
  * 팔당댐과 대교의 거리 (직선거리, 수로의 거리)
  
  * 총 방류된 수량이 각 대교의 유량, 수위에 반영되는 시차
  
  * 총 방류된 수량이 각 대교의 유량, 수위에 반영되는 정도 (방류된 물은 각 대교에 나뉘어져 흘러들어갈 것)

* 각 컬럼 간 관계 (correlation heatmap으로 그려보면 편함)
  
  * 청담대교 유량과 청담대교 수위의 관계
  
  * 강화대교 조위는 어떤 컬럼인지

* ### 전처리를 마친 데이터로 훈련한 결과 스코어가 매우 낮았음. 따라서 공유된 코드를 기반으로 점수를 개선하고자 함.
  * 현재 점수 3.78349, 순위 72/276
  * 외부 하천 등 추가 데이터 및 앙상블 적용 예정


* ### 최종 스코어
  * Public : 2.79191 (40/308)
  * Private : 2.66474 (53/308)

  * 제공된 데이터 이외에 주요 대교 근처 하천의 데이터 삽입
  * 관측 시간으로부터 시간 데이터를 생성. 시간은 sin, cos 함수를 적용하였으며, 월-일을 합쳐서 최대 365일의 값을 가질 수 있는 month-day 컬럼과 시간-분을 합쳐 최대 1440분의 값을 가질 수 있는 hour-minute 컬럼을 생성
  
  * 컬럼 선택
    * 삭제했을 때 좋았은 특성 : swl, inf, hour_month_sin, month_day_sin
    * 삭제하지 않았을 때 좋았던 특성 : fw_1018630, sfw, tide_level, hour_minute_cos, month_day_cos, ecpc, tototf
    * 컬럼의 결측치 갯수나 상관관계가 컬럼의 성능과 절대적으로 관련이 있는것은 아니었움
  
  * 방류된 물이 각 대교에 도달하는 시간을 고려하여, 총 방류량과 각 대교 수위의 상관관계가 가장 높아지는 시간 차 계산. 계산된 값에 따라 tototf 변수를 shift하여 사용함. 그러나 모든 변수가 점수 향상을 가져오진 않았음
  
  * RandomForestRegressor, GradientBoostingRegressor, XGBRegressor, LGBMRegressor를 앙상블하여 사용























