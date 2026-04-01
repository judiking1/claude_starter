---
name: design-system
description: 디자인 시스템 구축 — 타이포그래피, 색상 팔레트, 컴포넌트 라이브러리, 간격/레이아웃 시스템 설정
argument-hint: "component-name or init"
user-invocable: true
---

# Design System Skill

디자인 시스템을 구축하거나 업데이트할 때 사용합니다.

## 사용법
- `/design-system init` — 디자인 시스템 초기 구축
- `/design-system Button` — 특정 컴포넌트 추가
- `/design-system audit` — 기존 컴포넌트 일관성 검사

## Init 모드

### 1. 디자인 토큰 정의
docs/design-direction.md를 참고하여:

**색상 팔레트**:
```css
/* src/styles/design-tokens.css */
:root {
  --color-primary: /* 프로젝트에 맞게 */;
  --color-secondary: ;
  --color-success: ;
  --color-warning: ;
  --color-error: ;
  --color-background: ;
  --color-surface: ;
  --color-text-primary: ;
  --color-text-secondary: ;
}

.dark {
  /* 다크모드 변수 오버라이드 */
}
```

**타이포그래피**:
- 폰트 패밀리 설정 (Inter 기본)
- 스케일: Hero(36) → H1(30) → H2(24) → H3(20) → Body(16) → Small(14) → Caption(12)
- line-height: heading 1.2, body 1.5

**간격**:
- Tailwind 기본 4px 단위 사용
- 컴포넌트 내부: p-4 (16px)
- 섹션 간: gap-6 ~ gap-8

### 2. 기본 UI 컴포넌트 생성
src/components/ui/ 에 다음 컴포넌트를 생성:
- Button (variants: primary, secondary, outline, ghost, danger / sizes: sm, md, lg)
- Input (variants: default, error / sizes: sm, md, lg)
- Modal (overlay + 애니메이션)
- Card (기본 컨테이너)
- Badge (status indicators)

### 3. 레이아웃 컴포넌트
src/components/layout/ 에 생성:
- Header
- Sidebar (필요 시)
- Footer
- PageContainer (max-width + padding)

### 4. 데모 페이지
모든 UI 컴포넌트를 한눈에 볼 수 있는 데모 페이지 생성.
Claude Preview로 시각적 확인 필수.

## 컴포넌트 추가 모드
기존 디자인 토큰과 패턴을 따라 새 컴포넌트 추가.
반드시 기존 컴포넌트와 일관된 API, 스타일, 접근성 유지.

## Audit 모드
기존 컴포넌트들의 일관성 검사:
- 동일한 Props 패턴 사용 여부
- 디자인 토큰 준수 여부
- 접근성 (ARIA, 키보드) 준수 여부
- 불필요한 스타일 중복 여부
