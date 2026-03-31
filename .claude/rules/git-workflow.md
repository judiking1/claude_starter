# Git 워크플로우 상세

## 브랜치 전략 (간소화)
- `main`: 프로덕션 (항상 배포 가능 상태)
- `feat/기능명`: 기능 개발
- `fix/버그명`: 버그 수정
- `refactor/대상`: 리팩토링

## 커밋 컨벤션 (Conventional Commits)
```
feat: add user authentication with JWT
fix: resolve memory leak in WebSocket connection
refactor: extract API client into separate service
style: apply consistent spacing in dashboard layout
perf: optimize list rendering with virtualization
docs: update API endpoint documentation
chore: upgrade TanStack Query to v5
test: add integration tests for payment flow
```

## PR 규칙
- 제목은 커밋 컨벤션과 동일한 형식
- 본문에 Summary + Test Plan 포함
- Claude가 자동으로 PR 생성 및 관리
