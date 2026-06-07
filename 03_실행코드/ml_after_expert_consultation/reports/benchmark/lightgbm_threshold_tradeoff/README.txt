# LightGBM 임계값 변화 분석

이미지 파일: lightgbm_threshold_tradeoff.png

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
이 그림은 LightGBM에서 임계값을 바꿀 때 precision, recall, F1, TP, FP, FN이 어떻게 변하는지 보여준다. 이번 실험에서 공식 비교 기준은 0.590이다.

벤치마크 용어 설명
- Threshold: 모델 확률을 Low와 High로 나누는 기준값이다.
- Precision: 모델이 High라고 한 사람 중 실제 High인 비율이다.
- Recall: 실제 High 중 모델이 High로 잡아낸 비율이다.
- F1: precision과 recall의 균형을 요약한 값이다.

결과 해석
LightGBM은 0.590 기준에서 precision 0.130, recall 0.748, F1 0.221을 보였다. 임계값을 낮추면 recall은 올라가지만 FP가 늘고, 임계값을 높이면 FP는 줄지만 FN이 늘어난다.

보고서에 넣을 문장
"LightGBM 임계값 변화 분석에서는 0.590 기준의 precision, recall, F1과 오탐/미탐 변화를 확인하였다."

주의점
이 그래프는 임계값을 임의로 새로 고르기 위한 것이 아니라, 0.590 기준이 어떤 trade-off를 가지는지 설명하기 위한 자료이다.
