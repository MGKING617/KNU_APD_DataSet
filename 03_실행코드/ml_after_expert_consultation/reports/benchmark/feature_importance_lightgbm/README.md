# 변수 중요도 - LightGBM

이미지 파일: feature_importance_lightgbm.png

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
이 그림은 LightGBM 모델 안에서 어떤 변수가 트리 분기 기준으로 많이 사용되었는지 보여준다. 값이 높을수록 LightGBM이 예측 과정에서 해당 변수를 자주 활용했다는 뜻에 가깝다.

벤치마크 용어 설명
- Feature importance: 모델 내부에서 변수의 사용 정도나 기여도를 요약한 값이다.
- LightGBM importance: 이 그래프에서는 주로 트리 분기에서 사용된 정도를 반영한다.
- SHAP과의 차이: feature importance는 모델 내부 구조 기준이고, SHAP은 실제 예측값 변화 기준이다.

결과 해석
LightGBM에서는 age 3,157, stress_level 1,000, sex 939, region_group 926, walking_days 918, economic_activity 848 순으로 높게 나타났다. 기존에 높게 보였던 household_size는 single_person_household로 바꿔 들어갔다.

보고서에 넣을 문장
"LightGBM 내부 중요도에서는 age, stress_level, sex, region_group, walking_days가 주요 분기 변수로 나타났다."

주의점
이 그래프는 인과관계를 의미하지 않는다. 또한 다른 모델의 importance 값과 숫자 크기를 직접 비교하면 안 되고, LightGBM 내부의 상대적 순위로 해석해야 한다.
