# 학습곡선 - Learning Curves

이미지 파일: learning_curves.png

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
이 그림은 학습 데이터 수를 늘렸을 때 각 모델의 train PR-AUC와 hold-out PR-AUC가 어떻게 변하는지 보여준다. 데이터가 늘어날수록 성능이 안정되는지 확인하는 용도이다.

벤치마크 용어 설명
- Learning curve: 학습 데이터 크기 변화에 따른 성능 변화를 보는 그래프이다.
- Train PR-AUC: 학습에 사용한 데이터에서의 PR-AUC이다.
- Hold-out PR-AUC: 따로 떼어둔 테스트 데이터에서의 PR-AUC이다.

결과 해석
이번 실험에서 최종 hold-out PR-AUC는 XGBoost 0.198, LightGBM 0.196, Logistic Regression 0.194, Random Forest 0.137 순이었다.

보고서에 넣을 문장
"학습곡선은 데이터 규모 변화에 따른 모델 안정성을 확인하기 위해 사용했으며, 최종 PR-AUC는 XGBoost와 LightGBM이 비슷한 수준이었다."

주의점
학습곡선은 성능 안정성 확인용이다. 최종 성능 판단은 전체 테스트셋 기준 benchmark_metrics 결과를 우선으로 보아야 한다.
