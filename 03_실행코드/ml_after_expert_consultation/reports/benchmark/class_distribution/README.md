# 클래스 분포 - Class Distribution

이미지 파일: class_distribution.png

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
이 그림은 전체 학습 데이터에서 Reference low와 Reference high가 각각 몇 건인지 보여준다. Reference high는 PHQ-9 합산점수 10점 이상인 표본이다.

벤치마크 용어 설명
- Reference high: PHQ-9 합산점수 10점 이상으로 정의한 참고 고위험군이다.
- Reference low: PHQ-9 합산점수 10점 미만으로 정의한 참고 저위험군이다.
- 클래스 불균형: 한쪽 클래스가 다른 쪽보다 훨씬 많은 상태를 말한다.

결과 해석
전체 134,672건 중 Reference high는 5,468건(4.1%), Reference low는 129,204건(95.9%)이다. 고위험군 비율이 낮기 때문에 단순 Accuracy만 보면 모델 성능을 과대평가할 수 있다.

보고서에 넣을 문장
"분석 데이터에서 Reference high 비율은 4.1%로 낮아, Accuracy보다 recall, PR-AUC, F1, 혼동행렬을 함께 고려하였다."

주의점
Reference high는 의료적 진단명이 아니라 PHQ-9 합산점수 기준으로 만든 참고 라벨이다.
