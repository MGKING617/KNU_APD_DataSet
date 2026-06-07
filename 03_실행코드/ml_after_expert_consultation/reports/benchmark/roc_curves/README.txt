# ROC 곡선 - ROC Curves

이미지 파일: roc_curves.png

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
이 그림은 각 모델이 Reference low와 Reference high를 전반적으로 얼마나 잘 구분하는지 보여준다. 곡선이 왼쪽 위에 가까울수록 구분 성능이 좋다.

벤치마크 용어 설명
- ROC curve: 민감도와 거짓양성률의 관계를 보여주는 곡선이다.
- AUROC: ROC 곡선 아래 면적이다. 1에 가까울수록 구분력이 좋다.
- False positive rate: 실제 Low 중 High로 잘못 예측한 비율이다.

결과 해석
AUROC는 Logistic Regression 0.839, XGBoost 0.838, LightGBM 0.835, Random Forest 0.809 순이었다. 세 모델은 비슷했고 Random Forest는 조금 낮았다.

보고서에 넣을 문장
"ROC 곡선 기준으로 Logistic Regression, XGBoost, LightGBM은 유사한 구분 성능을 보였고, Random Forest는 상대적으로 낮았다."

주의점
AUROC는 클래스 불균형에 덜 민감하게 좋아 보일 수 있다. Reference high 비율이 낮기 때문에 PR-AUC와 recall도 함께 해석해야 한다.
