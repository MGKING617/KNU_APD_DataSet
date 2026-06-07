# Class Distribution

이미지 파일: class_distribution.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 학습 데이터의 정답 라벨 분포를 보여준다. Reference low는 PHQ-9 10점 미만, Reference high는 PHQ-9 10점 이상이다. 전체 134,672건 중 Reference low는 129,204건, Reference high는 5,468건이다.

벤치마크 용어 설명
- Class distribution: 정답 라벨이 각 범주에 얼마나 분포하는지 나타낸다.
- Reference high: 본 연구에서 우울 고위험군 참고 라벨로 둔 집단이다. 임상 진단명이 아니라 PHQ-9 점수 기준의 참고 그룹이다.
- Class imbalance: 한쪽 클래스가 훨씬 많은 상태이다. 본 데이터는 Reference high가 4.1%뿐이라 강한 불균형 데이터이다.

결과 해석
Reference high 비율이 4.1%로 낮기 때문에 accuracy만 보면 모델이 좋아 보일 수 있다. 예를 들어 대부분을 Low로 예측해도 accuracy는 높게 나올 수 있다. 따라서 이 연구에서는 accuracy보다 positive recall, PR-AUC, F1, 혼동행렬을 함께 보는 것이 중요하다.

보고서에 넣을 문장
"분석 데이터는 Reference low 129,204건과 Reference high 5,468건으로 구성되며, 고위험군 참고 라벨의 비율은 4.1%이다. 따라서 본 연구는 불균형 이진 분류 문제이며, 단순 정확도보다 고위험군 탐지 성능을 함께 평가하였다."

주의점
Reference high는 PHQ-9 10점 이상 기준의 참고 라벨이다. 실제 정신과 전문의의 임상 진단과 동일한 의미로 해석하면 안 된다.
