---
name: project-kickoff
description: 새 프로젝트 시작 시 컨셉, 페르소나, 디자인 패턴, 기술 결정을 대화형으로 설정
argument-hint: [project-name]
user-invocable: true
allowed-tools: Read, Write, Edit, Glob, Grep, Agent, AskUserQuestion, TodoWrite
---

# Project Kickoff Skill

새 프로젝트를 시작할 때 이 스킬을 실행합니다. 대화형으로 프로젝트의 핵심 설정을 완료합니다.

## 실행 순서

### Phase 1: 프로젝트 개요 설정
AskUserQuestion으로 다음을 파악:
1. **프로젝트 목적**: 이 프로젝트가 해결하려는 문제는?
2. **타겟 유저/페르소나**: 누가 사용하는가? (나이, 직업, 기술 수준, 사용 맥락)
3. **핵심 기능 3-5개**: MVP에 반드시 포함될 기능
4. **경쟁/참고 서비스**: 벤치마크할 서비스가 있는지
5. **프로젝트 규모**: 소규모(1-2주), 중규모(1-2개월), 대규모(3개월+)

### Phase 2: UX/UI 방향 설정
AskUserQuestion으로 다음을 파악:
1. **디자인 톤**: 미니멀/모던/playful/corporate/기타
2. **레이아웃 패턴**: 대시보드형/컨텐츠형/앱형/랜딩형
3. **색상 분위기**: 밝은/어두운/브랜드 색상 있는지
4. **참고 디자인**: 영감을 받은 사이트나 UI가 있는지
5. **반응형 우선순위**: 데스크톱 우선/모바일 우선

### Phase 3: 기술 결정
CLAUDE.md의 기본 스택을 기반으로 프로젝트에 맞게 조정:
1. 백엔드 방식 결정 (API Routes / Express / Supabase / 없음)
2. DB 선택 (PostgreSQL / Supabase / MongoDB / 없음)
3. 인증 방식 (없음 / JWT / OAuth / Supabase Auth)
4. 추가 라이브러리 필요 여부

### Phase 4: 문서 생성
파악한 정보를 바탕으로 자동 생성:
1. **CLAUDE.md 업데이트**: 플레이스홀더를 실제 값으로 교체
2. **docs/project-brief.md**: 프로젝트 개요, 페르소나, 핵심 기능
3. **docs/design-direction.md**: UX/UI 방향, 디자인 토큰 커스텀
4. **docs/adr/001-tech-decisions.md**: 기술 결정 기록
5. **docs/roadmap.md**: 마일스톤 및 기능별 우선순위

### Phase 5: 초기 구조 생성
```bash
# 프로젝트 초기화 (사용자 확인 후)
pnpm create vite . --template react-ts
pnpm install
pnpm add zustand @tanstack/react-query tailwindcss @tailwindcss/vite react-router-dom
pnpm add -D @biomejs/biome
pnpm biome init
```
- 폴더 구조 생성 (src/ 하위)
- tsconfig.json strict 설정 적용
- biome.json 설정
- 기본 레이아웃 컴포넌트 생성

### Phase 6: 세션 전략 추천
프로젝트 규모에 맞는 세션 구성을 추천하고 docs/session-plan.md에 기록:
- 소규모: 단일 세션
- 중규모: Architect + Dev (2세션)
- 대규모: PM + Design + Frontend + Backend + QA (5세션)

## 한국어로 대화하세요
모든 질문과 설명은 한국어로 진행합니다. 코드와 문서 내용은 영어입니다.
