# Feature Importance - Logistic Regression

이미지 파일: feature_importance_logistic_regression.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 Logistic Regression에서 각 변수의 계수 절댓값을 기준으로 중요도를 나타낸 것이다. 계수 절댓값이 클수록 해당 변수가 모델 점수에 더 크게 작용한다.

벤치마크 용어 설명
- Logistic Regression: 선형 모델로, 각 변수에 하나의 가중치를 부여해 High 가능성을 계산한다.
- Coefficient: 변수에 곱해지는 가중치이다. 부호는 방향, 절댓값은 영향 크기를 의미한다.
- Absolute coefficient: 방향을 제거하고 영향 크기만 본 값이다.

결과 해석
Logistic Regression에서는 stress_level 1.069, subjective_health 0.573, sex 0.312, smoking_status 0.244, body_self_overweight 0.202 순으로 영향 크기가 컸다. 이는 SHAP 중요도 상위 변수와 유사하게 스트레스와 주관적 건강상태가 핵심 신호로 잡혔다는 점을 보여준다.

보고서에 넣을 문장
"Logistic Regression의 계수 기반 중요도에서도 stress_level과 subjective_health가 가장 큰 영향을 보였으며, sex, smoking_status, body_self_overweight가 뒤를 이었다. 이는 여러 모델에서 심리적 스트레스와 주관적 건강상태가 반복적으로 중요한 신호로 나타났음을 시사한다."

주의점
이 그래프는 계수의 절댓값이므로 방향성은 따로 확인해야 한다. 또한 선형 모델의 결과이므로 LightGBM의 비선형 상호작용 구조와는 해석 방식이 다르다.
