---
paths:
  - "src/services/**"
  - "src/hooks/**"
---

# 에러 처리 패턴

## API 호출: Result 타입 패턴
```typescript
interface Result<T> {
  success: boolean;
  data?: T;
  error?: {
    code: string;
    message: string;
  };
}

async function fetchUser(id: string): Promise<Result<User>> {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) {
      return {
        success: false,
        error: { code: 'FETCH_ERROR', message: response.statusText },
      };
    }
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    return {
      success: false,
      error: { code: 'NETWORK_ERROR', message: 'Network request failed' },
    };
  }
}
```

## React 컴포넌트: ErrorBoundary
- 페이지 단위 ErrorBoundary 배치
- fallback UI에 재시도 버튼 포함
- 에러 로깅 통합

## 원칙
- 외부 API 호출은 항상 Result 타입으로 래핑
- TanStack Query의 error 상태 적극 활용
- 사용자에게 보여줄 에러 메시지는 친화적으로
