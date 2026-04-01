---
name: qa
description: QA 테스트 — Claude Preview 기반 체계적 QA, Health Score, 자동 수정, WTF-likelihood로 안전한 버그 수정
argument-hint: "URL or 'diff'"
user-invocable: true
---

# QA Skill — 체계적 QA 테스트

Claude Preview를 사용하여 웹 앱을 체계적으로 테스트하고 버그를 수정합니다.

## 사용법
- `/qa` — 현재 브랜치의 변경된 파일 기준 diff-aware QA
- `/qa http://localhost:5173` — 특정 URL 전체 QA
- `/qa quick` — 30초 스모크 테스트

## 테스트 티어
| 티어 | 범위 | 기본값 |
|------|------|--------|
| Quick | Critical + High만 | 명시적 요청 시 |
| Standard | + Medium | 기본 |
| Exhaustive | + Low/Cosmetic | 명시적 요청 시 |

## 워크플로우 (11단계)

### Phase 1: Initialize
- 개발 서버 확인 (preview_start 또는 기존 서버)
- QA 보고서 출력 디렉토리 생성

### Phase 2: Orient
- preview_screenshot으로 초기 상태 캡처
- preview_snapshot으로 페이지 구조 파악
- preview_console_logs로 초기 에러 확인
- 앱 네비게이션 구조 매핑

### Phase 3: Explore
페이지별 체계적 테스트:
- **시각적 검사**: 레이아웃, 정렬, 오버플로우
- **인터랙티브 요소**: 버튼, 링크, 폼 동작
- **폼 테스트**: 빈 값, 잘못된 값, 경계값
- **네비게이션**: 페이지 전환, 뒤로가기
- **상태 변화**: 빈 상태, 로딩, 에러, 오버플로우
- **반응형**: preview_resize로 모바일/태블릿 확인
- **콘솔 에러**: 매 인터랙션 후 preview_console_logs 확인

### Phase 4: Document Issues
각 이슈를 구조화하여 기록:
```
### ISSUE-NNN: [제목]
- **심각도**: Critical / High / Medium / Low / Cosmetic
- **재현 단계**: 1. ... 2. ... 3. ...
- **예상 결과**: ...
- **실제 결과**: ...
- **스크린샷**: [before/after]
```

### Phase 5: Wrap Up
- Health Score 계산 (아래 참조)
- Top 3 수정 우선순위 정리
- 콘솔 에러 요약

### Phase 6: Triage
심각도별 정렬, 티어에 따라 수정 대상 결정.

### Phase 7: Fix Loop
수정 가능한 이슈별:
1. **소스 찾기**: Grep/Glob으로 관련 파일 탐색
2. **최소 수정**: 리팩토링 없이 버그만 수정
3. **커밋**: `fix(qa): ISSUE-NNN — description`
4. **재검증**: preview_screenshot + preview_console_logs로 수정 확인
5. **분류**: verified / best-effort / reverted / deferred

### Phase 8: Self-Regulation (WTF-likelihood)
5건 수정 후 위험도 평가:

| 이벤트 | WTF 증가 |
|--------|----------|
| revert 발생 | +15% |
| 3개 파일 초과 수정 | +5% |
| 15건 이후 추가 수정 | +1%/건 |
| 관련 없는 파일 수정 | +20% |

**WTF > 20%이면 즉시 중단.** Hard cap: 50건.

### Phase 9: Final QA
수정된 페이지 재테스트, 최종 Health Score 계산.

### Phase 10: Report
결과를 사용자에게 한국어로 보고:
- 발견된 이슈 수
- 수정된 이슈 (verified/best-effort/reverted/deferred)
- Health Score 변화 (before → after)
- 남은 이슈 목록

### Phase 11: 스크린샷 공유
주요 수정 전/후 스크린샷을 사용자에게 공유.

## Health Score

| 카테고리 | 가중치 | 채점 |
|----------|--------|------|
| Console Errors | 15% | 0→100, 1-3→70, 4-10→40, 10+→10 |
| Broken Links | 10% | 0→100, 각 broken→-15 (min 0) |
| Visual | 10% | 100에서 심각도별 차감 (critical -25, high -15, medium -8, low -3) |
| Functional | 20% | 동일 |
| UX | 15% | 동일 |
| Performance | 10% | 동일 |
| Content | 5% | 동일 |
| Accessibility | 15% | 동일 |

## Diff-Aware 모드 (기본)
feature 브랜치에서 URL 없이 실행 시:
1. `git diff main...HEAD`로 변경 파일 분석
2. 변경 파일 → 영향받는 페이지/라우트 매핑
3. 로컬 서버 자동 감지 (port 5173, 3000, 4000, 8080)
4. 영향받는 페이지만 집중 테스트

## 핵심 규칙
1. 모든 이슈에 스크린샷 필수
2. 재현 가능한지 2회 확인
3. 개발자가 아닌 사용자 관점에서 테스트
4. 매 인터랙션 후 콘솔 확인
5. 수정 전 working tree clean 확인
6. 한 커밋에 한 수정
7. 회귀 발생 시 즉시 revert

## 한국어로 소통
QA 보고서와 이슈 설명은 한국어. 커밋 메시지와 코드는 영어.
