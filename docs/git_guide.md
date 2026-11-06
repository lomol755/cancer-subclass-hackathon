# 팀 깃 사용 안내

1. 저장소 설정: `git config --local commit.template ./.github/commit_template.txt`
2. 최신화: `git checkout main`, `git pull origin main`
3. 작업 브랜치: `git checkout -b feature/작업내용`
4. 저장: `git add .`, `git commit`
5. 업로드: `git push origin feature/작업내용`
6. PR 생성 후 팀원 1명 이상 확인, 승인 뒤 `main`에 병합

규칙: `main`에 직접 push하지 않고, 하루 1회 이상 커밋하며, 데이터 파일은 올리지 않습니다.

최종 제출 코드는 전체 과정을 주석과 함께 하나의 `.ipynb` 또는 `.py` 파일로 정리합니다.

## 유의사항

- `data/`, 모델 파일, `.env`는 저장소에 올리지 않습니다.
- 커밋 전에 `git status`로 올라갈 파일을 확인합니다.
- Test로 fit, 통계 계산, EDA를 하지 않고 행 수와 인덱스도 활용하지 않습니다.
- 전처리, 특성 선택, 변이 사전은 폴드 안에서 다시 학습합니다.
- 공개 코드, 과거 제출 코드, 타 대회 코드를 기반으로 작성하지 않습니다.
- 현재 운영 중인 대회 페이지에서 개인 계정 1개로만 제출합니다.
- 시드, 파라미터, 라이브러리 버전을 코드에 기록합니다.
- 최종 코드는 `/data` 경로를 포함해 오류 없이 재실행되는지 확인합니다.
- Data Leakage가 확인되면 리더보드 1위여도 실격될 수 있습니다.
