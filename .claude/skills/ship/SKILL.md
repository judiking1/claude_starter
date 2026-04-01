---
name: ship
description: 자동 배포 파이프라인 — 테스트, 버전, CHANGELOG, Bisectable Commits, PR 생성까지 한 번에
user-invocable: true
---

# Ship Skill — 자동 배포 파이프라인

`/ship`을 실행하면 feature 브랜치의 코드를 테스트부터 PR 생성까지 자동으로 처리합니다.
사용자가 "ship"이라고 했으면 ship한다. 불필요한 확인 단계를 최소화한다.

## 전제 조건
- feature 브랜치에서 실행 (main/master에서는 즉시 중단)
- 프로젝트에 테스트 프레임워크가 설정되어 있어야 함

## 파이프라인 (7단계)

### Phase 1: Pre-Flight
1. 현재 브랜치 확인 (main이면 중단)
2. uncommitted 변경사항 확인 → 있으면 자동 스테이징
3. `git fetch origin` 으로 최신 상태 동기화

### Phase 2: Merge & Test
1. base 브랜치(main) 병합
2. 충돌 발생 시 → 사용자에게 알리고 중단
3. 병합된 상태에서 테스트 실행: `pnpm test` 또는 `pnpm vitest run`
4. **테스트 프레임워크가 없으면**: Vitest + Testing Library 부트스트랩 제안

### Phase 3: Quality Gates
1. **테스트 통과 확인**: 실패 시 사용자에게 알리고 중단
2. **커버리지 감사** (테스트 프레임워크가 커버리지를 지원하는 경우):
   - 최소 60%, 목표 80%
   - 미달 시 부족한 코드 경로 식별 → 테스트 생성 제안
3. **인라인 리뷰**: `/review` 스킬의 핵심 체크 실행
   - SQL 안전성, 타입 안전성, 성능, 접근성
   - AUTO-FIX 가능한 것은 즉시 적용
   - ASK가 필요한 것은 배치 질문

### Phase 4: Version Bump
diff 크기와 변경 성격 기반 자동 판단:

| 조건 | 버전 | 사용자 확인 |
|------|------|------------|
| 50줄 미만, 사소한 변경 | PATCH (0.0.x) | 자동 |
| 50줄 이상, 일반적 변경 | PATCH (0.0.x) | 자동 |
| 500줄 이상 또는 새 모듈 | MINOR (0.x.0) | 확인 필요 |
| 브레이킹 체인지, 마일스톤 | MAJOR (x.0.0) | 확인 필요 |

`package.json`의 `version` 필드 업데이트.

### Phase 5: CHANGELOG
커밋 히스토리에서 자동 생성:
- **Added**: feat 커밋
- **Changed**: refactor, style, perf 커밋
- **Fixed**: fix 커밋
- **Removed**: 삭제된 기능

제품 릴리스 노트 스타일로 작성: "You can now..." / "Fixed an issue where..."
내부 인프라 변경은 "For contributors" 섹션에.

### Phase 6: Bisectable Commits
변경사항을 논리적 단위로 분리하여 커밋:
1. 인프라/설정 변경
2. 타입/인터페이스
3. 서비스/유틸
4. 컴포넌트/페이지
5. 테스트
6. 메타데이터 (CHANGELOG, 버전, 문서)

각 커밋은 독립적으로 빌드 가능해야 한다.

### Phase 7: Push & PR
1. `git push -u origin <branch>`
2. PR 생성 (gh pr create):

```markdown
## Summary
- [변경사항 요약 1-3줄]

## Quality Report
- Tests: ✅ X passed, 0 failed
- Coverage: XX% (delta: +X%)
- Review: X findings (X auto-fixed, X approved)

## Test Plan
- [ ] 수동 검증 항목 1
- [ ] 수동 검증 항목 2
```

## 실행이 중단되는 경우 (이것만 멈춤)
- main 브랜치에서 실행
- merge conflict 해결 불가
- 테스트 실패 (개발자가 수정해야 함)
- MINOR/MAJOR 버전 범프 확인
- 커버리지 최소치 미달

그 외 모든 것 — uncommitted 변경사항, PATCH 버전, CHANGELOG, 커밋 분리 — 은 자동 처리.

## 한국어로 소통
진행 상황과 결과는 한국어. PR 본문은 영어.
