# 혼동행렬 - Confusion Matrices

이미지 파일: confusion_matrices.png

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
이 그림은 각 모델이 실제 Reference low와 Reference high를 어떻게 예측했는지 TP, FP, FN, TN 개수로 보여준다. 이번 실험에서는 모든 모델에 임계값 0.590을 동일하게 적용하였다.

벤치마크 용어 설명
- TP: 실제 High를 모델도 High로 맞춘 경우이다.
- FP: 실제 Low인데 모델이 High라고 예측한 경우이다.
- FN: 실제 High인데 모델이 Low라고 놓친 경우이다.
- TN: 실제 Low를 모델도 Low로 맞춘 경우이다.

결과 해석
0.590 기준에서 Logistic Regression은 TP 868, FN 226으로 High를 가장 많이 포착했다. XGBoost는 TP 853, FN 241, LightGBM은 TP 818, FN 276이었다. Random Forest는 FP가 가장 적었지만 TP 458, FN 636으로 High를 놓친 비율이 컸다.

보고서에 넣을 문장
"혼동행렬 기준으로는 Logistic Regression이 Reference high를 가장 많이 포착했고, Random Forest는 오탐은 적었지만 미탐이 많았다."

주의점
선별 목적에서는 FN을 줄이는 것이 중요하지만, FP가 많아지면 불필요한 후속 확인 대상도 늘어난다. 따라서 TP와 FN뿐 아니라 FP도 함께 보아야 한다.
