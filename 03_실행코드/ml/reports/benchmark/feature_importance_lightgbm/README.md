# Feature Importance - LightGBM

이미지 파일: feature_importance_lightgbm.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 벤치마크 LightGBM 모델의 내부 feature importance를 보여준다. LightGBM 트리 학습 과정에서 어떤 변수가 분기에 많이 사용되었는지 또는 모델 성능 향상에 많이 기여했는지 나타내는 지표이다.

벤치마크 용어 설명
- Feature importance: 모델 내부에서 변수의 사용 정도나 기여도를 요약한 값이다.
- Split importance: 트리 모델에서 해당 변수가 분기 기준으로 사용된 횟수를 중심으로 계산되는 중요도이다.
- SHAP과의 차이: feature importance는 모델 내부 구조 기준이고, SHAP은 실제 예측값 변화 기준이다.

결과 해석
벤치마크 LightGBM에서는 age 2,890, household_size 1,486, stress_level 831, smoking_status 698, sex 698, economic_activity 646, education_level 633, subjective_health 623 순으로 높게 나타났다. 이는 LightGBM이 나이와 가구원 수를 트리 분기에서 자주 활용했음을 의미한다.

보고서에 넣을 문장
"LightGBM 내부 중요도에서는 age와 household_size가 높게 나타났으며, stress_level, smoking_status, sex, subjective_health도 주요 분기 변수로 사용되었다. 다만 본 지표는 모델 내부 사용 빈도에 가까우므로, 실제 예측 설명에는 SHAP 결과를 중심으로 해석하였다."

주의점
이 그래프는 인과관계를 의미하지 않는다. 또한 운영 모델의 SHAP 중요도 순위와 다를 수 있으며, 전문가 설명에서는 SHAP을 중심 지표로 사용하는 것이 더 적절하다.
