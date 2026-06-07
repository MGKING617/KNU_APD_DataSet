# 고급프로그래밍설계 텀 프로젝트 제출용 GitHub 자료

이 저장소는 고급프로그래밍설계 텀 프로젝트 제출 항목 중 GitHub 주소로 제출할 자료를 정리한 저장소입니다.

## 폴더 구성

| 폴더 | 내용 |
|---|---|
| `01_데이터셋/` | 분석과 학습에 활용한 원자료 및 최종 전처리 데이터 |
| `02_ipynb/` | 파이썬 코드셀과 결과 해석 주석셀을 포함한 Jupyter Notebook 및 시각화 산출물 |
| `03_실행코드/` | 프론트엔드, 백엔드, ML 서버, DB 초기화, 실행 스크립트 등 실행에 필요한 코드 |

## 데이터셋

`01_데이터셋/raw/`에는 원자료 XLSX가 포함되어 있고, `01_데이터셋/processed/`에는 최종 학습과 분석에 사용한 전처리 CSV가 포함되어 있습니다.

주요 파일은 다음과 같습니다.

- `01_데이터셋/raw/0519) 지역사회건강조사 재범주화.xlsx`
- `01_데이터셋/raw/0519) 지역사회건강조사 재범주화_수정본.xlsx`
- `01_데이터셋/processed/chs_training_0519_recoded_expert_consultation.csv`

## Jupyter Notebook

`02_ipynb/term_project_pandas_visualization.ipynb`에는 데이터 로드, 전처리 결과 확인, 변수별 Reference high 비율 분석, 모델 성능 비교, SHAP 변수 중요도 시각화 코드가 포함되어 있습니다.

노트북 실행에 필요한 주요 파일은 같은 폴더에 포함되어 있습니다.

- `term_project_insight_data.csv`
- `benchmark_metrics.csv`
- `shap_variable_contribution.csv`
- `figures/`

## 실행 코드

`03_실행코드/`는 실제 프로젝트 실행에 필요한 파일을 모아둔 폴더입니다.

- `frontend/`: React/Vite 기반 프론트엔드
- `backend/`: Spring Boot 기반 백엔드 API
- `ml/`: ML 모델 서버 및 학습 코드
- `ml_after_expert_consultation/`: 전문가 상담 후 변수 구성을 반영한 ML 실험 코드
- `database/`: DB 초기화 스크립트
- `scripts/`: 재학습 및 보조 실행 스크립트
- `root_config/`: 루트 실행 설정 예시

## 환경 변수

실제 키와 운영 API 주소는 코드에 포함하지 않았습니다. 
실행 전 `.env.example`을 참고하여 `.env`를 작성해서 API 키와 모델등을 입력해야 정상적으로 웹앱 기능이 실행됩니다.

주요 환경 변수는 다음과 같습니다.

- `VITE_API_BASE_URL`: 프론트엔드가 호출할 백엔드 API 주소
- `LLM_BASE_URL`: 백엔드가 호출할 LLM 서버 주소
- `OPENAI_API_KEY`: LLM API 키
- `DATABASE_URL`, `DB_USERNAME`, `DB_PASSWORD`: DB 접속 정보

## Docker 실행 예시

```bash
cp .env.example .env
docker compose -f docker-compose.example.yml --env-file .env up -d --build
```

