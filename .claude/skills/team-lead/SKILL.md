---
name: team-lead
description: 팀장 모드 — Agent 서브에이전트를 오케스트레이션하여 병렬로 기능 구현. 작업을 분해하고 각 에이전트에 역할을 배정
argument-hint: [task-description]
user-invocable: true
allowed-tools: Read, Write, Edit, Glob, Grep, Agent, Bash, TodoWrite, AskUserQuestion
---

# Team Lead Skill — 팀장 모드

이 스킬은 하나의 세션에서 팀장 역할을 수행합니다.
Agent tool로 서브에이전트(팀원)를 스폰하여 병렬로 작업을 진행합니다.

## 작업 흐름

### Step 1: 요구사항 분석
- CLAUDE.md와 docs/ 디렉토리의 문서를 읽어 프로젝트 컨텍스트 파악
- 사용자의 요청을 분석하여 작업 단위로 분해
- TodoWrite로 작업 목록 생성

### Step 2: 작업 분배 설계
작업의 성격에 따라 에이전트 구성을 결정:

**병렬 가능한 작업** (서로 의존성 없음):
- Agent tool을 병렬로 호출하여 동시 진행
- 각 에이전트는 `isolation: "worktree"`로 격리된 환경에서 작업

**순차 작업** (의존성 있음):
- 선행 작업 에이전트 완료 후 후행 작업 진행

### Step 3: 에이전트 스폰
각 에이전트에게 명확한 역할과 작업 지시를 전달:

```
Agent: Frontend Component
- CLAUDE.md의 규칙을 따라
- [구체적 컴포넌트/페이지] 구현
- 완료 후 결과 보고

Agent: API Service
- CLAUDE.md의 규칙을 따라
- [구체적 API/서비스] 구현
- 완료 후 결과 보고
```

### Step 4: 품질 검증
모든 에이전트 완료 후:
1. 각 에이전트의 결과물 검토
2. 통합 시 충돌 여부 확인
3. CLAUDE.md 규칙 준수 여부 확인
4. 필요 시 수정 지시

### Step 5: 사용자 보고
작업 결과를 사용자에게 한국어로 요약 보고:
- 완료된 작업 목록
- 각 작업의 핵심 변경사항
- 주의할 점이나 추가 결정 필요 사항
- 다음 단계 제안

## 사용자 피드백 반영
docs/project-brief.md와 docs/design-direction.md에 저장된 사용자 의견을 기준으로
에이전트 작업물을 자율적으로 평가합니다.
사용자의 직접 피드백이 필요한 경우에만 AskUserQuestion으로 질문합니다.

## 에이전트 프롬프트 템플릿

에이전트에게 전달할 때 반드시 포함할 내용:
1. "CLAUDE.md를 읽고 규칙을 따라라"
2. 구체적 작업 범위 (어떤 파일, 어떤 기능)
3. 기대 결과물 (어떤 파일이 생성/수정되어야 하는지)
4. 품질 기준 (타입 안전성, 성능, 접근성 등)

## 한국어로 소통
사용자에게 보고할 때는 한국어. 에이전트에게 지시할 때는 영어.
