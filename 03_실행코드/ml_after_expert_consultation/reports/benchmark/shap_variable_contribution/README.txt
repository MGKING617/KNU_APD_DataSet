# SHAP 변수 기여도

이미지 파일: shap_variable_contribution.png

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
이 그림은 재학습한 LightGBM 모델에서 각 변수가 실제 예측값을 얼마나 많이 움직였는지 SHAP 평균 절댓값으로 보여준다. 모델 내부 사용 빈도보다 예측 설명에 더 가까운 자료이다.

벤치마크 용어 설명
- SHAP: 각 변수가 예측값을 올리거나 낮추는 기여도를 계산하는 설명 방법이다.
- Mean absolute SHAP: 방향은 빼고 영향의 크기만 평균낸 값이다.
- Feature importance와의 차이: feature importance는 모델 내부 구조 기준이고, SHAP은 예측값 변화 기준이다.

결과 해석
SHAP 기준으로는 stress_level이 가장 컸고, 그 다음으로 sex, smoking_status, age, marital_status, region_group, walking_days가 주요 변수로 나타났다.

보고서에 넣을 문장
"LightGBM SHAP 분석에서는 stress_level, sex, smoking_status, age가 예측값 변화에 크게 기여하는 변수로 확인되었다."

주의점
SHAP도 인과관계를 의미하지 않는다. 변수와 예측값 사이의 모델 내부 관계를 설명하는 지표로 해석해야 한다.
