# 라이즌푸드 공식 웹사이트

크림오브라이스 브랜드 사이트 + 관리자 시스템.

작업 기준 문서는 [CLAUDE.md](CLAUDE.md) 와 [docs/기획서.md](docs/기획서.md) 다.
디자인 레퍼런스는 [docs/prototype.html](docs/prototype.html).
히어로 연출 프로토타입은 [docs/prototype-hero.html](docs/prototype-hero.html) — 단일 파일이라 더블클릭하면 바로 열린다.

---

## 화면

> 2026-09-14 기준, 로컬 개발 서버 캡처. 전체 페이지 이미지는 [docs/screenshots](docs/screenshots) 에 있다.

### 메인

| PC | 모바일 |
|---|---|
| <img src="docs/screenshots/home-desktop.jpg" width="520" alt="메인 PC 전체 화면"> | <img src="docs/screenshots/home-mobile.jpg" width="220" alt="메인 모바일 전체 화면"> |

메인 구성 (위에서 아래로)

| # | 섹션 | 내용 | 데이터 |
|---|---|---|---|
| 1 | 히어로 캐러셀 | 상품별 배경색·누끼·기둥·장식 3종, 맛 아이콘 내비(쌀·초콜릿·땅콩), 품절·출시 예정 표시 | 관리자 **메인 배너** |
| 2 | 상품 | 메인 노출 상품 카드 | 관리자 상품 |
| 3 | Why RiZen | 브랜드 이야기(Rice + Risen) + 특징 4가지 | 고정 문구 |
| 4 | 영양성분 | 대표 상품 1회 제공량 기준 수치 (다크 사진 밴드) | 상품 영양성분 |
| 5 | 조리법 | 전자레인지 조리 4단계 | 고정 문구 |
| 6 | Features | PC — 사진 고정 + 스크롤 크로스페이드 / 모바일 — 네모 카드 4장. 3번 수치는 DB 영양성분 | 고정 문구 + 영양성분 |
| 7 | 후기 | 최근 후기 3개 | 후기 |
| 8 | Q&A | 자주 묻는 질문 3개 (아코디언) | 고정 문구 |
| 9 | 공지사항 | 최근 공지 | 공지 |
| — | 푸터 | 브랜드 배너 + 사업자정보·통신판매업 신고번호 | 사이트 설정 |

고정 문구는 전부 식품표시광고법 기준으로 검토·승인한 카피다 (효능·효과 표현 없음).
사진 자리 `FE/public/assets/features/feature-{1..4}.jpg` 는 새 사진을 넣으면 바로 바뀐다(없으면 기존 사진으로 대체).

### 상품 상세

| PC | 모바일 |
|---|---|
| <img src="docs/screenshots/product-detail-desktop.jpg" width="520" alt="상품 상세 PC"> | <img src="docs/screenshots/product-detail-mobile.jpg" width="220" alt="상품 상세 모바일"> |

갤러리 · 구매 패널(수량·장바구니·찜·외부 구매처) · 상세 설명(에디터) · **영양성분 / 원재료·표시사항은 전부 텍스트**(상품정보 고시 · 제조원/판매원 주소 · 포장재질 · 주의사항).

### 그 밖의 화면

| 상품 목록 | 장바구니 |
|---|---|
| <img src="docs/screenshots/products-desktop.jpg" width="420" alt="상품 목록"> | <img src="docs/screenshots/cart-desktop.jpg" width="420" alt="장바구니"> |
| **공지사항** | **로그인 · 회원가입** |
| <img src="docs/screenshots/notice-desktop.jpg" width="420" alt="공지사항"> | <img src="docs/screenshots/login-desktop.jpg" width="420" alt="로그인"> |
| **관리자 로그인** (`/admin/login`) | |
| <img src="docs/screenshots/admin-login-desktop.jpg" width="420" alt="관리자 로그인"> | |

---

## 구성

저장소가 셋으로 나뉘어 있다. 이 디렉터리는 셋을 나란히 두고 작업하는 로컬 작업 공간이다.

| 디렉터리 | 저장소 | 내용 | 포트 |
|---|---|---|---|
| `FE/` | [RIZEN-FOOD/FE](https://github.com/RIZEN-FOOD/FE) | Next.js 15 · TypeScript · Tailwind v4 · GSAP · Lenis | 3000 |
| `BE/` | [RIZEN-FOOD/BE](https://github.com/RIZEN-FOOD/BE) | Spring Boot 3.5 · Java 21 · PostgreSQL · Flyway | 8080 |
| 루트 | [RIZEN-FOOD/.github](https://github.com/RIZEN-FOOD/.github) | 기획서 · 프로토타입 · 프로젝트 문서 | — |
| `docker-compose.yml` | (.github) | 로컬 개발용 PostgreSQL 16 | 5432 |

`BE/` 와 `FE/` 는 각각 독립된 저장소라 루트 저장소에서는 추적하지 않는다(`.gitignore`).

---

## 처음 한 번만

    cp .env.example .env

`.env` 를 열어 `POSTGRES_PASSWORD` 와 `DB_PASSWORD` 를 같은 값으로 바꾼다.
`.env` 는 커밋되지 않는다. 커밋되는 건 `.env.example` 뿐이다.

FE 를 3000 이 아닌 포트로 띄우면 `CORS_ALLOWED_ORIGINS` 에 그 주소를 더한다 (예: `http://localhost:3200`).

Java 를 따로 깔 필요는 없다. Gradle 이 JDK 21 을 자동으로 내려받는다.

### 결제 (포트원)

기본값은 목(mock) 결제다. 포트원 콘솔에서 받은 값을 넣으면 실제 결제로 바뀐다. 프론트 재배포는 필요 없다.

    PAYMENT_PROVIDER=portone
    PORTONE_API_SECRET=...     # 서버 전용. 절대 프론트에 두지 않는다
    PORTONE_STORE_ID=...
    PORTONE_CHANNEL_KEY=...

---

## 실행

세 개를 순서대로 띄운다.

### 1. 데이터베이스

    docker compose up -d

준비될 때까지 헬스체크가 돈다. 상태 확인:

    docker compose ps

### 2. API

    cd BE
    ./gradlew bootRun

확인:

    curl -i http://localhost:8080/healthz
    # HTTP/1.1 200
    # {"status":"UP","time":"..."}

`bootRun` 은 레포 루트의 `.env` 를 읽어 환경변수로 넣는다.
`.env` 를 바꿨는데 반영이 안 되면 Gradle 데몬이 옛 값을 쥐고 있는 것이다 — `./gradlew --stop` 후 다시 띄운다.

### 3. 웹

    cd FE
    npm install     # 처음 한 번
    npm run dev

http://localhost:3000 — 실제 메인 페이지. 다른 포트는 `PORT=3200 npm run dev`.

> Windows 에서 개발 서버가 갑자기 500 을 내고 로그에 `UNKNOWN: unknown error, open '.next\...'` 가 보이면
> 빌드 캐시가 깨진 것이다. 서버를 끄고 `FE/.next` 를 지운 뒤 다시 띄운다.

---

## 종료

    docker compose down          # 컨테이너만 내림 (데이터 유지)
    docker compose down -v       # 데이터까지 삭제

---

## 구현 현황

메인·상품·인증·마이페이지·관리자 + 자사몰 커머스(장바구니·주문·결제·위시리스트·후기·취소반품교환)까지 화면과 API 가 붙어 있다.

### 공개 (스토어)

- 메인(위 [화면](#화면) 참고), 상품 목록·상세, 후기, 공지, 1:1 문의
- 장바구니 → 주문·결제 → 주문 상세 → 취소·반품·교환 요청 (회원·비회원)
- 마이페이지(주문·위시리스트·후기·문의·계정)
- 법정 페이지: 이용약관 · 개인정보처리방침 · 배송/교환/환불
- 푸터에 사업자정보·통신판매업 신고번호 게시 (`site_setting` 에서 읽음)

### 결제 흐름

- 금액은 서버가 상품 테이블로 다시 계산하고, **포트원 API 로 결제 상태·금액을 직접 조회해 대조한 뒤에만** 주문을 확정한다
- 결제창 이탈·실패 → 재고 즉시 복구, 장바구니 유지 (장바구니는 결제 확정 때 비운다)
- 결제창만 열고 떠난 주문은 30분 뒤 자동 정리 — 실제로 결제가 끝난 건이면 취소하지 않고 확정한다
- 관리자 취소·반품 완료 → 포트원 환불 요청 (실패하면 상태·재고 변경도 롤백)
- 결제수단: 카드 · 카카오페이 · 네이버페이 · 토스페이 · 계좌이체

### 관리자 (`/admin`, 로그인 후 · 전 API `@PreAuthorize`)

- 주문 관리 · 취소·반품·교환 · 회원 관리 · 후기 관리 · 문의함 · 공지사항 · 사이트 설정 · 배송비 정책
- 상품 관리 — 가격·할인가·재고·**품절 토글**·노출, 영양성분, 원재료·표시사항·상품정보 고시, 상세 설명(에디터)
- **메인 배너** — 상품별 메인 문구·서브 문구·메인 이미지·구성 이미지 4종·배경색·표시 순서·활성/비활성

### 미확정 · 남은 일

- **포트원 실키 테스트** — 키 수령 후 테스트 채널로 확인. 웹훅(즉시 확정)·가상계좌는 아직 없음
- 소셜 로그인(카카오·네이버), 우편번호 API, S3/CloudFront 이미지, 배포(Vercel + EC2)
- Features 섹션 사진 4장, 브라우니·피넛 상품 정보(출시 예정)
- 운영 전: 쿠키 Secure 강제, 요청 횟수 제한, 모바일 메뉴 포커스 가두기
- 이벤트·쿠폰은 테이블만 있고 화면·API 는 붙이지 않는다 (요건 확정 전)

> 개인정보처리방침·이용약관·식품 표시사항의 문안은 초안이다. 게시 전 사업자가 검토·확정한다.

---

## 디자인 토큰

팔레트·서체는 `FE/src/app/globals.css` 의 `@theme` 블록 한 곳에서만 정의한다.
색상 값을 컴포넌트에 직접 적지 말고 토큰 이름을 쓴다.

| 토큰 | 값 | 용도 |
|---|---|---|
| `clay` | `#DEB191` | 히어로 배경 |
| `clay-deep` | `#B87F5D` | 포인트 |
| `clay-soft` | `#E8C4A6` | 보조 배경 |
| `cream` | `#F4EFE6` | 기본 지면 |
| `cream-warm` | `#FAF7F1` | 헤더 |
| `paper` | `#FFFDF9` | 카드 |
| `ink` | `#221E1C` | 본문 |
| `ink-soft` | `#5A524C` | 보조 텍스트 |
| `ink-faint` | `#7C7067` | 캡션 (명도 대비 4.5:1 이상) |
| `danger` | `#B4321F` | 오류 문구 |
| `slate` | `#4F5660` | 수치 강조 |
| `slate-deep` | `#383E47` | 다크 섹션 · 푸터 |
| `berry` | `#35406B` | 블루베리 |

사용 예: `bg-clay`, `text-ink-soft`, `border-line`, `max-w-wrap`

서체(토큰):

- `font-kr` — Pretendard Variable (본문 한글, 동적 서브셋) → Noto Sans KR 폴백
- `font-display` — Fraunces (제목 세리프) → Pretendard 폴백
- `font-en` — Archivo (라틴 UI). 숫자 강조 `.font-numeric` 도 Archivo
- `font-script` — Kaushan Script (로고 전용)

공용 컴포넌트는 `FE/src/components/ui` — `Button`, `Container`, `SectionTag`, `Card`.
헤더·푸터·퀵메뉴는 라우트 그룹 템플릿(`FE/src/app/(main)/layout.tsx`, `(store)/layout.tsx`)이 그리고, 페이지는 내용만 그린다.

---

## 규칙

작업 전 [CLAUDE.md](CLAUDE.md) 를 읽는다. 특히:

- **식품 표시·광고 규제** — 크림오브라이스는 일반 식품이다. 효능·효과 표현은 위법이다.
- **법정 표시사항은 텍스트로** — 영양성분·원재료를 이미지에 넣지 않는다.
- **상품 하드코딩 금지** — 전부 DB 에서 온다.
- **`.env` 커밋 금지**
- **`main` 직접 커밋 금지** — `phase2/admin` 같은 브랜치를 쓴다.

### 법적 책임

개인정보처리방침·이용약관·식품 표시사항의 **법적 책임 주체는 사업자(라이즌푸드)** 다.
이 저장소가 제공하는 문안은 초안이며, 게시 전 사업자가 검토·확정해야 한다.
