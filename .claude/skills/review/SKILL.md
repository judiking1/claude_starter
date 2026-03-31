---
name: review
description: 코드 리뷰 — 현재 변경사항을 CLAUDE.md 규칙 기준으로 검사하고 개선점 제안
user-invocable: true
allowed-tools: Read, Glob, Grep, Bash, Agent
context: fork
---

# Code Review Skill

현재 변경사항을 CLAUDE.md 규칙 기준으로 리뷰합니다.

## 리뷰 체크리스트

### 1. 타입 안전성
- `any` 사용 여부
- 타입 가드 적절한 사용 여부
- `as` 단언 남용 여부
- 반환 타입 명시 여부

### 2. 성능
- `useState` 대신 `useRef` 사용 가능한 곳
- 불필요한 리렌더 유발 패턴
- 큰 리스트의 가상화 여부
- React.memo / useMemo / useCallback 적절성

### 3. 네이밍 & 컨벤션
- CLAUDE.md 네이밍 컨벤션 준수
- Import 순서
- 파일/폴더 구조 규칙 준수

### 4. 에러 처리
- API 호출의 Result 타입 래핑
- ErrorBoundary 배치

### 5. 접근성
- ARIA 속성
- 키보드 네비게이션
- alt 속성

## 출력 형식
한국어로 결과를 요약:
- **잘된 점**: 규칙을 잘 따른 부분
- **개선 필요**: 규칙 위반이나 개선 가능한 부분 (왜 개선해야 하는지 교육적 설명 포함)
- **제안**: 선택적 개선 아이디어
