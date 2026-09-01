# Piggyback 코드 컨벤션

## 1. 공통 코드 원칙

- 읽기 쉬운 코드를 우선합니다. 과도한 추상화와 불필요한 공통화는 피합니다.
- 파일과 함수는 한 가지 책임을 갖도록 작성합니다.
- 이름만 보고 역할을 알 수 있게 짓습니다. `data`, `temp`, `a`, `result2` 같은 이름은 피합니다.
- 들여쓰기, 따옴표, 줄바꿈은 개인 취향보다 자동 포맷터를 우선합니다.
- 주석은 코드가 왜 필요한지 설명할 때만 짧게 작성합니다.
- 예외 처리는 조용히 무시하지 말고, 사용자 메시지 또는 서버 로그로 확인 가능하게 남깁니다.
- API 필드명, 상태값, 공통 용어는 API 명세를 기준으로 통일합니다.

## 2. 네이밍 공통

| 대상 | 규칙 | 예시 |
| --- | --- | --- |
| 클래스, Vue 컴포넌트 | PascalCase | `ConsultInputView`, `FraudWarningPanel` |
| 변수, 함수, 메서드 | camelCase | `selectedIntent`, `fetchConsultResult()` |
| Boolean | `is`, `has`, `can`, `should` 등으로 시작 | `isLoading`, `hasFraudRisk`, `canVisit` |
| 상수 | UPPER_SNAKE_CASE | `MIN_CONFIDENCE_THRESHOLD` |
| URL, DB 컬럼, JSON 키 | API 명세 기준 | `visit_required`, `fraud_risk` |

## 3. 프론트엔드 규칙

기술 스택:

```text
Vue 3
TypeScript
Vite
Vue Router
Pinia
ESLint
Prettier
```

권장 구조:

```text
src/
├── api/          기능별 API 호출 모듈
├── components/   재사용 UI 컴포넌트
├── views/        라우트 단위 화면
├── stores/       Pinia 전역 상태
├── composables/  재사용 로직
├── router/       라우팅 설정
├── types/        공용 TypeScript 타입
└── utils/        순수 유틸 함수
```

작성 기준:

- Vue 파일은 `<script setup lang="ts">`와 Composition API를 사용합니다.
- `views`는 화면 조합과 흐름을 담당하고, `components`는 재사용 가능한 UI를 담당합니다.
- 한 화면에서만 쓰는 입력값은 해당 컴포넌트에 둡니다.
- 여러 화면에서 공유되는 상담 결과, 선택된 업무, 사용자 상태만 Pinia에 둡니다.
- API 호출은 `src/api/`에 모으고, 컴포넌트 안에서 fetch/axios 설정을 반복하지 않습니다.
- 사용자에게 보이는 로딩, 실패, 빈 상태를 구현합니다.
- 서버 오류 원문, 토큰, API Key, DB 정보는 화면에 노출하지 않습니다.

시니어 친화 UI 기준:

- 한 화면에는 하나의 핵심 질문과 하나의 주요 행동을 둡니다.
- 버튼과 입력 영역은 충분히 크게 만들고, 문장은 쉬운 말로 작성합니다.
- 전문 용어를 쓰면 쉬운 설명을 함께 보여줍니다.
- 음성 입력 결과는 사용자가 직접 수정할 수 있게 합니다.
- 뒤로가기 또는 다시 선택하기 흐름을 항상 제공합니다.

## 4. 백엔드 규칙

기술 스택:

```text
Java 17
Spring Boot
Spring Web
Spring Data JPA
MySQL
Lombok
Validation
```

권장 구조:

```text
com.piggyback.backend
├── controller
├── service
├── repository
├── entity
├── dto
├── config
├── exception
└── BackendApplication.java
```

작성 기준:

- Controller는 요청 검증, Service 호출, HTTP 응답을 담당합니다.
- Service는 비즈니스 규칙, 추천 로직, 트랜잭션을 담당합니다.
- Repository는 DB 접근만 담당합니다.
- Entity를 API 응답으로 직접 노출하지 않고 Response DTO를 사용합니다.
- Request DTO와 Response DTO는 분리합니다.
- 입력값 검증은 DTO에서 `Validation`으로 처리합니다.
- 예상 가능한 오류는 공통 예외 응답 방식으로 처리합니다.
- API prefix는 `/api`를 기본으로 사용합니다.
- DB 비밀번호, API Key, Secret은 코드에 직접 작성하지 않습니다.

## 5. AI 기능 규칙

AI 기능은 다음 원칙을 따릅니다.

- AI는 사용자의 말을 금융 업무 유형으로 분류합니다.
- AI 응답은 가능한 한 구조화된 형태로 다룹니다.
- confidence가 낮으면 단정하지 않고 후보를 제시하거나 상담 안내로 종료합니다.
- 사기 위험 표현이 감지되면 일반 업무 안내보다 경고와 안전 행동 안내를 우선합니다.
- AI는 금융 결정을 대신하지 않습니다.
- 사용자의 민감 정보, 계좌번호, 비밀번호, 인증번호는 저장하거나 로그에 남기지 않습니다.

권장 응답 필드:

```json
{
  "intent": "PASSBOOK_REISSUE",
  "confidence": 0.86,
  "visitRequired": true,
  "fraudRisk": false,
  "reason": "통장 재발급은 신분 확인이 필요할 수 있습니다.",
  "checklist": ["신분증", "기존 통장"],
  "recommendedBranchId": 1,
  "recommendedTime": "내일 오전 10시"
}
```

## 6. 공용 변경 규칙

아래 영역은 여러 기능에 영향을 주므로 수정 전 담당자에게 공유합니다.

- FE: `router/`, Pinia store, API 호출 모듈, `App.vue`, 공통 Layout, 공통 컴포넌트
- BE: 전역 예외 처리, 공통 응답 형식, CORS 설정, DB 설정, JPA 설정
- AI: intent 목록, confidence 기준, safety guardrail, LLM prompt, structured output
- 공통: API 명세, DB 스키마, 환경 변수 키, 의존성 버전, 배포 설정

API URL, Request, Response, 상태값을 변경하면 코드 변경보다 API 명세 수정과 공유를 먼저 합니다.

## 7. 보안 규칙

- `.env`, 키 파일, 토큰, 실제 개인정보는 절대 커밋하지 않습니다.
- 필요한 환경 변수 이름은 `.env.example` 또는 문서로만 공유합니다.
- DB 비밀번호는 `MYSQL_PASSWORD` 같은 환경 변수로 관리합니다.
- AI API Key는 프론트엔드에 두지 않고 백엔드 환경 변수로 관리합니다.
- 로그에는 비밀번호, 인증번호, 계좌번호, API Key를 남기지 않습니다.
