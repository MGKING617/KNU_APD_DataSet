# Metrics Comparison

이미지 파일: metrics_comparison.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 네 모델의 주요 성능지표를 한 번에 비교한 막대그래프이다. 비교 지표는 AUROC, PR-AUC, positive precision, positive recall, positive F1이다. 같은 테스트셋에서 각 모델이 Reference high를 얼마나 잘 구분하고 포착했는지 비교한다.

벤치마크 용어 설명
- AUROC: Low와 High를 전반적으로 구분하는 능력이다. 1에 가까울수록 좋다.
- PR-AUC: precision과 recall의 균형을 보는 지표이다. 고위험군 비율이 낮은 불균형 데이터에서는 AUROC보다 더 현실적인 비교 지표가 될 수 있다.
- Positive recall: 실제 High 중 모델이 High로 잡아낸 비율이다. 선별 모델에서는 놓치는 사람을 줄이는 데 중요하다.
- Positive precision: 모델이 High라고 한 사람 중 실제 High인 비율이다.
- Positive F1: precision과 recall의 균형을 하나로 요약한 값이다.

결과 해석
Logistic Regression은 AUROC 0.861, positive recall 0.727로 가장 높았다. XGBoost는 PR-AUC 0.225와 positive F1 0.265가 가장 높았다. LightGBM은 AUROC 0.851, PR-AUC 0.218, positive recall 0.719, positive F1 0.259로 운영 기준에 가까운 안정적인 성능을 보였다. Random Forest는 상대적으로 PR-AUC와 F1이 낮았다.

보고서에 넣을 문장
"벤치마크 비교에서 Logistic Regression은 고위험군 recall이 가장 높았고, XGBoost는 PR-AUC와 F1에서 가장 높았다. 운영 모델로 사용한 LightGBM은 recall 중심 선별 목적에 적합한 수준의 성능을 보였으며, 실제 서비스에는 별도 운영 임계값을 적용한 LightGBM 모델을 연결하였다."

주의점
이 그림은 모델 간 상대 비교용이다. 지표 하나만으로 최종 모델의 임상적 적합성을 판단하기보다, 선별 목적, 설명 가능성, 웹서비스 연동 가능성을 함께 고려해야 한다.
