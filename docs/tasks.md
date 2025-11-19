# RealWorld MVP 구현 작업 목록

> **바이브 코딩 방식**: 빠르게 작동하는 MVP를 만들고 점진적으로 개선
> 핵심 기능에 집중하여 단시간에 완성하는 것이 목표

## 🎯 MVP 범위

**필수 기능만 구현:**
- ✅ 사용자 인증 (회원가입, 로그인)
- ✅ 게시글 CRUD (작성, 조회, 수정, 삭제)
- ✅ 기본 UI (홈페이지, 게시글 페이지)

**나중에 추가할 기능:**
- 댓글 시스템
- 팔로우/언팔로우
- 좋아요(Favorite)
- 태그 필터링
- 프로필 페이지

---

## Phase 1: 프로젝트 기초 설정

### 1.1 프로젝트 구조
- [ ] 저장소 디렉토리 생성 (`backend/`, `frontend/`)
- [ ] Go 프로젝트 초기화 (`go mod init`)
- [ ] React + TypeScript + Vite 초기화
- [ ] `.gitignore` 설정

### 1.2 백엔드 의존성
- [ ] SQLite 드라이버 설치
- [ ] JWT 라이브러리 설치 (`github.com/golang-jwt/jwt/v5`)
- [ ] bcrypt 설치 (`golang.org/x/crypto/bcrypt`)

### 1.3 프론트엔드 의존성
- [ ] Tailwind CSS 설치 및 설정
- [ ] TanStack Query 설치
- [ ] React Router 설치 (또는 TanStack Router)
- [ ] Axios 설치

---

## Phase 2: 데이터베이스 (최소 구성)

### 2.1 스키마 작성
- [ ] Users 테이블 (id, email, username, password_hash, bio, image, created_at)
- [ ] Articles 테이블 (id, slug, title, description, body, author_id, created_at, updated_at)

### 2.2 마이그레이션
- [ ] 마이그레이션 스크립트 작성 (`cmd/migrate/main.go`)
- [ ] 테이블 생성 SQL 작성
- [ ] 기본 인덱스 추가 (email, username, slug)

---

## Phase 3: 백엔드 핵심 기능

### 3.1 기본 인프라
- [ ] 데이터베이스 연결 설정
- [ ] HTTP 서버 설정 (`cmd/server/main.go`)
- [ ] CORS 미들웨어
- [ ] JWT 미들웨어 (인증 필요 시)

### 3.2 인증 시스템
- [ ] JWT 토큰 생성/검증 함수
- [ ] 비밀번호 해싱/검증 함수

### 3.3 사용자 API (인증)
- [ ] `POST /api/users` - 회원가입
  - [ ] 이메일, 사용자명, 비밀번호 검증
  - [ ] JWT 토큰 반환
- [ ] `POST /api/users/login` - 로그인
  - [ ] 이메일/비밀번호 검증
  - [ ] JWT 토큰 반환
- [ ] `GET /api/user` - 현재 사용자 정보 (인증 필요)
- [ ] `PUT /api/user` - 사용자 정보 수정 (인증 필요)

### 3.4 게시글 API (핵심)
- [ ] `GET /api/articles` - 게시글 목록
  - [ ] 페이지네이션 (limit, offset)
  - [ ] 최신순 정렬
- [ ] `GET /api/articles/:slug` - 게시글 상세
- [ ] `POST /api/articles` - 게시글 작성 (인증 필요)
  - [ ] 제목에서 slug 자동 생성
- [ ] `PUT /api/articles/:slug` - 게시글 수정 (인증 필요)
  - [ ] 작성자 권한 확인
- [ ] `DELETE /api/articles/:slug` - 게시글 삭제 (인증 필요)
  - [ ] 작성자 권한 확인

### 3.5 에러 처리
- [ ] RealWorld 표준 에러 형식 구현
  ```json
  {"errors": {"body": ["error message"]}}
  ```

---

## Phase 4: 프론트엔드 핵심 기능

### 4.1 기본 설정
- [ ] API 클라이언트 설정 (axios + base URL)
- [ ] React Query 설정
- [ ] React Router 설정
- [ ] Auth Context 생성 (JWT 토큰 관리)

### 4.2 레이아웃
- [ ] Header 컴포넌트
  - [ ] 비인증: Home, Login, Register 링크
  - [ ] 인증: Home, New Post, Settings, Profile 링크
- [ ] Footer 컴포넌트

### 4.3 인증 페이지
- [ ] 로그인 페이지 (`/login`)
  - [ ] 이메일, 비밀번호 입력
  - [ ] 로그인 API 호출
  - [ ] JWT 토큰 localStorage 저장
  - [ ] 성공 시 홈으로 리다이렉트
- [ ] 회원가입 페이지 (`/register`)
  - [ ] 사용자명, 이메일, 비밀번호 입력
  - [ ] 회원가입 API 호출
  - [ ] 자동 로그인

### 4.4 홈 페이지
- [ ] 홈 페이지 (`/`)
  - [ ] 게시글 목록 표시
  - [ ] 페이지네이션 (간단한 Next/Prev)
  - [ ] 게시글 클릭 시 상세 페이지 이동

### 4.5 게시글 페이지
- [ ] 게시글 작성 페이지 (`/editor`)
  - [ ] 제목, 설명, 본문 입력
  - [ ] 작성 API 호출
  - [ ] 성공 시 게시글 상세로 이동
- [ ] 게시글 상세 페이지 (`/article/:slug`)
  - [ ] 게시글 내용 표시 (Markdown 기본 렌더링)
  - [ ] 작성자만 수정/삭제 버튼 표시
- [ ] 게시글 수정 페이지 (`/editor/:slug`)
  - [ ] 기존 데이터 로드
  - [ ] 수정 API 호출

### 4.6 설정 페이지
- [ ] 설정 페이지 (`/settings`)
  - [ ] 사용자명, 이메일, bio, 이미지 URL 수정
  - [ ] 로그아웃 버튼

### 4.7 라우팅 보호
- [ ] Protected Routes (인증 필요: /editor, /settings)
- [ ] 비인증 시 /login으로 리다이렉트

---

## Phase 5: 테스트 및 문서

### 5.1 기본 테스트
- [ ] 백엔드 유닛 테스트 1개 이상
  - [ ] JWT 생성/검증 테스트 또는
  - [ ] 비밀번호 해싱 테스트

### 5.2 수동 테스트
- [ ] Postman/curl로 모든 API 엔드포인트 테스트
- [ ] 브라우저에서 전체 플로우 테스트
  - [ ] 회원가입 → 로그인 → 게시글 작성 → 조회 → 수정 → 삭제

### 5.3 문서화
- [x] README.md 업데이트 (이미 완료)
- [ ] API 엔드포인트 간단 정리 (README 또는 별도 파일)

---

## Phase 6: 완성도 높이기 (선택사항)

> MVP 완성 후 시간이 있다면 추가

### 6.1 추가 기능
- [ ] 댓글 시스템 (생성, 조회, 삭제)
- [ ] 좋아요 기능
- [ ] 태그 시스템
- [ ] 프로필 페이지
- [ ] 팔로우/언팔로우

### 6.2 개선 사항
- [ ] 로딩 스피너
- [ ] 에러 메시지 Toast
- [ ] 반응형 디자인 개선
- [ ] Markdown 에디터 개선
- [ ] 이미지 업로드 기능

---

## 🚀 빠른 시작 가이드

### 개발 순서 (추천)
1. **백엔드부터 시작**: 데이터베이스 → 인증 API → 게시글 API
2. **Postman으로 테스트**: API가 정상 작동하는지 확인
3. **프론트엔드 구현**: 로그인 → 홈 → 게시글 작성/조회
4. **통합 테스트**: 브라우저에서 전체 플로우 확인
5. **선택 기능 추가**: 시간이 있다면 댓글, 좋아요 등

### 예상 작업 시간 (참고용)
- Phase 1-2: 2-3시간
- Phase 3: 4-6시간
- Phase 4: 6-8시간
- Phase 5: 1-2시간
- **총 MVP 완성**: 15-20시간 예상

---

## 📊 체크리스트 요약

- **프로젝트 설정**: 0/7
- **데이터베이스**: 0/4
- **백엔드 API**: 0/13
- **프론트엔드**: 0/18
- **테스트/문서**: 1/4

**다음 작업**: Phase 1 프로젝트 기초 설정부터 시작

---

## 🔗 참고 자료

- [RealWorld API 스펙](https://realworld-docs.netlify.app/specifications/backend/)
- [프로젝트 PRD](./PRD.md)
- [프로젝트 설계](./design.md)

**마지막 업데이트**: 2025-11-19
