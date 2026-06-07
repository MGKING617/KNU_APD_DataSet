# Precision-Recall Curves

이미지 파일: precision_recall_curves.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
Precision-Recall 곡선은 임계값을 바꿔가며 precision과 recall의 관계를 보여준다. 고위험군처럼 양성 비율이 낮은 문제에서는 ROC 곡선보다 실제 선별 성능을 더 잘 보여줄 수 있다.

벤치마크 용어 설명
- Precision: High 예측 중 실제 High의 비율이다.
- Recall: 실제 High 중 모델이 High로 찾아낸 비율이다.
- PR-AUC: Precision-Recall 곡선 아래 면적이다. 값이 높을수록 고위험군을 찾으면서도 불필요한 High 예측을 줄이는 균형이 좋다.
- Baseline: 양성 비율이다. 이 데이터의 기준선은 약 0.041이다.

결과 해석
PR-AUC는 XGBoost 0.225, Logistic Regression 0.223, LightGBM 0.218, Random Forest 0.186 순으로 나타났다. 모두 baseline 0.041보다 높아 무작위보다 의미 있는 선별 정보를 제공한다. XGBoost가 PR-AUC에서 가장 높지만, 운영 모델은 설명 가능성과 서비스 연동 기준을 함께 고려해 LightGBM으로 구성되었다.

보고서에 넣을 문장
"Precision-Recall 분석에서 모든 모델의 PR-AUC가 양성 비율 기준선보다 높게 나타나, 고위험군 선별에 유의미한 예측 정보를 제공하였다. XGBoost가 PR-AUC에서 가장 높았고, LightGBM은 운영 모델로서 recall 중심 임계값을 적용하였다."

주의점
PR-AUC는 양성 클래스가 적은 데이터에서 중요한 지표지만, 단독으로 임상적 유용성을 보장하지는 않는다. 혼동행렬과 사용자에게 전달되는 결과 표현까지 함께 검토해야 한다.
