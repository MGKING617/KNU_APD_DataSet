# 변수 중요도 - XGBoost

이미지 파일: feature_importance_xgboost.png

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
이 그림은 XGBoost 모델에서 어떤 변수가 예측 성능 향상에 많이 기여했는지 보여준다. 값이 높을수록 XGBoost가 해당 변수를 강하게 활용했다는 뜻이다.

벤치마크 용어 설명
- XGBoost: 이전 트리의 오류를 다음 트리가 보완하는 gradient boosting 계열 모델이다.
- Gain 기반 중요도: 변수를 사용했을 때 모델 손실이 얼마나 줄어드는지에 가까운 기준이다.
- Feature importance: 모델 내부에서 변수별 기여도를 요약한 값이다.

결과 해석
XGBoost에서는 stress_level 0.749가 가장 높았고, sex 0.050, smoking_status 0.036, basic_livelihood_recipient 0.024, accident_poisoning_experience 0.020 순으로 나타났다.

보고서에 넣을 문장
"XGBoost에서는 stress_level의 중요도가 가장 크게 나타났고, sex와 smoking_status가 그 다음 주요 변수로 확인되었다."

주의점
XGBoost 중요도는 XGBoost 내부 기준으로 계산된 값이다. LightGBM이나 Random Forest의 importance와 숫자 크기를 직접 비교하지 말고, 같은 모델 안에서의 순위를 중심으로 해석해야 한다.
