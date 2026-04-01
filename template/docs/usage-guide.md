# 상세 사용 가이드

> 코드를 모르는 사람도 따라할 수 있는 단계별 설명서입니다.
> 예시: "롤러코스터 타이쿤 웹 버전"을 만드는 상황으로 설명합니다.

---

## 목차

1. [사전 준비](#1-사전-준비)
2. [보일러플레이트 가져오기](#2-보일러플레이트-가져오기)
3. [내 프로젝트에 맞게 수정하기](#3-내-프로젝트에-맞게-수정하기)
4. [Claude Code로 프로젝트 시작하기](#4-claude-code로-프로젝트-시작하기)
5. [개발하기](#5-개발하기)
6. [코드 리뷰 & QA](#6-코드-리뷰--qa)
7. [배포하기](#7-배포하기)
8. [각 스킬 상세 설명](#8-각-스킬-상세-설명)

---

## 1. 사전 준비

### 필수 설치 항목

아래 도구들이 모두 설치되어 있어야 합니다.

#### 1-1. Node.js (v18 이상)
- 다운로드: https://nodejs.org/
- 설치 확인: 터미널에서 `node --version` 입력 → `v18.x.x` 이상이면 OK

#### 1-2. pnpm (패키지 매니저)
```bash
npm install -g pnpm
```
- 설치 확인: `pnpm --version`

#### 1-3. Git
- 다운로드: https://git-scm.com/
- 설치 확인: `git --version`

#### 1-4. Claude Code
- 설치 가이드: https://docs.anthropic.com/en/docs/claude-code/overview
- 설치 확인: 터미널에서 `claude` 입력 → Claude Code가 실행되면 OK

#### 1-5. GitHub 계정 & SSH 설정 (선택)
- PR 생성, 원격 push 등을 하려면 GitHub 계정과 SSH 키 설정이 필요합니다.
- 가이드: https://docs.github.com/en/authentication/connecting-to-github-with-ssh

---

## 2. 보일러플레이트 설치 & 프로젝트 초기화

### 2-1. 글로벌 설치 (PC에 한 번만)

스킬(12개 커맨드)을 PC에 설치합니다. 이후 어떤 프로젝트에서든 사용 가능합니다.

```bash
git clone --depth 1 https://github.com/judiking1/claude_starter.git ~/.claude/skills/claude_starter
cd ~/.claude/skills/claude_starter && ./setup
```

### 2-2. 새 프로젝트 시작

```bash
# 프로젝트 폴더 생성 (또는 기존 폴더 사용)
mkdir roller-coaster-tycoon && cd roller-coaster-tycoon
git init

# Claude Code 실행
claude
```

Claude Code 채팅창에서:
```
> /init roller-coaster-tycoon
```

이것만 하면 프로젝트 폴더에 `CLAUDE.md`, `.claude/rules/`, `docs/`가 생성됩니다.

### 2-3. 이미 만들어둔 GitHub repo에 적용

이미 `roller-coaster-tycoon` repo가 GitHub에 있고, 로컬에 clone해둔 상태:

```bash
cd ~/projects/roller-coaster-tycoon
claude
```

```
> /init roller-coaster-tycoon
```

초기화 후 터미널에서:
```bash
git add CLAUDE.md .claude docs
git commit -m "feat: add Claude Code boilerplate"
git push
```

기존 코드에 영향 없습니다 — `CLAUDE.md`, `.claude/`, `docs/`는 Claude Code가 읽는 문서일 뿐, 빌드나 실행과 무관합니다.

> 기존에 `.claude/` 폴더나 `CLAUDE.md`가 있다면 덮어쓸지 물어봅니다.

---

## 3. 내 프로젝트에 맞게 수정하기

보일러플레이트를 가져왔으면, `CLAUDE.md` 파일을 열어서 2곳만 수정합니다:

```
찾기:  [PROJECT_NAME]
바꾸기: roller-coaster-tycoon

찾기:  [PROJECT_DESCRIPTION]
바꾸기: Classic roller coaster tycoon game rebuilt as a modern web application
```

이것만 하면 준비 완료입니다. 나머지는 `/project-kickoff`이 알아서 해줍니다.

---

## 4. Claude Code로 프로젝트 시작하기

### 4-1. Claude Code 실행

터미널에서 프로젝트 폴더로 이동한 후:

```bash
cd ~/projects/roller-coaster-tycoon
claude
```

Claude Code가 실행되면 채팅 인터페이스가 나타납니다.

### 4-2. 프로젝트 킥오프

채팅창에 다음을 입력합니다:

```
/project-kickoff roller-coaster-tycoon
```

Claude가 단계별로 질문합니다. 한국어로 대답하면 됩니다:

**Phase 1 — 프로젝트 개요:**
```
Q: "이 프로젝트가 해결하려는 문제는?"
A: "롤러코스터 타이쿤 게임을 브라우저에서 플레이할 수 있게 만들고 싶어"

Q: "타겟 유저는?"
A: "레트로 게임 팬, 캐주얼 시뮬레이션 게임 좋아하는 사람"

Q: "핵심 기능 3-5개는?"
A: "놀이공원 건설, 롤러코스터 디자인, 손님 AI, 재정 관리, 연구 시스템"
```

**Phase 2 — 디자인 방향:**
```
Q: "디자인 톤은?"
A: "픽셀아트 + 모던 UI 혼합"

Q: "레이아웃은?"
A: "게임 화면 + 사이드바 컨트롤 패널"
```

**Phase 3 — 기술 결정:**
```
Q: "백엔드는?"
A: "없음. 순수 클라이언트 사이드"
```

**Phase 4 — Premise Challenge:**
Claude가 핵심 가정을 반박합니다:
```
"이 프로젝트에서 가장 위험한 가정: 브라우저에서 시뮬레이션 성능이
 충분할까? WebGL + Web Worker 조합이 필요할 수 있습니다."
```

**Phase 5 — 대안 비교:**
Claude가 2-3개 접근법을 제시합니다:
```
1. Minimal: Canvas 2D + 간단한 그리드 시스템 (Effort: M)
2. Ideal: WebGL + ECS 아키텍처 (Effort: XL)
3. Creative: Phaser.js 게임 엔진 활용 (Effort: L)
```

선택하면 Claude가 프로젝트를 자동으로 세팅합니다.

### 4-3. 디자인 시스템 구축

프로젝트 킥오프 후:

```
/design-system init
```

Claude가 프로젝트에 맞는 UI 컴포넌트(버튼, 입력, 모달 등)를 자동 생성합니다.

---

## 5. 개발하기

이제부터는 Claude와 대화하면서 기능을 구현합니다.

### 일반적인 대화
```
> 놀이공원 그리드 시스템을 만들어줘. 사용자가 타일을 클릭하면 건물을 배치할 수 있어야 해.
```

Claude가 코드를 작성하면, 자동으로 `.claude/rules/`의 규칙을 따릅니다:
- TypeScript strict 모드
- 네이밍 컨벤션 (camelCase, PascalCase 등)
- 성능 최적화 패턴 (useRef > useState 등)

### 큰 기능을 한 번에 구현하고 싶을 때
```
/team-lead 손님 시뮬레이션 시스템을 구현해줘.
손님 생성, 경로 탐색, 만족도 계산, UI 표시가 필요해.
```

Claude가 여러 에이전트를 동시에 실행하여 병렬로 구현합니다.

### 구현 전에 설계를 검토하고 싶을 때
```
/plan-review
```

Claude가 현재 구현 계획의 아키텍처, 테스트 커버리지, 실패 시나리오를 분석합니다.

### 버그를 만났을 때
```
/investigate
```

Claude가 체계적으로 근본 원인을 찾습니다:
1. 증상 수집
2. 가설 수립
3. 가설 검증 (최대 3회)
4. 수정 + 회귀 테스트

### 위험한 작업을 할 때
```
/careful
```

이후 `rm -rf`, `git push -f` 같은 위험한 명령을 실행하려 하면 Claude가 경고합니다.

---

## 6. 코드 리뷰 & QA

### 코드 리뷰
```
/review
```

Claude가 현재 변경사항을 7개 관점에서 리뷰합니다:
- Testing, Maintainability, Security, Performance, API Contract, Accessibility, Design
- 사소한 문제는 자동 수정 (AUTO-FIX)
- 판단이 필요한 문제는 질문 (ASK)

### QA 테스트
```
/qa
```

Claude가 브라우저에서 직접 앱을 테스트합니다:
- 페이지별 시각적 검사
- 버튼/폼 동작 확인
- 콘솔 에러 확인
- 반응형 레이아웃 체크
- 발견된 버그 자동 수정
- Health Score (0-100) 측정

---

## 7. 배포하기

```
/ship
```

Claude가 자동으로 처리합니다:
1. 테스트 전체 실행
2. 코드 리뷰 (인라인)
3. 버전 번호 업데이트
4. CHANGELOG 자동 생성
5. 커밋을 논리적 단위로 분리
6. GitHub에 push
7. PR (Pull Request) 생성

---

## 8. 각 스킬 상세 설명

### `/project-kickoff` — 프로젝트 시작
**언제 쓰나:** 새 프로젝트를 처음 시작할 때
**뭘 하나:**
- 대화형으로 프로젝트 컨셉 설정
- UX/UI 디자인 방향 결정
- 기술 스택 조정
- 핵심 가정 검증 (Premise Challenge)
- 2-3개 접근법 비교 (Alternatives)
- 프로젝트 구조 & 코드 자동 생성

### `/plan-review` — 아키텍처 리뷰
**언제 쓰나:** 복잡한 기능을 구현하기 전에
**뭘 하나:**
- 구현 계획의 범위가 적절한지 검증 (8파일 초과 시 경고)
- 데이터 흐름, 상태 관리 전략 검토
- 테스트 커버리지 다이어그램 (어디가 빠져있는지)
- 실패 시나리오 분석 (프로덕션에서 뭐가 터질 수 있는지)
- 기존 코드 재사용 가능 부분 식별

### `/team-lead` — 팀장 모드
**언제 쓰나:** 큰 기능을 한 번에 구현하고 싶을 때
**뭘 하나:**
- 작업을 독립적 단위로 분해
- 여러 에이전트를 병렬로 실행 (각각 격리된 환경)
- 결과물 품질 검증 + 통합
- 위험도 평가 (WTF-likelihood)

### `/design-system` — 디자인 시스템
**언제 쓰나:** UI 컴포넌트를 처음 구축하거나 추가할 때
**뭘 하나:**
- `init`: 디자인 토큰 + 기본 UI 컴포넌트 전체 생성
- `Button`: 특정 컴포넌트만 추가
- `audit`: 기존 컴포넌트 일관성 검사

### `/review` — 코드 리뷰
**언제 쓰나:** 기능 구현 후, PR 전에
**뭘 하나:**
- 7개 전문가 관점에서 병렬 리뷰
- 사소한 문제 자동 수정 (import 정리, 타입 추가 등)
- 심각한 문제는 사용자에게 질문
- 각 발견에 신뢰도 점수 (1-10) 부여

### `/qa` — QA 테스트
**언제 쓰나:** 기능이 완성되어 테스트가 필요할 때
**뭘 하나:**
- 브라우저에서 직접 앱을 열고 테스트
- 버튼 클릭, 폼 입력, 네비게이션 등 사용자 시나리오 테스트
- 발견된 버그 자동 수정 (한 커밋에 한 수정)
- 자기 조절: 수정이 오히려 해로우면 자동 중단
- Health Score 측정 (Console 15% + Functional 20% + Accessibility 15% + ...)

### `/ship` — 자동 배포
**언제 쓰나:** 기능이 완성되어 PR을 만들고 싶을 때
**뭘 하나:**
- 테스트 전체 실행
- 버전 번호 자동 결정
- CHANGELOG 자동 생성
- 커밋을 논리적 단위로 분리 (bisectable commits)
- PR 생성 (테스트 결과, 커버리지, 리뷰 요약 포함)

### `/investigate` — 체계적 디버깅
**언제 쓰나:** 버그를 만났을 때
**뭘 하나:**
- 근본 원인 분석 (추측으로 수정하지 않음)
- 가설을 세우고 검증 (최대 3회)
- 3회 실패하면 추측 반복 대신 에스컬레이션
- 수정 후 회귀 테스트 작성
- Scope Lock: 관련 파일만 수정, 범위 확산 방지

### `/careful` — 위험 명령 가드레일
**언제 쓰나:** 위험한 작업 전에 안전장치를 켜고 싶을 때
**뭘 하나:**
- `rm -rf`, `DROP TABLE`, `git push -f` 등 위험 명령 감지
- 실행 전에 경고 + 확인 요청
- `node_modules`, `dist` 등 빌드 산출물은 안전하게 허용
- 다시 입력하면 가드레일 해제

### `/retro` — 주간 회고
**언제 쓰나:** 한 주/한 달의 작업을 돌아보고 싶을 때
**뭘 하나:**
- Git 커밋 히스토리 분석
- 시간대별 작업 분포 (언제 가장 많이 코딩하는지)
- feat/fix/refactor 비율
- 핫스팟 파일 (가장 자주 바뀌는 파일 = 불안정한 코드)
- 포커스 점수 (한 영역에 집중했는지 vs 산발적)
- 연속 출근일 (streak)

### `/learn` — 세션 간 학습 관리
**언제 쓰나:** 세션에서 유용한 발견을 다음에도 기억하고 싶을 때
**뭘 하나:**
- `/learn`: 현재 세션에서 발견한 패턴·실수·선호도를 `.claude/learnings.md`에 기록
- `/learn review`: 지금까지 축적된 학습 내용 확인
- `/learn prune`: 오래되거나 불필요한 항목 정리
- `/learn search 키워드`: 학습 내용에서 키워드 검색
- 다음 세션에서 Claude가 자동으로 읽어 같은 실수를 반복하지 않음

---

## 자주 묻는 질문

### Q: Windows에서도 동작하나요?
네. Claude Code가 Windows를 지원하면 이 보일러플레이트도 동작합니다.

### Q: CLAUDE.md를 꼭 수정해야 하나요?
최소한 `[PROJECT_NAME]`과 `[PROJECT_DESCRIPTION]`은 수정해야 합니다.
나머지는 `/project-kickoff`이 자동으로 처리합니다.

### Q: 기술 스택을 바꾸고 싶으면?
`/project-kickoff`에서 대화하며 바꿀 수 있습니다.
수동으로 하려면 `CLAUDE.md`의 "기술 스택" 섹션과 `.claude/rules/`의 규칙 파일들을 수정하세요.

### Q: 이 파일들을 .gitignore에 넣어야 하나요?
**넣지 마세요.** 이 파일들은 프로젝트의 일부로 커밋해야 합니다.
다른 환경에서 Claude Code를 실행할 때도 같은 규칙이 적용되려면 repo에 포함되어야 합니다.

### Q: .claude/settings.local.json은 뭔가요?
Claude Code의 로컬 설정 파일입니다. 이건 각자의 환경에 맞게 자동 생성되므로 `.gitignore`에 넣는 것이 좋습니다.

### Q: 팀에서 같이 쓸 수 있나요?
네. repo에 보일러플레이트가 포함되어 있으면, 팀원 모두 같은 규칙으로 Claude Code를 사용할 수 있습니다. 단, 각자 Claude Code 구독이 필요합니다.

### Q: `/명령어`가 안 되면?
- Claude Code가 실행 중인지 확인 (터미널에서 `claude` 입력)
- `.claude/skills/` 폴더에 해당 SKILL.md 파일이 있는지 확인
- Claude Code를 재시작해보세요
