# Quick Start Guide

> 경험자를 위한 간결한 가이드. 상세 설명은 [usage-guide.md](usage-guide.md)를 참조하세요.

---

## 30초 시작

```bash
# 1. 복제
git clone https://github.com/judiking1/claude_starter.git my-project
cd my-project
rm -rf .git && git init

# 2. (선택) GitHub repo 연결
git add . && git commit -m "first commit"
git remote add origin https://github.com/내아이디/my-project.git
git branch -M main && git push -u origin main

# 3. CLAUDE.md 수정
#    [PROJECT_NAME] → my-project
#    [PROJECT_DESCRIPTION] → 프로젝트 설명

# 4. Claude Code 실행
claude
> /project-kickoff my-project
```

## 기존 repo에 추가

```bash
git clone https://github.com/judiking1/claude_starter.git /tmp/claude_starter
cp /tmp/claude_starter/CLAUDE.md .
cp -r /tmp/claude_starter/.claude .
cp -r /tmp/claude_starter/docs .
rm -rf /tmp/claude_starter
git add CLAUDE.md .claude docs && git commit -m "feat: add Claude Code boilerplate"
```

---

## 파일 구조

```
.claude/
├── rules/         # 조건부 자동 로드 (관련 파일 작업 시)
│   ├── typescript.md, components.md, performance.md
│   ├── error-handling.md, design-system.md
│   ├── git-workflow.md, safety.md
└── skills/        # 호출 시에만 로드 (/명령어)
    ├── project-kickoff, team-lead, design-system, review
    ├── investigate, careful, ship, qa, retro, plan-review
```

---

## 스킬 레퍼런스

### 개발 흐름
| 스킬 | 명령 | 핵심 |
|------|------|------|
| `/project-kickoff` | `/project-kickoff my-app` | Premise Challenge + Alternatives |
| `/plan-review` | `/plan-review` | Scope Challenge + Test Coverage Diagram |
| `/team-lead` | `/team-lead 기능설명` | 병렬 에이전트 + WTF-likelihood |
| `/design-system` | `/design-system init` | 토큰 + UI 컴포넌트 자동 생성 |

### 품질 관리
| 스킬 | 명령 | 핵심 |
|------|------|------|
| `/review` | `/review` | 7-Specialist Dispatch + Fix-First |
| `/qa` | `/qa` 또는 `/qa URL` | Health Score + 자동 수정 |
| `/ship` | `/ship` | 테스트→버전→CHANGELOG→PR |

### 디버깅 & 분석
| 스킬 | 명령 | 핵심 |
|------|------|------|
| `/investigate` | `/investigate` | Root Cause + 3-Hypothesis |
| `/careful` | `/careful` | 위험 명령 가드레일 |
| `/retro` | `/retro` 또는 `/retro 30d` | Git 기반 생산성 분석 |

---

## 추천 워크플로우

```
/project-kickoff → /design-system init → /plan-review → 코딩 → /review → /qa → /ship → /retro
```

---

## pnpm 명령어

```bash
pnpm install          # 의존성 설치
pnpm add <pkg>        # 패키지 추가
pnpm add -D <pkg>     # 개발 의존성 추가
pnpm dev              # dev 서버 시작
pnpm build            # 빌드
pnpm dlx <cmd>        # 일회성 실행
```
