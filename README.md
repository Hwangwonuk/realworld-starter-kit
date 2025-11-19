# ![RealWorld Example App](logo.png)

> ### Go + React codebase containing real world examples (CRUD, auth, advanced patterns, etc) that adheres to the [RealWorld](https://github.com/gothinkster/realworld) spec and API.

### [Demo](https://demo.realworld.io/)&nbsp;&nbsp;&nbsp;&nbsp;[RealWorld](https://github.com/gothinkster/realworld)

## 프로젝트 소개

**Conduit**는 Medium.com을 모델로 한 실제 운영 가능한 소셜 블로깅 플랫폼입니다. 이 프로젝트는 **바이브 코딩(Vibe Coding)** 방법론과 **아르민 로나허(Armin Ronacher)의 실용주의 철학**을 기반으로 구축되었으며, RealWorld 표준 사양을 준수합니다.

### 핵심 철학

- **바이브 코딩**: 직관적이고 빠른 개발을 통한 MVP 우선 접근
- **아르민 로나허 철학**: 단순함과 실용성을 중시하는 기술 스택
- **RealWorld 표준**: 검증된 사양을 통한 일관성 있는 구현

## 기술 스택

### 백엔드
- **Go**: 표준 라이브러리 기반 HTTP 서버 (프레임워크 없음)
- **SQLite**: 경량 임베디드 데이터베이스 (Plain SQL, ORM 없음)
- **JWT**: 토큰 기반 무상태 인증

### 프론트엔드
- **React**: 컴포넌트 기반 UI 프레임워크
- **TypeScript**: 타입 안전성 보장
- **Vite**: 빠른 개발 서버 및 빌드 도구
- **Tailwind CSS**: 유틸리티 우선 CSS 프레임워크
- **shadcn/ui**: 재사용 가능한 UI 컴포넌트 라이브러리
- **TanStack Query**: 서버 상태 관리 및 캐싱
- **TanStack Router**: 타입 안전한 라우팅

### 개발 도구
- **Docker**: 컨테이너 기반 개발 환경
- **Makefile**: 빌드 및 실행 자동화
- **GitHub Actions**: CI/CD 파이프라인 (선택적)

## 주요 기능

### 인증 및 사용자 관리
- JWT 기반 사용자 인증
- 회원가입 / 로그인 / 로그아웃
- 프로필 조회 및 수정

### 게시글 시스템
- 게시글 작성, 조회, 수정, 삭제 (CRUD)
- Markdown 기반 게시글 본문
- 태그 시스템
- 좋아요(Favorite) 기능
- 페이지네이션 지원

### 소셜 기능
- 사용자 팔로우/언팔로우
- 팔로우한 사용자의 게시글 피드
- 게시글 댓글 작성 및 삭제

### UI/UX
- 반응형 디자인
- 다크 모드 지원 (선택적)
- 일관된 디자인 시스템

## 아키텍처

### 백엔드 아키텍처 (Clean Architecture)

```
Presentation Layer (HTTP Handlers, Middleware)
       ↓
Application Layer (Use Cases, DTOs)
       ↓
Domain Layer (Entities, Business Rules, Interfaces)
       ↓
Infrastructure Layer (SQLite Repository, JWT Service)
```

### 프론트엔드 아키텍처

```
Pages → Components → UI Components (shadcn/ui)
  ↓
State Management (Context API + React Query)
  ↓
Services (API Client, Auth Service)
  ↓
Routing (TanStack Router)
```

## Getting Started

### 사전 요구사항

- **Go** 1.21 이상
- **Node.js** 18 이상
- **Docker** (선택적)

### 로컬 개발 환경 설정

#### 1. 저장소 클론

```bash
git clone https://github.com/yourusername/realworld-starter-kit.git
cd realworld-starter-kit
```

#### 2. 환경 변수 설정

```bash
# 백엔드 환경 변수
cp backend/.env.example backend/.env
# JWT_SECRET 등 필요한 값 설정

# 프론트엔드 환경 변수
cp frontend/.env.example frontend/.env
# VITE_API_URL 등 필요한 값 설정
```

#### 3. 백엔드 실행

```bash
cd backend
go mod download
go run cmd/migrate/main.go  # 데이터베이스 마이그레이션
go run cmd/server/main.go   # 서버 시작 (http://localhost:8080)
```

#### 4. 프론트엔드 실행

```bash
cd frontend
npm install
npm run dev  # 개발 서버 시작 (http://localhost:5173)
```

### Docker를 사용한 실행

```bash
# 개발 환경 시작
make docker-up
# 또는
docker-compose up -d

# 환경 종료
make docker-down
# 또는
docker-compose down
```

## 개발 명령어

### 백엔드

```bash
cd backend

# 개발 서버 실행
go run cmd/server/main.go

# 테스트 실행
go test ./...

# 테스트 커버리지 확인
go test -cover ./...

# 코드 포맷팅
go fmt ./...

# 린트 실행
golangci-lint run
```

### 프론트엔드

```bash
cd frontend

# 개발 서버 실행
npm run dev

# 프로덕션 빌드
npm run build

# 프리뷰 서버 실행
npm run preview

# 테스트 실행
npm test

# 린트 실행
npm run lint

# 코드 포맷팅
npm run format
```

### Makefile 명령어

```bash
make dev           # 개발 환경 시작
make build         # 프로덕션 빌드
make test          # 모든 테스트 실행
make migrate       # 데이터베이스 마이그레이션
make fmt           # 코드 포맷팅
make lint          # 린터 실행
make clean         # 환경 정리
```

## 프로젝트 구조

```
.
├── backend/
│   ├── cmd/
│   │   ├── server/      # 메인 서버 애플리케이션
│   │   └── migrate/     # 데이터베이스 마이그레이션
│   ├── internal/
│   │   ├── handlers/    # HTTP 핸들러
│   │   ├── models/      # 데이터 모델
│   │   ├── auth/        # 인증 로직
│   │   └── db/          # 데이터베이스 연결 및 쿼리
│   └── go.mod
├── frontend/
│   ├── src/
│   │   ├── components/  # 재사용 가능한 UI 컴포넌트
│   │   ├── pages/       # 라우트 레벨 컴포넌트
│   │   ├── hooks/       # 커스텀 React 훅
│   │   ├── services/    # API 클라이언트 및 유틸리티
│   │   └── types/       # TypeScript 타입 정의
│   └── package.json
├── docs/
│   ├── PRD.md          # 프로덕트 요구사항 문서
│   ├── design.md       # 시스템 설계 문서
│   └── tasks.md        # MVP 구현 체크리스트
├── docker-compose.yml
├── Makefile
└── README.md
```

## API 문서

API는 RealWorld 표준 사양을 따릅니다:

- 인증: `POST /api/users/login`, `POST /api/users`
- 사용자: `GET /api/user`, `PUT /api/user`
- 프로필: `GET /api/profiles/:username`
- 게시글: `GET /api/articles`, `POST /api/articles`, `GET/PUT/DELETE /api/articles/:slug`
- 댓글: `GET/POST /api/articles/:slug/comments`, `DELETE /api/articles/:slug/comments/:id`
- 태그: `GET /api/tags`

자세한 API 사양은 [RealWorld API 문서](https://realworld-docs.netlify.app/specifications/backend/endpoints/)를 참조하세요.

## 설계 문서

프로젝트의 상세한 설계 문서는 다음에서 확인할 수 있습니다:

- [PRD.md](docs/PRD.md) - 프로덕트 요구사항 문서
- [design.md](docs/design.md) - 시스템 아키텍처 및 설계
- [tasks.md](docs/tasks.md) - MVP 구현 체크리스트

## 테스트

### 백엔드 테스트

- 유닛 테스트: Go 표준 `testing` 패키지
- 통합 테스트: `httptest` 패키지 활용
- 테스트 데이터베이스: 인메모리 SQLite

### 프론트엔드 테스트

- 유닛/통합 테스트: Vitest
- 컴포넌트 테스트: React Testing Library
- E2E 테스트: Playwright (선택적)

## 배포

### Docker를 사용한 프로덕션 배포

```bash
# 프로덕션 빌드
docker-compose -f docker-compose.prod.yml build

# 프로덕션 환경 실행
docker-compose -f docker-compose.prod.yml up -d
```

### 환경 변수

#### 백엔드
- `PORT`: 서버 포트 (기본값: 8080)
- `JWT_SECRET`: JWT 서명용 시크릿 키
- `DATABASE_PATH`: SQLite 데이터베이스 파일 경로
- `CORS_ORIGIN`: 허용할 프론트엔드 도메인
- `LOG_LEVEL`: 로그 레벨 (DEBUG, INFO, WARN, ERROR)

#### 프론트엔드
- `VITE_API_URL`: 백엔드 API URL
- `VITE_APP_NAME`: 애플리케이션 이름

## 기여하기

버그 리포트, 기능 제안, 풀 리퀘스트를 환영합니다. 자세한 내용은 [CONTRIBUTING.md](CONTRIBUTING.md)를 참조하세요.

## 라이선스

MIT License

## 참고 자료

- [RealWorld 공식 문서](https://realworld-docs.netlify.app/)
- [RealWorld GitHub](https://github.com/gothinkster/realworld)
- [데모 사이트](https://demo.realworld.io/)

## 크레딧

이 프로젝트는 [RealWorld](https://github.com/gothinkster/realworld) 사양을 구현한 예제 애플리케이션입니다.

Built with [Vibe Coding](https://vibe-coding.com) methodology.
