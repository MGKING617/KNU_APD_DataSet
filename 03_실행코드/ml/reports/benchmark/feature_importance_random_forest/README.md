# Feature Importance - Random Forest

이미지 파일: feature_importance_random_forest.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 Random Forest 모델에서 변수 중요도를 보여준다. Random Forest는 여러 의사결정나무를 결합한 모델이며, 각 변수가 나무 분기에서 성능 향상에 얼마나 기여했는지를 중요도로 계산한다.

벤치마크 용어 설명
- Random Forest: 여러 트리의 예측을 종합하는 앙상블 모델이다.
- Impurity-based importance: 트리 분기에서 불순도 감소에 얼마나 기여했는지를 보는 중요도이다.
- Overfitting: 학습 데이터에는 매우 잘 맞지만 새로운 데이터에서는 성능이 떨어지는 현상이다.

결과 해석
Random Forest에서는 stress_level 0.341, age 0.151, subjective_health 0.117, household_size 0.070 순으로 높았다. 학습곡선에서는 Random Forest의 train PR-AUC가 test PR-AUC보다 크게 높아 과적합 가능성이 보였다. 따라서 변수 중요도는 참고로 볼 수 있지만, 운영 모델 설명의 중심으로 삼기에는 제한이 있다.

보고서에 넣을 문장
"Random Forest 중요도에서도 stress_level이 가장 높은 변수로 나타났고, age와 subjective_health가 뒤를 이었다. 다만 학습 성능과 테스트 성능 차이가 커 과적합 가능성이 있으므로 보조적 참고 지표로 해석하였다."

주의점
트리 기반 중요도는 연속형 변수나 분기 가능성이 많은 변수에 유리하게 나타날 수 있다. 따라서 SHAP 등 다른 해석 지표와 함께 비교해야 한다.
