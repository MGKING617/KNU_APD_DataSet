# Learning Curves

이미지 파일: learning_curves.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
학습곡선은 학습 데이터 수를 늘렸을 때 train PR-AUC와 hold-out PR-AUC가 어떻게 변하는지 보여준다. 모델이 데이터를 더 받으면 좋아지는지, 학습 데이터에만 과하게 맞는지 확인하는 데 사용한다.

벤치마크 용어 설명
- Train PR-AUC: 학습에 사용한 데이터에서의 PR-AUC이다.
- Hold-out PR-AUC: 따로 떼어둔 테스트 데이터에서의 PR-AUC이다.
- Generalization: 학습 데이터가 아닌 새 데이터에서도 성능이 유지되는 능력이다.
- Overfitting: train 성능은 높지만 hold-out 성능이 낮은 상태이다.

결과 해석
Logistic Regression은 train PR-AUC 0.235, hold-out PR-AUC 0.224로 차이가 작아 일반화가 안정적이다. LightGBM은 train PR-AUC 0.404, hold-out PR-AUC 0.213으로 차이가 있으며, Random Forest는 train PR-AUC 0.784, hold-out PR-AUC 0.191로 과적합 가능성이 크다. XGBoost는 train PR-AUC 0.304, hold-out PR-AUC 0.217 수준이다.

보고서에 넣을 문장
"학습곡선 분석에서 Logistic Regression은 train과 hold-out 성능 차이가 작아 안정적인 일반화 양상을 보였다. 반면 Random Forest는 train 성능이 매우 높고 hold-out 성능은 낮아 과적합 가능성이 확인되었다."

주의점
학습곡선은 모델 선택을 보조하는 진단 그래프이다. 최종 운영 모델 판단에는 성능, 설명 가능성, 서비스 연동 구조, 임계값 전략을 함께 고려해야 한다.
