# ROC Curves

이미지 파일: roc_curves.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
ROC 곡선은 임계값을 바꿔가며 false positive rate와 true positive rate의 관계를 보여주는 그래프이다. 곡선이 왼쪽 위에 가까울수록 Low와 High를 잘 구분한다.

벤치마크 용어 설명
- True positive rate: 실제 High 중 High로 예측한 비율이다. recall과 같은 의미이다.
- False positive rate: 실제 Low 중 High로 잘못 예측한 비율이다.
- AUROC: ROC 곡선 아래 면적이다. 1에 가까울수록 구분 능력이 좋고, 0.5는 무작위에 가까운 수준이다.

결과 해석
AUROC는 Logistic Regression 0.861, XGBoost 0.859, LightGBM 0.851, Random Forest 0.845 순으로 나타났다. 네 모델 모두 0.84 이상으로 Low와 High를 어느 정도 구분하는 능력을 보였다. 다만 고위험군 비율이 4.1%로 낮기 때문에 AUROC만으로 선별 성능을 판단하면 부족하고, PR-AUC와 recall도 함께 봐야 한다.

보고서에 넣을 문장
"ROC 분석에서 네 모델 모두 AUROC 0.84 이상을 보여 Reference low와 Reference high를 구분하는 전반적 판별력은 확보하였다. 다만 본 데이터는 고위험군 비율이 낮은 불균형 데이터이므로 PR-AUC와 positive recall을 함께 해석하였다."

주의점
AUROC가 높아도 실제 High를 충분히 잘 찾는다는 뜻은 아닐 수 있다. 특히 불균형 데이터에서는 precision과 recall을 같이 확인해야 한다.
