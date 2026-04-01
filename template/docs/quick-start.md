# Quick Start Guide

> 경험자를 위한 간결한 가이드. 상세 설명은 [usage-guide.md](usage-guide.md)를 참조하세요.

---

## 글로벌 설치 (PC에 1번)

```bash
git clone --depth 1 https://github.com/judiking1/claude_starter.git ~/.claude/skills/claude_starter
cd ~/.claude/skills/claude_starter && ./setup
```

## 새 프로젝트 시작

```bash
mkdir my-project && cd my-project && git init
claude
> /starter my-project
> /project-kickoff my-project
```

## 기존 프로젝트에 추가

```bash
cd ~/projects/existing-project
claude
> /starter my-project
# 이후 터미널에서:
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
| `/learn` | `/learn` 또는 `/learn review` | 세션 간 학습 관리 |

---

## 추천 워크플로우

```
/project-kickoff → /design-system init → /plan-review → 코딩 → /review → /qa → /ship → /retro → /learn
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
