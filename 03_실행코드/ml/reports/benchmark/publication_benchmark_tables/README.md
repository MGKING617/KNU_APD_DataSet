# Publication Benchmark Tables

이미지 파일: publication_benchmark_tables.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 발표나 보고서에 바로 넣기 쉽도록 성능지표와 혼동행렬 요약을 표 형태로 정리한 이미지이다. 위 표는 AUROC, PR-AUC, recall, precision, F1, balanced accuracy를 보여주고, 아래 표는 TN, FP, FN, TP와 고위험군 포착 건수를 보여준다.

벤치마크 용어 설명
- Balanced accuracy: Low와 High 각각의 recall을 평균낸 값이다. 불균형 데이터에서 accuracy보다 공정한 비교 지표가 될 수 있다.
- Detected High: 실제 High 중 모델이 High로 잡아낸 건수이다. TP를 TP와 FN의 합으로 나눈 값이 recall이다.
- False Alerts: 실제 Low인데 High로 예측된 FP 건수이다.

결과 해석
Logistic Regression은 recall 0.727로 고위험군 포착률이 가장 높다. XGBoost는 PR-AUC 0.225와 F1 0.265가 가장 높다. LightGBM은 recall 0.719, precision 0.158, F1 0.259로 운영 모델 기준에 가까운 성능을 보인다. Random Forest는 전체 accuracy는 높게 보일 수 있으나 고위험군 선별 지표에서는 상대적으로 낮다.

보고서에 넣을 문장
"발표용 성능표에서는 Logistic Regression이 recall에서, XGBoost가 PR-AUC와 F1에서 가장 높게 나타났다. LightGBM은 운영 모델로 채택되어 임계값 기반 recall 중심 선별 성능을 확보하였다."

주의점
표의 LightGBM 수치는 벤치마크 LightGBM 기준이다. 실제 웹사이트 운영 모델의 임계값은 0.590이며, 벤치마크 표의 0.588과 거의 유사하지만 보고서에서는 두 기준을 구분해 설명하는 것이 안전하다.
