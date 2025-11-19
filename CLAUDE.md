# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

이 프로젝트는 **바이브 코딩(Vibe Coding)** 방법론과 **아르민 로나허(Armin Ronacher)의 추천 기술 스택**을 사용한 **RealWorld 앱 구현**입니다. Medium.com 클론인 소셜 블로깅 플랫폼을 구축하며, 실용적인 기술 선택으로 빠른 MVP 개발을 시연합니다.

### 핵심 원칙
- **바이브 코딩**: 완벽한 설계보다 작동하는 코드를 우선시하는 직관적이고 빠른 개발
- **아르민 로나허 철학**: 단순함 우선, 표준 라이브러리 사용, 실용적 접근
- **RealWorld 표준**: 확립된 API 사양을 따라 일관성 유지

## 아키텍처

### 백엔드 (Go)
- **프레임워크**: 표준 `net/http` 라이브러리와 커스텀 미들웨어 사용 (Gin/Echo 같은 프레임워크 사용 안 함)
- **데이터베이스**: 순수 SQL 쿼리를 사용하는 SQLite (ORM 사용 안 함)
- **인증**: JWT 토큰 기반 무상태 인증
- **구조**: `cmd/`와 `internal/` 디렉토리를 사용한 Clean Architecture 패턴

### 프론트엔드 (React + TypeScript)
- **빌드 도구**: Vite
- **스타일링**: Tailwind CSS + shadcn/ui 컴포넌트
- **라우팅**: TanStack Router (타입 안전한 라우팅)
- **상태 관리**: 인증은 Context API, 서버 상태는 TanStack Query
- **아키텍처**: 기능별 폴더를 가진 컴포넌트 기반 구조

### 데이터베이스 스키마
핵심 엔티티: `users`, `articles`, `comments`, `follows`, `favorites`, `tags`
- SQLite 제약조건에 맞게 설계
- 복잡한 관계보다 단순함에 집중

## 개발 명령어

### 초기 설정 (backend/frontend가 존재할 때)
```bash
# 백엔드 설정
cd backend
go mod tidy
go run cmd/migrate/main.go  # 데이터베이스 마이그레이션
go run cmd/server/main.go   # 서버 시작 (http://localhost:8080)

# 프론트엔드 설정
cd frontend
npm install
npm run dev  # 개발 서버 시작 (http://localhost:5173)
```

### 개발 워크플로우
```bash
# 개발 환경 시작
make dev  # 또는 docker-compose up (가능한 경우)

# 백엔드 개발
cd backend
go run cmd/server/main.go          # 서버 시작
go test ./...                      # 모든 테스트 실행
go test ./internal/handlers        # 특정 패키지 테스트 실행
golangci-lint run                  # 린트 실행

# 프론트엔드 개발
cd frontend
npm run dev                        # 개발 서버 시작
npm test                           # 테스트 실행
npm run build                      # 프로덕션 빌드
npm run lint                       # 린트 실행
```

## 구현 상태

현재 **계획 단계**이며 포괄적인 문서를 보유하고 있습니다:
- `docs/PRD.md` - 프로덕트 요구사항 문서
- `docs/design.md` - Mermaid 다이어그램을 포함한 시스템 설계
- `docs/tasks.md` - MVP 구현 체크리스트
- GitHub Issues #3-#8이 구현 단계를 추적합니다

## API 설계

RealWorld API 사양을 따릅니다:
- **인증**: `POST /api/users`, `POST /api/users/login`, `GET /api/user`
- **게시글**: `GET /api/articles`, `POST /api/articles`, `GET/PUT/DELETE /api/articles/:slug`
- **댓글**: `GET/POST /api/articles/:slug/comments`, `DELETE /api/articles/:slug/comments/:id`
- **프로필**: `GET /api/profiles/:username`, `POST/DELETE /api/profiles/:username/follow`

모든 응답은 일관된 에러 처리와 함께 표준 RealWorld JSON 형식을 따릅니다.

## 주요 구현 결정사항

### 백엔드 제약조건
- 외부 프레임워크 없음 - Go 표준 라이브러리 사용
- SQLite만 사용 - PostgreSQL/MySQL 복잡성 없음
- JWT는 클라이언트 사이드 저장 - 세션 관리 없음
- 미들웨어 체인: CORS → 로깅 → 인증 → 비즈니스 로직

### 프론트엔드 제약조건
- 복잡한 상태 관리 라이브러리 없음 (Redux/Zustand)
- 핵심 스택 외 최소한의 외부 의존성
- 상속보다 컴포넌트 조합
- 네이티브 HTML5 검증 + 수동 JS를 사용한 폼 처리

### 보안 모델
- bcrypt 비밀번호 해싱
- 24시간 만료 기간을 가진 JWT
- 개발용 CORS 설정
- Prepared statements를 통한 SQL 인젝션 방지

## 개발 방법론

### 백엔드 개발 가이드라인
- **테스트 주도 개발(TDD)**: 모든 백엔드 및 핵심 로직 구현은 TDD 원칙을 따라야 합니다
  - 실패하는 테스트를 먼저 작성
  - 테스트를 통과하는 최소한의 코드 구현
  - 테스트 커버리지를 유지하며 리팩토링
- **SOLID 원칙**: 단일 책임, 개방-폐쇄, 리스코프 치환, 인터페이스 분리, 의존성 역전 원칙 준수
- **Clean Architecture**: 명확한 관심사 분리를 가진 Clean Architecture 패턴으로 구현
  - 도메인 계층: 비즈니스 엔티티 및 규칙
  - 애플리케이션 계층: 유스케이스 및 애플리케이션 서비스
  - 인프라 계층: 외부 의존성 (데이터베이스, HTTP 등)
  - 프레젠테이션 계층: HTTP 핸들러 및 요청/응답 모델

### 진행 상황 추적 및 이슈 관리
- **커밋 단위 완성**: 각 커밋은 완전하고 작동하는 기능 증분을 나타내야 합니다
- **GitHub 작업**: 모든 GitHub 관련 작업(이슈, PR, 댓글)에 `gh` CLI를 항상 사용
  - **이슈 관리**: 이슈 생성, 업데이트, 댓글 작성에 `gh issue` 명령어 사용
  - **Pull Request 관리**: PR 생성 및 관리에 `gh pr` 명령어 사용
  - **저장소 작업**: 저장소 수준 작업에 `gh repo` 및 `gh api` 사용
- **이슈 상태 업데이트**: 중요한 커밋 후 해당 GitHub 이슈를 다음으로 업데이트:
  - `gh issue comment`를 사용하여 완료된 내용을 설명하는 진행 상황 댓글
  - 체크박스를 사용하여 관련 인수 조건을 완료로 표시
  - 추적을 위한 커밋 해시 참조 포함
- **이슈 종료 정책**: **변경사항을 커밋하기 전에 이슈를 절대 종료하지 마세요**
  - 관련된 모든 변경사항이 커밋되고 푸시된 후에만 이슈 종료
  - 모든 인수 조건이 충족되고 코드가 커밋된 경우에만 `gh issue close` 사용
  - 작업은 완료되었지만 아직 커밋되지 않은 경우, 완료 댓글을 추가하되 이슈는 열어둠

## 테스트 전략

### 백엔드 테스트
- `internal/` 패키지의 비즈니스 로직에 대한 유닛 테스트
- API 엔드포인트에 대한 통합 테스트
- 별도 SQLite 파일을 사용한 테스트 데이터베이스 격리
- **TDD 구현**: 구현 코드 전에 테스트 작성
- 핵심 비즈니스 로직에 대해 최소 80% 코드 커버리지

### 프론트엔드 테스트
- Vitest를 사용한 컴포넌트 테스트
- MSW(Mock Service Worker)를 사용한 API 통합 테스트
- 중요한 사용자 플로우에 대한 기본 E2E

## 파일 구조 예상

```
backend/
  cmd/
    server/     # 메인 애플리케이션 진입점
    migrate/    # 데이터베이스 마이그레이션 유틸리티
  internal/
    handlers/   # HTTP 핸들러
    models/     # 데이터 모델
    auth/       # 인증 로직
    db/         # 데이터베이스 연결 및 쿼리

frontend/
  src/
    components/ # 재사용 가능한 UI 컴포넌트
    pages/      # 라우트 레벨 컴포넌트
    hooks/      # 커스텀 React 훅
    services/   # API 클라이언트 및 유틸리티
    types/      # TypeScript 타입 정의
```

## 일반적인 개발 패턴

### 에러 처리
- 백엔드: 일관된 JSON 형식의 구조화된 에러 응답
- 프론트엔드: 사용자 피드백을 위한 에러 바운더리 + 토스트 알림

### API 통합
- 프론트엔드는 자동 JWT 헤더 주입이 있는 axios/fetch 사용
- 생성/삭제 작업에서 더 나은 UX를 위한 낙관적 업데이트

### 데이터베이스 패턴
- 데이터 접근을 위한 저장소 패턴
- 다중 테이블 작업을 위한 트랜잭션 래핑
- 모든 동적 쿼리에 대한 Prepared statements

## 의존성 철학

아르민 로나허의 접근 방식을 따릅니다:
- 외부 의존성 최소화
- 표준 라이브러리 솔루션 선호
- 중요한 가치를 제공하는 의존성만 추가
- 가능한 한 프레임워크 종속 회피

## MVP 집중 영역

구현 우선순위:
1. 인증 시스템 (로그인/회원가입)
2. 게시글 CRUD 작업
3. 기본 댓글 시스템
4. 필수 UI 컴포넌트
5. 통합 테스트

고급 기능(팔로우, 좋아요, 태그)은 빠른 MVP 제공을 위해 명시적으로 우선순위가 낮습니다.
