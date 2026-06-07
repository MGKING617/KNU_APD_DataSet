# Normalized Confusion Matrices

이미지 파일: confusion_matrices_normalized.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 혼동행렬을 비율로 바꿔 보여준다. 각 행은 실제 집단을 기준으로 100%가 되도록 정규화되어 있다. 따라서 실제 Low 중 몇 퍼센트가 Low 또는 High로 예측되었는지, 실제 High 중 몇 퍼센트가 Low 또는 High로 예측되었는지 한눈에 볼 수 있다.

벤치마크 용어 설명
- Normalized confusion matrix: 건수가 아니라 비율로 나타낸 혼동행렬이다.
- Specificity: 실제 Low를 Low로 맞히는 비율이다. 그림의 왼쪽 위 칸에 해당한다.
- Recall 또는 sensitivity: 실제 High를 High로 맞히는 비율이다. 그림의 오른쪽 아래 칸에 해당한다.
- False negative rate: 실제 High를 Low로 놓친 비율이다. 그림의 왼쪽 아래 칸에 해당한다.

결과 해석
LightGBM은 실제 Low의 83.8%를 Low로 맞히고, 16.2%를 High로 잘못 예측했다. 실제 High에 대해서는 71.9%를 High로 포착했고, 28.1%는 Low로 놓쳤다. Logistic Regression은 실제 High 포착률이 72.7%로 가장 높고, XGBoost는 실제 Low를 Low로 맞히는 비율이 84.7%로 가장 높다.

보고서에 넣을 문장
"정규화 혼동행렬에서 LightGBM은 실제 고위험군의 71.9%를 고위험군으로 탐지하였다. 반면 실제 저위험군의 16.2%는 고위험군으로 예측되어, 조기 선별 목적에서 recall을 확보하는 대신 false positive가 발생하는 구조를 확인할 수 있다."

주의점
정규화 비율은 모델의 성향을 이해하기 좋지만, 실제 사용자 수를 보려면 원본 혼동행렬의 건수도 함께 확인해야 한다.
