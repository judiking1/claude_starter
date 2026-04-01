---
name: careful
description: 위험 명령 가드레일 — 파괴적 명령어 감지 시 경고 및 사용자 확인 요청
user-invocable: true
---

# Careful Skill — 위험 명령 가드레일

파괴적 명령어를 실행하기 전에 사용자에게 경고하고 확인을 받습니다.
`/careful`을 실행하면 현재 세션 동안 가드레일이 활성화됩니다.

## 보호 대상

### 파일 삭제
- `rm -rf`, `rm -r`, `--recursive` 플래그가 포함된 삭제 명령
- **안전 예외** (경고 없이 허용):
  - `node_modules`, `.next`, `dist`, `build`, `__pycache__`
  - `.cache`, `.turbo`, `coverage`, `.parcel-cache`
  - 이들은 빌드 산출물로 언제든 재생성 가능

### 데이터베이스
- `DROP TABLE`, `DROP DATABASE`, `TRUNCATE`
- 프로덕션 DB 연결 문자열이 포함된 명령
- 마이그레이션 롤백 (`migrate:rollback`, `migrate:reset`)

### Git 히스토리 변경
- `git push -f`, `git push --force`, `git push --force-with-lease`
- `git reset --hard`
- `git checkout -- .`, `git restore .` (수정된 파일 덮어쓰기)
- `git clean -fd`, `git clean -fdx`
- `git branch -D` (force delete)

### 컨테이너/인프라
- `docker rm -f`, `docker rmi -f`
- `docker system prune`, `docker volume prune`
- 프로덕션 환경 배포 명령

### 패키지 관리
- `pnpm store prune` (전역 캐시 삭제)
- major 버전 패키지 다운그레이드

## 작동 방식

가드레일이 활성화되면:

1. **패턴 감지**: Bash 명령 실행 전 위험 패턴 스캔
2. **경고 표시**: 어떤 위험이 있는지 구체적으로 설명
3. **확인 요청**: AskUserQuestion으로 진행 여부 확인
4. **사용자 승인 시만 실행**

```
⚠️ 위험 명령 감지: git reset --hard HEAD~3
→ 최근 3개 커밋의 변경사항이 영구 삭제됩니다.
→ 이 작업은 되돌릴 수 없습니다.
→ 계속하시겠습니까?
```

## 비활성화
`/careful`을 다시 실행하면 가드레일이 해제됩니다.
새 세션에서는 자동으로 비활성화 상태입니다.

## 한국어로 소통
모든 경고 메시지는 한국어로 표시합니다.
