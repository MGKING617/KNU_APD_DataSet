# Positive Class Metrics

이미지 파일: positive_class_metrics.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 Reference high, 즉 PHQ-9 10점 이상 참고 고위험군에 대한 precision, recall, F1만 따로 비교한 그래프이다. 전체 정확도가 아니라 고위험군을 얼마나 잘 찾는지에 초점을 둔다.

벤치마크 용어 설명
- Positive class: 본 연구에서는 Reference high를 의미한다.
- Precision: 모델이 High라고 예측한 사람 중 실제 High인 비율이다.
- Recall: 실제 High 중 모델이 High로 찾아낸 비율이다.
- F1: precision과 recall의 조화평균이다. 둘 중 하나만 높을 때보다 균형을 확인하는 데 유용하다.

결과 해석
Logistic Regression의 positive recall은 0.727로 가장 높다. LightGBM은 0.719로 비슷한 수준이며, XGBoost는 precision 0.163과 F1 0.265가 가장 높다. Random Forest는 recall 0.704와 F1 0.246으로 상대적으로 낮다. 우울 고위험군 조기 선별에서는 실제 고위험군을 놓치지 않는 것이 중요하므로 recall을 핵심 지표로 볼 수 있다.

보고서에 넣을 문장
"고위험군 선별 성능만 별도로 비교한 결과, Logistic Regression과 LightGBM은 0.72 내외의 recall을 보였고, XGBoost는 F1에서 가장 높았다. 본 서비스는 확정 진단보다 조기 선별을 목표로 하므로 positive recall을 중요하게 고려하였다."

주의점
Precision이 낮다는 것은 High로 안내받은 사용자 중 실제 Reference high가 아닌 사람이 많을 수 있다는 뜻이다. 따라서 서비스 화면에서는 진단 표현보다 상담 권장 또는 추가 점검 필요와 같은 완화된 표현을 사용해야 한다.
