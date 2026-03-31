# CLAUDE.md — Claude Code Boilerplate

> 새 프로젝트 시작 시 플레이스홀더를 채우고, `/project-kickoff`로 프로젝트 설정을 진행하세요.

## 프로젝트 정보

- **프로젝트명**: [PROJECT_NAME]
- **설명**: [PROJECT_DESCRIPTION]
- **타겟**: 데스크톱 웹 우선 (반응형)
- **배포**: Vercel

## 기술 스택

| 영역 | 기술 | 비고 |
|------|------|------|
| 프레임워크 | React + Vite | SPA 기본 |
| 언어 | TypeScript (strict) | no-any, 타입 가드 필수 |
| 스타일링 | Tailwind CSS v4 | 유틸리티 퍼스트 |
| 상태관리 | Zustand | 경량 전역 상태 |
| 서버 상태 | TanStack Query v5 | fetch 기반 |
| 패키지 매니저 | pnpm | 빠른 속도, 엄격한 의존성 |
| 린터/포매터 | Biome | 올인원 |

## 협업 방식

- **역할**: 페어 프로그래머 — 함께 고민하는 동료. 지시 수행자가 아님
- **코드** (변수, 함수, 주석, 커밋): 영어 / **대화**: 한국어
- **실수 대응**: 원인을 교육적으로 설명 (왜 틀렸는지 → 올바른 패턴)
- **자율성**: 높음 — 목표만 주면 설계~구현까지. 아키텍처 변경만 확인
- **동기부여**: 작업을 15-30분 단위로 분해, 반복/지루한 작업은 자동 처리
- **접근법 공유**: 코드 작성 전 방향을 간단히 공유하고, 더 나은 방법 제안
- **학습 기회**: 설계 결정 시 trade-off 설명, TS 고급 패턴/아키텍처 교육적 맥락 제공

## 핵심 TypeScript 규칙

- `any` 금지 → `unknown` + 타입 가드
- 모든 public 함수에 반환 타입 명시
- `interface` 우선 (`type`은 union/intersection에만)
- `enum` 대신 `as const` 객체
- `as` 타입 단언 최소화

## 네이밍 컨벤션

| 대상 | 패턴 | 예시 |
|------|------|------|
| 컴포넌트 파일 | PascalCase | `UserProfile.tsx` |
| 훅/유틸 파일 | camelCase | `useAuth.ts`, `formatDate.ts` |
| 디렉토리 | kebab-case | `user-profile/` |
| 변수/함수 | camelCase | `userName`, `handleClick` |
| 타입/인터페이스 | PascalCase | `UserProfile`, `ApiResponse` |
| 상수 | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| boolean | is/has/should 접두사 | `isLoading`, `hasError` |

## 아키텍처 핵심 (성능 우선)

- **`useRef` > `useState`**: 리렌더 불필요한 값은 반드시 ref
- **Typed Arrays**: 대량 수치 데이터는 `Float32Array` 등 사용
- **코드 스플리팅**: `React.lazy()` + `Suspense` 라우트 단위
- **가상화**: 긴 리스트는 `@tanstack/react-virtual`
- **barrel file 남용 금지**: 트리 셰이킹 방해

## 폴더 구조

```
src/
├── components/{ui,layout}/  # 재사용 UI 컴포넌트
├── hooks/                   # 커스텀 훅
├── services/                # API 통신
├── stores/                  # Zustand 스토어
├── types/                   # 공유 타입
├── utils/                   # 유틸리티 함수
├── pages/                   # 페이지/라우트
├── styles/                  # 전역 스타일
├── constants/               # 상수
└── assets/                  # 정적 파일
```

## Git 규칙

- **브랜치**: `main` + `feat/`, `fix/`, `refactor/`
- **커밋**: Conventional Commits (`feat:`, `fix:`, `refactor:`, `perf:`, `chore:`)
- **PR**: Claude가 자동 생성, Summary + Test Plan 포함

## 도구 활용

- **Claude Preview**: UI 변경 시 시각적 확인
- **/simplify**: 기능 완성 후 품질 점검
- **/commit**: 자동 커밋
- **/project-kickoff**: 새 프로젝트 컨셉/페르소나/디자인 설정
- **/team-lead**: 팀장 모드로 Agent 서브에이전트 오케스트레이션

## 상세 규칙 참조

> 아래 규칙들은 `.claude/rules/`에 모듈별로 분리되어 자동 로드됩니다:
> - `typescript.md` — TS 상세 규칙 및 패턴 예시
> - `components.md` — 컴포넌트 패턴, import 순서
> - `performance.md` — 성능 최적화 상세
> - `error-handling.md` — 에러 처리 패턴
> - `design-system.md` — 타이포/색상/간격/접근성
> - `git-workflow.md` — 커밋/PR/브랜치 상세

## 프로젝트 시작 체크리스트

1. [ ] 플레이스홀더 채우기 (`[PROJECT_NAME]`, `[PROJECT_DESCRIPTION]`)
2. [ ] `/project-kickoff` 실행 — 컨셉, 페르소나, 디자인 패턴 설정
3. [ ] `pnpm create vite . --template react-ts`
4. [ ] 핵심 패키지 설치 (Tailwind, Zustand, TanStack Query, Biome)
5. [ ] 프로젝트 규모에 맞는 세션 구성 결정
