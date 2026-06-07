# 모델 성능 비교 - Metrics Comparison

이미지 파일: metrics_comparison.png

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
이 그림은 네 모델의 주요 성능지표를 한 번에 비교한 막대그래프이다. 비교 지표는 AUROC, PR-AUC, positive precision, positive recall, positive F1이다.

벤치마크 용어 설명
- AUROC: Low와 High를 전반적으로 구분하는 능력이다. 1에 가까울수록 좋다.
- PR-AUC: precision과 recall의 균형을 보는 지표이다. 고위험군 비율이 낮은 데이터에서 중요하다.
- Positive recall: 실제 High 중 모델이 High로 잡아낸 비율이다.
- Positive precision: 모델이 High라고 한 사람 중 실제 High인 비율이다.
- Positive F1: precision과 recall의 균형을 하나로 요약한 값이다.

결과 해석
0.590 기준에서 Logistic Regression은 recall 0.793으로 가장 높았다. XGBoost는 PR-AUC 0.198로 가장 높았고, LightGBM은 AUROC 0.835, PR-AUC 0.196, recall 0.748로 비슷한 수준을 보였다. Random Forest는 recall 0.419로 High 포착력이 낮았다.

보고서에 넣을 문장
"전문가 상담 후 변수 구성에서는 Logistic Regression의 recall이 가장 높았고, XGBoost와 LightGBM은 PR-AUC 기준으로 비슷한 수준을 보였다."

주의점
이번 결과는 기존 ml 모델과 변수 구성이 다르므로 같은 모델명이라도 직접 대체 결과로 보면 안 된다. 전문가 상담 후 변수만 바꾼 별도 비교 실험이다.
