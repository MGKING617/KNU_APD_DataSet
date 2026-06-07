# Feature Importance - XGBoost

이미지 파일: feature_importance_xgboost.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 XGBoost 모델의 변수 중요도를 보여준다. XGBoost는 순차적으로 트리를 추가하면서 이전 모델의 오류를 보완하는 gradient boosting 계열 모델이다.

벤치마크 용어 설명
- XGBoost: 성능이 강한 boosting 기반 트리 모델이다.
- Feature importance: XGBoost 내부에서 각 변수가 예측 성능 향상에 기여한 정도를 나타낸다.
- Boosting: 약한 모델을 순차적으로 쌓아 성능을 높이는 방식이다.

결과 해석
XGBoost에서는 stress_level 0.609가 가장 높고, subjective_health 0.159가 뒤를 이었다. 그 다음은 smoking_status, sex, education_level, body_self_overweight 순으로 나타났다. XGBoost는 PR-AUC와 F1에서 가장 높은 성능을 보였지만, 실제 운영 모델은 LightGBM으로 연결되어 있다.

보고서에 넣을 문장
"XGBoost 중요도에서는 stress_level과 subjective_health가 가장 큰 비중을 차지했으며, 이는 SHAP 및 다른 모델 중요도 분석과 일관되게 주요 심리 신호가 예측에 크게 반영되었음을 보여준다."

주의점
XGBoost가 일부 성능지표에서 우세하더라도 현재 웹사이트 운영 모델은 LightGBM이다. 보고서에서는 성능 비교와 운영 모델 설명을 구분해야 한다.
