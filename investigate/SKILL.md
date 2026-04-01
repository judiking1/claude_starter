---
name: investigate
description: 체계적 디버깅 — 근본 원인 분석 후 수정, 3-Hypothesis Rule, Scope Lock으로 안전한 버그 수정
user-invocable: true
---

# Investigate Skill — 체계적 디버깅

버그 수정 시 이 스킬을 실행합니다. 근본 원인 없이 수정을 시작하지 않습니다.

> **Iron Law: No fixes without root cause.**

## 트리거
- "debug this", "fix this bug", "왜 이게 안 돼", "에러 나"
- 에러 메시지, 스택 트레이스, 예기치 않은 동작 보고

## 워크플로우

### Phase 1: Root Cause Investigation
1. **증상 수집**: 에러 메시지, 스크린샷, 재현 단계 정리
2. **코드 읽기**: 관련 파일 탐색, 데이터 흐름 추적
3. **최근 변경사항**: `git log --oneline -20`으로 관련 커밋 확인
4. **재현**: 동일 조건에서 버그 재현 시도

### Phase 2: Pattern Analysis
알려진 버그 패턴과 매칭:
- **Race Condition**: 비동기 작업 순서 의존
- **Nil/Undefined Propagation**: optional chaining 누락, null check 부재
- **State Corruption**: Zustand 스토어 동시 업데이트, stale closure
- **Stale Cache**: TanStack Query 캐시 무효화 누락
- **Integration Failure**: API 스키마 불일치, CORS
- **Config Drift**: 환경변수 누락, tsconfig 불일치

`git log`으로 같은 파일의 반복 수정 이력 확인.

### Phase 3: Hypothesis Testing
1. 가설을 명시적으로 진술: "X 때문에 Y가 발생한다"
2. `console.log`, 단언문, 또는 단위 테스트로 가설 검증
3. **3-Hypothesis Rule**: 가설 3회 연속 실패 시 추측 반복을 중단
   - 아키텍처 리뷰로 전환하거나
   - 사용자에게 에스컬레이션
   - **절대 4번째 추측을 시도하지 않는다**

### Phase 4: Implementation
가설이 확인되면:
1. **Scope Lock 활성화**: 해당 디렉토리/모듈만 편집
2. **근본 원인 수정** (증상이 아닌 원인을 고친다)
3. **회귀 테스트 작성**: 같은 버그가 재발하지 않도록
4. **전체 테스트 실행**: 수정으로 인한 사이드 이펙트 확인

### Phase 5: Verification & Report
1. **원래 버그 재현 시도** → 해결 확인
2. 구조화된 디버그 리포트 출력:

```
## Debug Report
- **증상**: [사용자가 보고한 현상]
- **근본 원인**: [실제 원인]
- **수정 위치**: [파일:라인]
- **증거**: [가설 검증 결과]
- **회귀 테스트**: [테스트 파일 참조]
```

## Scope Control 규칙
- 수정이 **5개 파일 초과** 시 사용자에게 blast radius 알리고 확인
- 3회 이상 실패한 수정 시도는 **전부 revert** 후 재접근
- 수정할 수 없는 경우 "BLOCKED" 상태로 에스컬레이션

## 완료 상태
- **DONE**: 근본 원인 발견, 수정 적용, 회귀 테스트 작성, 전체 테스트 통과
- **DONE_WITH_CONCERNS**: 수정했으나 완전 검증 불가 (간헐적 버그 등)
- **BLOCKED**: 근본 원인 불명확, 에스컬레이션 필요

## 한국어로 소통
디버그 리포트와 중간 경과는 한국어. 코드 주석과 테스트는 영어.
