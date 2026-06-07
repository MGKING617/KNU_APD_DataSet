# LightGBM Outcome Group Contributions

이미지 파일: lightgbm_outcome_group_contributions.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 LightGBM의 signed SHAP 값을 TP, FP, FN, TN 결과집단별로 평균낸 그래프이다. 각 변수가 예측값을 High 방향으로 밀었는지, Low 방향으로 밀었는지를 집단별로 비교한다.

벤치마크 용어 설명
- SHAP: 모델 예측값에 각 변수가 얼마나 기여했는지 설명하는 방법이다.
- Signed SHAP: 부호가 있는 SHAP 값이다. 양수는 High 예측을 높이는 방향, 음수는 High 예측을 낮추는 방향으로 해석한다.
- TP: 실제 High를 High로 맞힌 집단이다.
- FP: 실제 Low인데 High로 잘못 예측한 집단이다.
- FN: 실제 High인데 Low로 놓친 집단이다.
- TN: 실제 Low를 Low로 맞힌 집단이다.

결과 해석
TP와 FP 집단에서는 stress_level과 subjective_health가 High 예측을 크게 높이는 방향으로 작용했다. TN 집단에서는 stress_level과 subjective_health가 음의 방향으로 나타나 Low 예측에 기여했다. FN 집단은 TP보다 주요 신호의 양의 기여가 작아, 모델이 실제 High를 충분히 강하게 감지하지 못한 사례로 해석할 수 있다.

보고서에 넣을 문장
"결과집단별 SHAP 분석에서 TP와 FP 집단은 stress_level과 subjective_health가 고위험군 예측을 높이는 방향으로 크게 기여하였다. 반대로 TN 집단에서는 해당 변수들이 고위험군 예측을 낮추는 방향으로 작용하였다."

주의점
이 그래프는 방향성을 보여주는 그래프이며, 평균 절댓값 SHAP 중요도 그래프와 목적이 다르다. 또한 인과관계를 의미하지 않는다.
