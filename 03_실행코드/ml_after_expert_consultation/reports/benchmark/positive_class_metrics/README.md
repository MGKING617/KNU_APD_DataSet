# 고위험군 지표 비교 - Positive Class Metrics

이미지 파일: positive_class_metrics.png

공통 벤치마크 기준
- 데이터: 전문가 상담 후 재전처리한 지역사회건강조사 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 변수 변경: 가구원 수는 인원수 그대로 쓰지 않고 1인 가구 여부(single_person_household)로 재범주화하였다.
- 제외 변수: 주관적 건강인식, 체형인식(마름/비만), 아침식사 빈도, 교육수준은 전문가 상담 내용을 반영해 입력 변수에서 제외하였다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 임계값 기준: 이번 전문가 상담 후 실험에서는 모든 모델의 예측 확률 임계값을 0.590으로 고정하였다.
- 해석 주의: 이 폴더의 결과는 기존 ml 폴더를 대체한 것이 아니라, 전문가 상담 이후 변수 구성을 바꿔 다시 돌린 별도 비교 실험이다.

그림이 보여주는 것
이 그림은 Reference high만 놓고 precision, recall, F1을 비교한다. 선별 모델에서 중요한 High 포착 성능을 한눈에 보기 위한 그래프이다.

벤치마크 용어 설명
- Positive class: 여기서는 Reference high를 뜻한다.
- Precision: High라고 예측한 표본 중 실제 High인 비율이다.
- Recall: 실제 High 중 모델이 High로 포착한 비율이다.
- F1: precision과 recall의 균형 지표이다.

결과 해석
Logistic Regression은 recall 0.793으로 가장 높았고, XGBoost는 recall 0.780, LightGBM은 recall 0.748이었다. Random Forest는 precision 0.151로 가장 높았지만 recall 0.419로 낮았다.

보고서에 넣을 문장
"Positive class 기준으로 Logistic Regression은 가장 높은 포착률을 보였고, Random Forest는 상대적으로 보수적인 예측 경향을 보였다."

주의점
Precision만 높으면 놓치는 High가 많아질 수 있고, recall만 높으면 오탐이 늘 수 있다. 선별 목적에서는 둘의 균형을 함께 보아야 한다.
