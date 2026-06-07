# 정밀도-재현율 곡선 - Precision Recall Curves

이미지 파일: precision_recall_curves.png

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
이 그림은 임계값을 여러 단계로 바꿨을 때 precision과 recall이 어떻게 바뀌는지 보여준다. Reference high 비율이 낮은 데이터에서는 ROC 곡선보다 현실적인 비교 자료가 될 수 있다.

벤치마크 용어 설명
- Precision: High라고 예측한 표본 중 실제 High인 비율이다.
- Recall: 실제 High 중 모델이 High로 잡아낸 비율이다.
- PR-AUC: precision-recall 곡선 아래 면적이다. 높을수록 좋다.

결과 해석
PR-AUC는 XGBoost 0.198, LightGBM 0.196, Logistic Regression 0.194, Random Forest 0.137 순이었다. 세 모델은 비슷했지만 Random Forest는 상대적으로 낮았다.

보고서에 넣을 문장
"Precision-recall 곡선 기준으로 XGBoost와 LightGBM이 가장 높은 PR-AUC를 보였고, Logistic Regression도 비슷한 수준이었다."

주의점
곡선 전체 성능과 0.590 임계값에서의 결과는 다를 수 있다. 최종 표는 benchmark_metrics.csv의 0.590 기준 지표를 함께 보아야 한다.
