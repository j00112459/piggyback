# Piggyback Git · GitHub 협업 컨벤션

> 적용 대상: `Piggyback` 단일 저장소  
> 마지막 정리: 2026-09-01

## 1. 기본 원칙

- 모든 기능 작업은 `Issue 생성 -> feature/chore/fix 브랜치 생성 -> Pull Request -> develop 병합` 순서로 진행합니다.
- `main`, `develop` 브랜치에는 직접 push하지 않습니다.
- 브랜치는 개인 이름이 아니라 기능 단위로 만듭니다.
- 프론트엔드와 백엔드는 하나의 저장소를 함께 사용하되, 담당 폴더를 명확히 구분합니다.

```text
piggyback/
├─ frontend/    # Vue 3 + TypeScript
├─ backend/     # Spring Boot + MySQL
├─ docs/        # 컨벤션, ERD, API 명세 등
└─ .github/     # Issue/PR 템플릿
```

## 2. 브랜치 전략

```text
main
  ↑
develop
  ↑
feature/*
chore/*
fix/*
```

| 브랜치 | 역할 | 생성 기준 | 병합 대상 |
| --- | --- | --- | --- |
| `main` | 최종 안정 버전 | 직접 작업 금지 | `develop -> main` PR |
| `develop` | 기능 통합 브랜치 | 직접 작업 금지 | `feature/*`, `chore/*`, `fix/*` PR |
| `feature/*` | 실제 기능 개발 | Issue 또는 기능 단위 | `develop` |
| `chore/*` | 설정, 환경, 템플릿, 문서 구조 작업 | 작업 단위 | `develop` |
| `fix/*` | 버그 수정 | 버그 Issue 또는 수정 단위 | `develop` |

## 3. 브랜치 이름 규칙

```text
feature/파트-이슈번호-기능명
fix/파트-이슈번호-수정내용
chore/작업내용
```

예시:

```text
feature/fe-consult-input
feature/fe-12-consult-result
feature/be-18-intent-api
feature/ai-22-fraud-guardrail
fix/fe-31-route-error
chore/github-templates
```

- 이슈 번호가 아직 없으면 번호 없이 작성할 수 있습니다.
- 하나의 브랜치에는 가능한 한 하나의 기능 또는 하나의 목적만 담습니다.
- 다른 사람의 브랜치에는 허가 없이 push하지 않습니다.

## 4. 작업 흐름

### 작업 시작

```bash
git switch develop
git pull origin develop
git switch -c feature/fe-consult-input
```

### 작업 완료

```bash
git add .
git commit -m "feat: 상담 입력 화면 추가"
git push -u origin feature/fe-consult-input
```

### Pull Request

```text
feature/* -> develop
chore/* -> develop
fix/* -> develop
```

PR 제목 예시:

```text
[FE] 상담 입력 화면 구현
[BE] 상담 분석 API 구현
[AI] 업무 분류 응답 구조 추가
[Docs] GitHub 템플릿 추가
```

## 5. Pull Request 규칙

- PR은 기능 하나가 동작 가능한 단위로 올립니다.
- 화면만 먼저 올리는 경우, 서버 연동 전이라는 점을 PR 본문에 적습니다.
- 관련 Issue가 있으면 `Closes #12` 형식으로 연결합니다.
- API, DB, AI 응답 구조 변경은 PR 본문에 변경 전/후를 적습니다.
- 해커톤 특성상 승인자는 필수가 아니지만, 충돌과 실행 오류는 병합 전 확인합니다.

## 6. GitHub Ruleset 기준

| 대상 브랜치 | 기준 |
| --- | --- |
| `main` | PR 병합만 허용, force push 금지, 브랜치 삭제 금지 |
| `develop` | PR 병합만 허용, force push 금지, 브랜치 삭제 금지 |
| 공통 | 민감 정보 커밋 금지, 직접 수정 금지 |

`Restrict updates`는 켜지 않습니다. 이 옵션을 켜면 protected branch의 PR merge가 막힐 수 있습니다.

## 7. Issue 관리

Issue 제목 예시:

```text
[FE] 상담 입력 화면 구현
[BE] 업무 분석 API 구현
[AI] 사기 위험 표현 탐지 로직 구현
[DB] 업무별 준비물 테이블 설계
[Docs] API 명세 작성
```

권장 라벨:

```text
frontend / backend / ai / db / common / docs / bug
priority: high / priority: medium / priority: low
```

## 8. 금지 사항

- `main`, `develop`에 직접 push
- 다른 사람 브랜치에 허가 없이 push
- 빌드 에러 또는 실행 불가 상태를 설명 없이 병합
- `.env`, API Key, DB 비밀번호, 개인 토큰 커밋
- 충돌 해결 중 다른 사람 코드를 이해 없이 삭제
- 의미 없는 대규모 포맷 변경을 기능 PR에 섞기

## 9. PR 전 체크리스트

- [ ] 최신 `develop` 기준으로 작업했는가?
- [ ] 브랜치가 기능 단위이고 올바른 접두어를 사용하는가?
- [ ] PR 대상 브랜치가 `develop`인가?
- [ ] 커밋 메시지가 작업 내용을 설명하는가?
- [ ] 로컬에서 화면/API가 동작하는가?
- [ ] 관련 Issue와 PR을 연결했는가?
- [ ] API, DB, AI 응답 구조 변경 사항을 문서와 팀원에게 공유했는가?
