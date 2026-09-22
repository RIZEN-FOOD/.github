# 라이즌푸드 공식 웹사이트

크림오브라이스 브랜드 사이트 + 자사몰 커머스 + 관리자 시스템.
**운영 중** — [www.rizenfoods.com](https://www.rizenfoods.com)

작업 기준 문서는 [CLAUDE.md](CLAUDE.md) 와 [docs/기획서.md](docs/기획서.md) 다.
디자인 레퍼런스는 [docs/prototype.html](docs/prototype.html).
히어로 연출 프로토타입은 [docs/prototype-hero.html](docs/prototype-hero.html) — 단일 파일이라 더블클릭하면 바로 열린다.

> 규모 (2026-09-22, 소스에서 직접 센 값)
> 공개 화면 19 · 관리 화면 30 · API 135 · Flyway V34 · 테스트 145건(파일 33) · Java 197파일 17,877줄

---

## 화면

> ⚠️ 아래 캡처는 **2026-09-14** 로컬 화면이다. 2026-09-22 에 타이포그래피·간격·푸터·모바일 메뉴를
> 전면 재정비해서 **지금 화면과는 다르다.** 현재 모습은 [www.rizenfoods.com](https://www.rizenfoods.com) 을 본다.
> 전체 페이지 이미지는 [docs/screenshots](docs/screenshots) 에 있다.

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
| 5 | 조리법 | 전자레인지 조리 4단계 (왼쪽 제목 고정 + 오른쪽 2×2 바둑판) | 고정 문구 |
| 6 | Features | PC — 사진 고정 + 스크롤 크로스페이드 / 모바일 — 네모 카드 4장. 3번 수치는 DB 영양성분 | 고정 문구 + 영양성분 |
| 7 | 후기 | 최근 후기 3개 (첫 건을 크게, 나머지 둘을 옆에 쌓음) | 후기 |
| 8 | Q&A | 자주 묻는 질문 3개 (아코디언) | 고정 문구 |
| 9 | 공지사항 | 최근 공지 | 공지 |
| — | 팝업 | 기간·«오늘 하루 보지 않기» 설정 가능한 메인 팝업 | 관리자 **팝업** |
| — | 푸터 | 제품 사진 + 사업자정보·통신판매업 신고번호 | 사이트 설정 |

고정 문구는 전부 식품표시광고법 기준으로 검토·승인한 카피다 (효능·효과 표현 없음).
사진 자리 `FE/public/assets/features/feature-{1..4}.jpg` 는 새 사진을 넣으면 바로 바뀐다(없으면 기존 사진으로 대체).

### 상품 상세

| PC | 모바일 |
|---|---|
| <img src="docs/screenshots/product-detail-desktop.jpg" width="520" alt="상품 상세 PC"> | <img src="docs/screenshots/product-detail-mobile.jpg" width="220" alt="상품 상세 모바일"> |

갤러리 · 구매 패널(수량·장바구니·찜·외부 구매처) · 상세 설명(에디터) · **사진형 상세페이지**(관리자가 쌓은 사진·영상·글 블록이 틈 없이 이어진다) ·
**영양성분 / 원재료·표시사항은 전부 텍스트**(상품정보 고시 · 제조원/판매원 주소 · 포장재질 · 주의사항).

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
| `BE/` | [RIZEN-FOOD/BE](https://github.com/RIZEN-FOOD/BE) | Spring Boot 3.5 · Java 21 · PostgreSQL 16 · Flyway | 8080 |
| 루트 | [RIZEN-FOOD/.github](https://github.com/RIZEN-FOOD/.github) | 기획서 · 프로토타입 · CI 킷 · 프로젝트 문서 | — |
| `docker-compose.yml` | (.github) | 로컬 개발용 PostgreSQL 16 | 5432 |

`BE/` 와 `FE/` 는 각각 독립된 저장소라 루트 저장소에서는 추적하지 않는다(`.gitignore`).
서버 배포용 파일(Caddyfile · docker-compose.prod.yml · 자동 배포 스크립트)은 `BE/deploy/` 에 있다.

---

## 처음 한 번만

    cp .env.example .env

`.env` 를 열어 `POSTGRES_PASSWORD` 와 `DB_PASSWORD` 를 같은 값으로 바꾼다.
`.env` 는 커밋되지 않는다. 커밋되는 건 `.env.example` 뿐이다.

FE 를 3000 이 아닌 포트로 띄우면 `CORS_ALLOWED_ORIGINS` 에 그 주소를 더한다 (예: `http://localhost:3200`).

Java 를 따로 깔 필요는 없다. Gradle 이 JDK 21 을 자동으로 내려받는다.

### 결제

기본값은 목(mock) 결제다. `PAYMENT_PROVIDER` 로 실제 PG 를 고른다 — 프론트 재배포는 필요 없다.

| 값 | 설명 |
|---|---|
| `mock` | 기본값. 로컬·개발 전용. **운영 프로필에서는 아예 로드되지 않는다** |
| `nicepay` | 나이스페이 신모듈(Server 승인 + Basic 인증). **2026-09-21 이 방식으로 계약했다** |
| `portone` | 포트원 V2. 우리 나이스 계약은 포트원이 지원하지 않아 쓰지 않지만 코드는 남겨뒀다 |

    PAYMENT_PROVIDER=nicepay
    NICEPAY_CLIENT_ID=...       # 공개값. 브라우저가 결제창을 여는 데 쓴다
    NICEPAY_SECRET_KEY=...      # 서버 전용. 절대 프론트에 두지 않는다
    NICEPAY_CANCEL_PASSWORD=... # 상점관리자에 설정한 결제취소 비밀번호. 환불에 쓴다

키가 없으면 서버는 뜨되 결제만 «결제 준비 중» 으로 거절한다 (fail closed — 조용히 통과하는 경로를 두지 않는다).

### 간편 로그인

카카오·네이버 OAuth 를 붙였다. **네이버는 2026-09-21 검수를 통과했다.**

    OAUTH_REDIRECT_BASE=http://localhost:8080
    KAKAO_CLIENT_ID=... / KAKAO_CLIENT_SECRET=...
    NAVER_CLIENT_ID=... / NAVER_CLIENT_SECRET=...

같은 이메일의 기존 계정이 있어도 **자동으로 합치지 않는다.** 원래 방식으로 로그인하도록 안내한다(계정 가로채기 차단).
처음 온 사람은 약관·만 14세 동의 화면을 거쳐야 가입된다. 자세한 절차는 [BE/deploy/SOCIAL_LOGIN.md](BE/deploy/SOCIAL_LOGIN.md).

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
> 빌드 캐시가 깨진 것이다. 서버를 끄고, `ryzen\FE` 아래에서 돌던 node 프로세스를 모두 죽인 뒤
> `FE/.next` 를 지우고 다시 띄운다. 동기화·백신이 도는 폴더(OneDrive·바탕화면)에서 특히 잘 난다.

---

## 종료

    docker compose down          # 컨테이너만 내림 (데이터 유지)
    docker compose down -v       # 데이터까지 삭제

---

## 배포

서버 한 대다. **AWS Lightsail 4GB (서울)** 에 Caddy + Next(standalone) + Spring + PostgreSQL 16 을
Docker Compose 로 올린다. Cloudflare·Supabase·Vercel 은 쓰지 않는다(국내 접속 경로·운영 복잡도).

    main 에 푸시
      → GitHub Actions 가 이미지를 만들어 ghcr.io 에 올린다
      → 서버의 rizen-deploy.timer 가 2분마다 새 이미지를 확인하고 바꾼다
      → 헬스체크 실패 시 이전 이미지로 되돌리고, 그 이미지를 «나쁜 이미지» 로 적어 다시 받지 않는다

**서버에 GitHub 키를 두지 않는다.** 당기는 쪽이 서버다.
Caddy 가 HTTPS 인증서를 자동으로 발급·갱신한다. 배포 기록은 서버의 `deploy.log` 에 쌓인다.

환경변수는 저장소에 두지 않고 서버에서 `set-env.sh` 로 넣는다. 구성 파일과 절차는 [BE/deploy/README.md](BE/deploy/README.md).

> ⚠️ **DB 백업이 아직 한 번도 돌지 않았다.** `backup.sh` 는 있지만 버킷(`BACKUP_BUCKET`)이 비어 있고
> cron·systemd 예약도 걸려 있지 않다. 운영 중인 사이트라 이게 지금 가장 큰 구멍이다.

---

## 구현 현황

메인·상품·인증·마이페이지·관리자 + 자사몰 커머스(장바구니·주문·결제·위시리스트·후기·취소반품교환)까지 화면과 API 가 붙어 있다.

### 공개 (스토어) — 19면

- 메인(위 [화면](#화면) 참고), 상품 목록·상세, 후기, 공지, 1:1 문의
- 장바구니 → 주문·결제 → 주문 상세 → 취소·반품·교환 요청 (회원·비회원)
- **비회원 주문 조회** (`/orders/lookup`) — 헤더에서 바로 들어간다. 비회원은 마이페이지가 없다
- 마이페이지(주문·위시리스트·후기·문의·계정)
- 법정 페이지: 이용약관 · 개인정보처리방침 · 배송/교환/환불
- 푸터에 사업자정보·통신판매업 신고번호 게시 (`site_setting` 에서 읽음)

### 결제 흐름

- 금액은 서버가 상품 테이블로 다시 계산하고, **PG API 로 결제 상태·금액을 직접 조회해 대조한 뒤에만** 주문을 확정한다
- 결제창 이탈·실패 → 재고·쿠폰 수량 즉시 복구, 장바구니 유지 (장바구니는 결제 확정 때 비운다)
- 결제창만 열고 떠난 주문은 30분 뒤 자동 정리 — 실제로 결제가 끝난 건이면 취소하지 않고 확정한다
- 관리자 취소·반품 완료 → PG 환불 요청 (실패하면 상태·재고 변경도 롤백)
- 결제수단: 카드 · 카카오페이 · 네이버페이 · 토스페이 · 계좌이체

### 할인코드 (2026-09-22)

- 관리자가 코드·비율(%)/금액(원)·최대 할인·최소 주문금액·기간·전체 수량·1인 한도를 등록한다
- 수량은 재고와 같은 방식으로 **원자적으로** 뺀다 (`UPDATE ... WHERE used_count < total_quantity`)
- 1인 한도는 회원이면 회원 ID, 비회원이면 **전화번호 HMAC 해시**로 센다.
  전화번호는 임의 IV 로 암호화해 두어 암호문이 매번 달라 찾을 수 없기 때문에, 찾기 전용 해시를 따로 둔다
- 할인액도 서버가 다시 계산한다. 브라우저가 보낸 금액은 믿지 않는다

### 관리자 (`/admin`, 로그인 후 · 전 API `@PreAuthorize`) — 30면

- 주문 관리 · **송장 엑셀 일괄 등록**(3PL 이 보낸 파일을 그대로 올린다) · 취소·반품·교환
- 회원 관리 · 후기 관리 · 문의함 · 공지사항 · 사이트 설정 · 배송비 정책 · 관리자 계정
- 상품 관리 — 가격·할인가·재고·**품절 토글**·노출, 영양성분, 원재료·표시사항·상품정보 고시, 상세 설명(에디터), **사진형 상세페이지 블록**
- **메인 배너** — 상품별 메인 문구·서브 문구·메인 이미지·구성 이미지 4종·배경색·표시 순서·활성/비활성
- **페이지 배너** — 후기·공지 등 페이지 상단 배너 이미지
- **메인 팝업** · **메인 FEATURES 사진** · **할인코드**

### 남은 일

- **DB 백업을 실제로 돌리기** (위 [배포](#배포) 경고 참고) · **결제 취소 비밀번호 설정 후 환불 확인**
- 손님 화면에 송장번호·배송조회 표시 (지금은 «배송중» 글자만 보인다) · 배송완료 자동 처리 방식 결정
- PG 심사 자료 제출 · 가상계좌를 쓴다면 입금 대기·기한·미입금 자동 취소
- 장바구니·주문서 화면 정리 (PG 심사 중이라 미뤄뒀다) · Next 16 업그레이드 · CSP nonce
- 브라우니·피넛 상품 정보(출시 예정) · Features 섹션 사진 4장
- 카카오 비즈 앱 전환(이메일 필수 수집) · 카카오톡 채널 · 네이버 파트너스 신청

> 개인정보처리방침·이용약관·식품 표시사항의 문안은 초안이다. 게시 전 사업자가 검토·확정한다.

---

## 디자인 토큰

팔레트·서체·크기는 `FE/src/app/globals.css` 의 `@theme` 블록 한 곳에서만 정의한다.
값을 컴포넌트에 직접 적지 말고 토큰 이름을 쓴다.

### 색

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

라이트 전용이다. 크림색 지면이 브랜드의 일부라 다크 모드 반전을 넣지 않는다.

### 크기 (2026-09-22 정리)

그 전에는 메인 한 화면에만 제목 크기가 11가지였다. 크기가 많으면 위계가 아니라 나열이 된다.
**아래 다섯 단계 밖으로 나가지 마라.** 강조는 크기가 아니라 굵기·색으로 준다.

| 토큰 | 쓰임 | 범위 |
|---|---|---|
| `text-display` | 히어로 한 줄 | 44 → 64px |
| `text-title` | 페이지 제목 (h1) | 30 → 42px |
| `text-section` | 섹션 제목 (h2) | 26 → 34px |
| `text-lead` | 섹션 안의 큰 항목 | 20 → 26px |
| `text-sub` | 카드·항목 제목 (h3) | 18 → 21px |
| `text-body` | 본문 | 16px |
| `text-small` | 보조 설명·메타 | 14px |
| `text-caption` | 날짜·배지·꼬리표 | 13px ← **한글 하한** |

한글은 라틴보다 획이 많고 네모틀에 꽉 차서 같은 px 라도 작아 보이고 빨리 뭉개진다.
12px 은 영문 아이브로우에만 쓰고 한글엔 쓰지 않는다.
본문은 줄높이 1.75 · `word-break: keep-all` — 단어 중간에서 꺾이지 않게 한다.

### 모서리 · 움직임

| 토큰 | 값 | 용도 |
|---|---|---|
| `radius-press` | 6px | 누르는 것 — 버튼·입력칸·선택칩 |
| `radius-box` | 12px | 담는 것 — 카드·패널·사진 틀 |
| (full) | — | 배지·아바타만 |

전에는 2·3·4·6·8·12·16px 이 뒤섞여 있었다. 위 셋만 쓴다. 알약 모양 버튼은 버튼이 아니라 배지다.

움직임은 `--ease-out` (`cubic-bezier(0.16, 1, 0.3, 1)`) 하나로 통일한다.
`ease-in-out` 과 `linear` 는 쓰지 않는다. 시간은 `--dur-fast` 180ms · `--dur-base` 320ms · `--dur-slow` 700ms.
애니메이션은 `transform` 과 `opacity` 만 건드린다.

### 서체

- `font-kr` — **Pretendard Variable** (본문 한글, 동적 서브셋)
- `font-display` — **고운바탕 Gowun Batang** (제목 한글 세리프. 라틴 글리프도 들어 있어 영문 제목까지 한 서체로 간다)
- `font-en` — **Archivo** (라틴 UI). 숫자 강조 `.font-numeric` 도 Archivo

> 2026-09-18 에 Fraunces·Kaushan Script·Noto Sans KR 을 걷어냈다.
> Noto Sans KR 은 라틴 서브셋만 로드돼 한글에 기여가 0이었다.

### 컴포넌트

공용 컴포넌트는 `FE/src/components/ui` — `Button`, `Container`, `SectionTag`, `Card`, `Reveal`.

- `Button` — 종류 `dark` · `line` · `soft`, 크기 `lg` · `md` · `sm`.
  글자가 두 줄로 꺾이지 않는다(`whitespace-nowrap`). 꺾이면 라벨을 줄이거나 버튼을 넓힌다
- `Card` — 톤 `paper` · `tint` · `ink`
- `Reveal` — 화면에 들어올 때 떠오르는 등장. `IntersectionObserver` 를 쓴다(스크롤 이벤트 금지)

헤더·푸터·퀵메뉴는 라우트 그룹 템플릿(`FE/src/app/(main)/layout.tsx`, `(store)/layout.tsx`)이 그리고,
페이지는 내용만 그린다. 모바일 메뉴는 **헤더 드롭다운 + 아코디언**이다 — 사이드 드로어는 쓰지 않는다
(포탈·전체 오버레이·스크롤 잠금이 맞물려, 닫는 전환이 끊기면 투명한 막이 남아 화면이 안 눌렸다).

---

## 규칙

작업 전 [CLAUDE.md](CLAUDE.md) 를 읽는다. 특히:

- **식품 표시·광고 규제** — 크림오브라이스는 일반 식품이다. 효능·효과 표현은 위법이다.
- **법정 표시사항은 텍스트로** — 영양성분·원재료를 이미지에 넣지 않는다.
- **상품 하드코딩 금지** — 전부 DB 에서 온다.
- **금액은 서버가 계산한다** — 브라우저가 보낸 금액을 믿지 않는다. 재고·쿠폰 수량은 원자적으로 뺀다.
- **`.env` 커밋 금지** — `.env.example` 만 올린다.
- 브랜치를 따로 만들지 않는다. `main` 에 바로 반영하고, **기능 → 테스트 → 서버 반영**까지를 한 단위로 한다.

### 법적 책임

개인정보처리방침·이용약관·식품 표시사항의 **법적 책임 주체는 사업자(라이즌푸드)** 다.
이 저장소가 제공하는 문안은 초안이며, 게시 전 사업자가 검토·확정해야 한다.
