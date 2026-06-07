# LightGBM Threshold Trade-off

이미지 파일: lightgbm_threshold_tradeoff.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 LightGBM에서 의사결정 임계값을 바꿀 때 precision, recall, F1, TP, FP, FN이 어떻게 변하는지 보여준다. 임계값이 낮으면 더 많은 사람을 High로 예측하고, 임계값이 높으면 High로 예측되는 사람이 줄어든다.

벤치마크 용어 설명
- Decision threshold: 모델 예측확률이 이 값 이상이면 High로 분류하는 기준이다.
- Precision과 recall trade-off: 임계값을 낮추면 recall은 높아지지만 FP가 늘어 precision이 낮아질 수 있다. 임계값을 높이면 precision은 올라갈 수 있지만 FN이 늘어 recall이 낮아질 수 있다.
- TP, FP, FN: 각각 실제 High를 잡은 건수, 실제 Low를 High로 잘못 예측한 건수, 실제 High를 놓친 건수이다.

결과 해석
벤치마크 LightGBM은 약 0.588 임계값에서 target recall 조건을 만족하도록 설정되었다. 임계값 0.59 근처에서는 precision 0.159, recall 0.718, F1 0.260 수준이다. 임계값을 0.50으로 낮추면 recall은 0.769로 올라가지만 FP가 5,178건으로 늘어난다. 임계값을 0.70으로 높이면 precision은 0.178로 올라가지만 recall은 0.593으로 낮아진다.

보고서에 넣을 문장
"LightGBM 임계값 분석은 고위험군을 더 많이 포착할수록 false positive가 함께 증가하는 trade-off를 보여준다. 본 프로젝트는 확정 진단보다 조기 선별을 목적으로 하므로 target recall 조건을 만족하는 임계값을 선택하였다."

주의점
이 그래프는 벤치마크 LightGBM 기준이며 실제 운영 모델의 임계값 0.590과 매우 유사하지만 완전히 같은 모델 산출물은 아니다.
