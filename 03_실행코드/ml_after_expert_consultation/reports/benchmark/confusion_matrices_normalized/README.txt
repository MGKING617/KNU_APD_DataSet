# 정규화 혼동행렬 - Normalized Confusion Matrices

이미지 파일: confusion_matrices_normalized.png

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
이 그림은 혼동행렬을 실제 클래스별 비율로 바꿔 보여준다. 실제 Low 중 몇 퍼센트를 Low로 맞췄는지, 실제 High 중 몇 퍼센트를 High로 잡았는지 확인할 수 있다.

벤치마크 용어 설명
- 정규화 혼동행렬: 단순 건수가 아니라 각 실제 클래스 안에서의 비율로 변환한 혼동행렬이다.
- Sensitivity 또는 recall: 실제 High 중 모델이 High로 잡은 비율이다.
- Specificity: 실제 Low 중 모델이 Low로 맞춘 비율이다.

결과 해석
High 포착률은 Logistic Regression 79.3%, XGBoost 78.0%, LightGBM 74.8%, Random Forest 41.9% 순이었다. Low를 Low로 맞추는 비율은 Random Forest가 90.1%로 가장 높았다.

보고서에 넣을 문장
"정규화 혼동행렬에서는 Logistic Regression과 XGBoost가 Reference high 포착률이 높았고, Random Forest는 Reference low 판별에 더 보수적인 경향을 보였다."

주의점
비율 그래프는 클래스 규모 차이를 줄여 보여주지만, 실제 후속 확인 대상 규모를 보려면 원자료 혼동행렬의 FP, TP 건수도 함께 확인해야 한다.
