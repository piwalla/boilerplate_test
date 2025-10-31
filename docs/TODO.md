- [x] `.cursor/` 디렉토리
  - [x] `rules/` 커서룰
  - [x] `mcp.json` MCP 서버 설정
  - [ ] `dir.md` 프로젝트 디렉토리 구조
- [x] `.github/` 디렉토리
- [ ] `.husky/` 디렉토리
- [x] `app/` 디렉토리
  - [x] `favicon.ico` 파일
  - [ ] `not-found.tsx` 파일
  - [ ] `robots.ts` 파일
  - [ ] `sitemap.ts` 파일
  - [ ] `manifest.ts` 파일
- [x] `supabase/` 디렉토리
- [x] `public/` 디렉토리
  - [x] `icons/` 디렉토리
  - [x] `logo.png` 파일
  - [x] `og-image.png` 파일
- [x] `tsconfig.json` 파일
- [x] `.cursorignore` 파일
- [x] `.gitignore` 파일
- [x] `.prettierignore` 파일
- [x] `.prettierrc` 파일
- [x] `eslint.config.mjs` 파일
- [x] `AGENTS.md` 파일

---

# Durian Store MVP 개발 TODO 리스트

> PRD v1.1 기준 작업 목록
> 기준 일자: 2025년 10월 31일

## 📋 데이터베이스 및 백엔드

- [x] Supabase 스키마 구성
  - [x] `products` 테이블 생성 (상품 정보)
  - [x] `profiles` 테이블 생성 (사용자 프로필)
  - [x] `orders` 테이블 생성 (주문 정보)
  - [x] `order_items` 테이블 생성 (주문 상품 내역)
  - [x] `event_logs` 테이블 생성 (이벤트 로그)
  - [x] `order_status` ENUM 타입 생성
  - [x] Storage 버킷 설정 (`uploads`)
- [ ] Server Actions 또는 API Routes 구현
  - [ ] 상품 조회 API (베스트 상품, 전체 상품)
  - [ ] 주문 생성 API (결제 전 주문 데이터 생성)
  - [ ] 주문 완료 처리 API (결제 승인 후 주문 상태 업데이트)
  - [ ] 이벤트 로그 기록 API (`purchase_completed` 이벤트)
  - [ ] 재고 차감 로직 (주문 완료 시)

## 🎨 메인 페이지 (MAIN-01, MAIN-02)

- [x] 메인 페이지 레이아웃 구성
  - [x] 헤더/네비게이션 바 (`components/Navbar.tsx` 존재)
  - [ ] 푸터 구성
- [ ] 베스트 상품 섹션 (MAIN-01)
  - [ ] Supabase에서 `is_best = true`인 상품 조회
  - [ ] 상품 카드 컴포넌트 (`components/product-card.tsx`)
  - [ ] 상품 카드에 이미지, 상품명, 가격 표시
  - [ ] 상품 카드 클릭 시 상품 상세 페이지로 이동
- [ ] 브랜드/원산지 소개 섹션 (MAIN-02)
  - [ ] 브랜드 스토리 텍스트 및 이미지 섹션
  - [ ] 원산지 특징 설명 섹션
- [ ] 반응형 디자인 적용 (모바일 우선)

## 📦 상품 상세 페이지 (PDP-01, PDP-02)

- [ ] 상품 상세 페이지 라우트 생성 (`app/products/[id]/page.tsx`)
- [ ] 상품 정보 표시 (PDP-01)
  - [ ] 상품 대표 이미지 표시
  - [ ] 상품명, 상세 설명, 판매 가격, 재고 수량 표시
  - [ ] Supabase에서 상품 데이터 조회
- [ ] 구매 액션 버튼 (PDP-02)
  - [ ] '장바구니 담기' 버튼 (MVP에서는 미구현, 버튼만 표시)
  - [ ] '바로 구매' 버튼
  - [ ] '바로 구매' 클릭 시 로그인 상태 확인
    - [ ] 로그인 상태: 결제 페이지로 이동
    - [ ] 비로그인 상태: Clerk 로그인 페이지로 이동

## 🔐 사용자 인증 (AUTH-01)

- [x] Clerk 기본 설정 (middleware.ts, layout.tsx)
- [x] Clerk-Supabase 사용자 동기화 (`app/api/sync-user`, `components/providers/sync-user-provider.tsx`)
- [x] 사용자 동기화 훅 (`hooks/use-sync-user.ts`)
- [x] 인증 상태에 따른 UI 처리
  - [x] 로그인/로그아웃 버튼 표시 (Navbar.tsx)
  - [x] 사용자 정보 표시 (UserButton)
  - [x] 한국어 로컬라이제이션 (layout.tsx에 koKR 적용)
- [ ] 로그인/회원가입 페이지 스타일링 (선택사항)
  - [ ] Clerk 컴포넌트 커스터마이징 (Durian Store 브랜드 색상 적용)

## 💳 결제 플로우 (PAY-01)

- [ ] Toss Payments SDK 설치 및 설정
  - [ ] `@tosspayments/payment-sdk` 또는 `@tosspayments/sdk` 설치
  - [ ] 환경 변수 설정 (Toss Payments 클라이언트 키)
- [ ] 결제 페이지 생성 (`app/checkout/page.tsx`)
  - [ ] 주문 상품 정보 표시
  - [ ] 배송 정보 입력 폼
    - [ ] 수령인 이름
    - [ ] 연락처
    - [ ] 배송지 주소
  - [ ] 최종 결제 금액 표시
  - [ ] Toss Payments 결제 버튼
- [ ] 결제 처리 로직
  - [ ] 주문 데이터 생성 (결제 전 `orders` 테이블에 `pending` 상태로 생성)
  - [ ] Toss Payments 결제창 호출
  - [ ] 결제 성공 콜백 처리
    - [ ] 주문 상태를 `paid`로 업데이트
    - [ ] `order_items` 테이블에 주문 상품 저장
    - [ ] `event_logs` 테이블에 `purchase_completed` 이벤트 기록
    - [ ] 재고 수량 차감
    - [ ] 주문 완료 페이지로 이동
  - [ ] 결제 실패 처리
    - [ ] 에러 메시지 표시
    - [ ] 이전 페이지로 이동 또는 재시도 옵션 제공
- [ ] 주문 완료 페이지 (`app/checkout/success/page.tsx`)
  - [ ] 주문 정보 요약 표시
  - [ ] 주문 번호 표시
  - [ ] 메인 페이지로 돌아가기 버튼

## 📊 이벤트 로그 및 통계 (LOG-01)

- [ ] 이벤트 로그 기록 기능
  - [ ] `purchase_completed` 이벤트 기록 (결제 완료 시)
  - [ ] 이벤트 로그에 주문 ID 포함 (`metadata` JSONB 필드)
- [ ] 구매 전환율 계산을 위한 데이터 확인
  - [ ] `event_logs` 테이블 데이터 조회 가능 여부 확인
  - [ ] Supabase 대시보드에서 수동 계산 가능 여부 확인

## 🎨 디자인 시스템 및 스타일링

- [ ] 브랜드 컬러 적용
  - [ ] Primary: `#064E3B` (짙은 녹색)
  - [ ] Accent: `#FACC15` (노란색)
  - [ ] Tailwind CSS 커스텀 컬러 설정 (`app/globals.css`)
- [ ] 공통 컴포넌트
  - [ ] 버튼 컴포넌트 스타일링 (shadcn/ui 기반)
  - [ ] 카드 컴포넌트 스타일링
  - [ ] 폼 컴포넌트 스타일링
- [ ] 반응형 디자인
  - [ ] 모바일 해상도 최적화
  - [ ] 태블릿 해상도 대응
  - [ ] 데스크톱 해상도 대응

## 🔧 인프라 및 배포

- [x] 기본 인프라 설정 (Phase 1 완료)
  - [x] Supabase 클라이언트 설정 (lib/supabase/)
    - [x] `clerk-client.ts` (Client Component용)
    - [x] `server.ts` (Server Component/Server Action용)
    - [x] `service-role.ts` (관리자 권한용)
    - [x] `client.ts` (공개 데이터용)
  - [x] TypeScript 설정 (tsconfig.json)
  - [x] Next.js 설정 (next.config.ts)
  - [x] Tailwind CSS 설정 (app/globals.css)
  - [x] shadcn/ui 컴포넌트 설치 (button, form, input, label, dialog, accordion, textarea)
- [ ] 환경 변수 설정
  - [ ] Toss Payments 클라이언트 키
  - [ ] Toss Payments 시크릿 키 (서버 사이드)
  - [x] Supabase 환경 변수 (코드에 설정됨, .env 파일 확인 필요)
  - [x] Clerk 환경 변수 (코드에 설정됨, .env 파일 확인 필요)
- [ ] Vercel 배포 설정
  - [ ] 프로젝트 연결
  - [ ] 환경 변수 설정
  - [ ] 배포 후 동작 확인

## ✅ 테스트 (비포 릴리즈 체크리스트)

- [ ] 결제 테스트
  - [ ] Toss Payments 샌드박스 환경에서 가상 결제 테스트
  - [ ] 결제 취소 테스트
  - [ ] 결제 실패 시나리오 테스트
- [ ] 디바이스 테스트
  - [ ] iPhone 해상도 테스트
  - [ ] Galaxy 해상도 테스트
  - [ ] UI 깨짐 현상 확인
- [ ] 인증 테스트
  - [ ] 회원가입 플로우 테스트
  - [ ] 로그인 플로우 테스트
  - [ ] 로그아웃 플로우 테스트
- [ ] 데이터 연동 테스트
  - [ ] Supabase 대시보드에서 상품 추가/수정/삭제
  - [ ] 웹사이트에 변경사항 즉시 반영 확인
  - [ ] 베스트 상품 노출 확인

## 📝 문서화

- [ ] README.md 업데이트
  - [ ] 프로젝트 개요
  - [ ] 설치 및 실행 방법
  - [ ] 환경 변수 설정 가이드
- [ ] API 문서화
  - [ ] Server Actions 또는 API Routes 명세
  - [ ] 데이터베이스 스키마 설명

## 🗑️ 정리 작업

- [ ] 테스트 페이지 제거 (선택사항)
  - [ ] `app/auth-test/` 제거 또는 리팩토링
  - [ ] `app/storage-test/` 제거 또는 리팩토링
- [ ] 불필요한 파일 정리
- [ ] 주석 및 로그 정리

---

## 참고사항

- **RLS 비활성화**: 모든 데이터 쓰기(CUD) 작업은 서버 사이드를 통해 처리
- **Supabase 관리**: 상품 데이터는 Supabase 대시보드에서 직접 관리
- **MVP 범위 외**: 장바구니, 마이페이지, 리뷰 기능은 제외
