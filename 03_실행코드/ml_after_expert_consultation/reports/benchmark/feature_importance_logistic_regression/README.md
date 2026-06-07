# 변수 중요도 - Logistic Regression

이미지 파일: feature_importance_logistic_regression.png

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
이 그림은 Logistic Regression에서 각 변수의 회귀계수 절댓값을 기준으로 중요도를 보여준다. 계수의 절댓값이 클수록 예측 점수에 미치는 영향이 크다는 뜻이다.

벤치마크 용어 설명
- Coefficient: 회귀모델에서 변수가 예측 점수를 어느 방향으로 얼마나 움직이는지 나타내는 값이다.
- 절댓값 중요도: 양수와 음수 방향을 구분하지 않고 영향의 크기만 본 값이다.
- 방향성: 실제로 위험을 높이는 방향인지 낮추는 방향인지는 별도 coefficient CSV의 부호를 함께 보아야 한다.

결과 해석
Logistic Regression에서는 stress_level 1.147, sex 0.346, smoking_status 0.328, marital_status 0.155, economic_activity 0.150 순으로 계수 크기가 컸다.

보고서에 넣을 문장
"Logistic Regression에서는 stress_level, sex, smoking_status가 예측 점수에 큰 영향을 주는 변수로 나타났다."

주의점
이 그래프는 계수의 크기만 보여주므로 방향성까지 설명하려면 signed_coefficient 값을 확인해야 한다. 트리 모델의 importance와 계산 방식도 다르다.
