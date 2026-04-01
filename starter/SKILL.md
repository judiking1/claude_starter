---
name: starter
description: 새 프로젝트에 Claude Code Starter 템플릿 초기화 — CLAUDE.md, rules, docs를 현재 프로젝트에 복사
argument-hint: "project-name"
user-invocable: true
---

# Starter Skill — 프로젝트 템플릿 초기화

현재 프로젝트에 Claude Code Starter의 규칙 파일과 문서를 복사합니다.

> **참고**: Claude Code의 내장 `/init`과 이름 충돌을 피하기 위해 `/starter`로 명명되었습니다.

## 트리거
- `/starter my-project` — 프로젝트 이름 지정
- `/starter` — 프로젝트 이름 없이 (나중에 CLAUDE.md에서 수동 수정)

## 실행 순서

### Step 1: 템플릿 디렉토리 찾기
`~/.claude/skills/claude_starter/template/` 디렉토리를 찾습니다.

```
~/.claude/skills/claude_starter/
├── starter/SKILL.md       ← 이 파일
└── template/              ← 여기서 읽음
    ├── CLAUDE.md
    ├── .claude/
    │   ├── launch.json
    │   └── rules/ (7개 규칙 파일)
    └── docs/
        ├── usage-guide.md
        ├── quick-start.md
        ├── session-orchestration.md
        └── adr/000-template.md
```

### Step 2: 기존 파일 충돌 확인
현재 프로젝트에 이미 `CLAUDE.md`나 `.claude/rules/`가 있는지 확인합니다.
- 있으면: AskUserQuestion으로 덮어쓸지 확인
- 없으면: 바로 진행

### Step 3: 파일 복사
template/ 디렉토리의 모든 파일을 현재 프로젝트에 복사합니다.

**복사 대상:**
1. `CLAUDE.md` → 프로젝트 루트
2. `.claude/launch.json` → `.claude/launch.json`
3. `.claude/rules/*.md` (7개) → `.claude/rules/`
4. `docs/*.md` + `docs/adr/*.md` → `docs/`

**방법:**
- 각 파일을 Read tool로 읽은 후 Write tool로 현재 프로젝트에 생성
- 디렉토리가 없으면 Bash로 `mkdir -p` 실행

### Step 4: 프로젝트 이름 반영
인자로 프로젝트 이름이 주어졌으면:
- `CLAUDE.md`에서 `[PROJECT_NAME]`을 프로젝트 이름으로 교체

### Step 5: 완료 보고
한국어로 결과를 보고:
```
초기화 완료!

복사된 파일:
  ✓ CLAUDE.md (프로젝트 이름: my-project)
  ✓ .claude/rules/ (7개 규칙)
  ✓ .claude/launch.json
  ✓ docs/ (4개 문서)

다음 단계:
  1. CLAUDE.md에서 [PROJECT_DESCRIPTION]을 수정하세요
  2. /project-kickoff my-project 실행
```

## 주의사항
- 스킬은 글로벌로 설치되어 있으므로 별도 복사 불필요
- `.claude/skills/`는 복사하지 않음 (글로벌 스킬 사용)
- `.claude/settings.local.json`은 복사하지 않음 (환경별 자동 생성)

## 한국어로 소통
모든 안내와 보고는 한국어.
