# 변수 중요도 - Random Forest

이미지 파일: feature_importance_random_forest.png

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
이 그림은 Random Forest 모델에서 각 변수가 분류 불순도 감소에 얼마나 기여했는지 보여준다. 값이 높을수록 여러 결정나무에서 분류를 나누는 데 많이 기여했다는 뜻이다.

벤치마크 용어 설명
- Random Forest: 여러 개의 결정나무를 학습해 예측을 종합하는 모델이다.
- Impurity decrease: 변수를 사용해 데이터를 나눴을 때 클래스가 더 잘 구분되는 정도이다.
- Feature importance: 각 변수의 평균적인 분류 기여도를 요약한 값이다.

결과 해석
Random Forest에서는 stress_level 0.484, age 0.255가 특히 높았고, 그 다음으로 smoking_status, sex, alcohol_frequency, weight_control 등이 이어졌다.

보고서에 넣을 문장
"Random Forest에서는 stress_level과 age가 가장 큰 중요도를 보였고, 흡연, 성별, 음주 빈도도 보조적으로 활용되었다."

주의점
Random Forest 중요도는 변수의 분류 기여도 기준이다. LightGBM, XGBoost, Logistic Regression의 중요도와 같은 단위가 아니므로 숫자를 직접 비교하면 안 된다.
