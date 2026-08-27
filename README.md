# 라이즌푸드 공식 웹사이트

크림오브라이스 브랜드 사이트 + 관리자 시스템.

작업 기준 문서는 [CLAUDE.md](CLAUDE.md) 와 [docs/기획서.md](docs/기획서.md) 다.
디자인 레퍼런스는 [docs/prototype.html](docs/prototype.html).
히어로 연출 프로토타입은 [docs/prototype-hero.html](docs/prototype-hero.html) — 단일 파일이라 더블클릭하면 바로 열린다.

---

## 구성

저장소가 셋으로 나뉘어 있다. 이 디렉터리는 셋을 나란히 두고 작업하는 로컬 작업 공간이다.

| 디렉터리 | 저장소 | 내용 | 포트 |
|---|---|---|---|
| `FE/` | [RIZEN-FOOD/FE](https://github.com/RIZEN-FOOD/FE) | Next.js 15 · TypeScript · Tailwind v4 | 3000 |
| `BE/` | [RIZEN-FOOD/BE](https://github.com/RIZEN-FOOD/BE) | Spring Boot 3.5 · Java 21 · PostgreSQL · Flyway | 8080 |
| 루트 | [RIZEN-FOOD/.github](https://github.com/RIZEN-FOOD/.github) | 기획서 · 프로토타입 · 프로젝트 문서 | — |
| `docker-compose.yml` | (.github) | 로컬 개발용 PostgreSQL 16 | 5432 |

`BE/` 와 `FE/` 는 각각 독립된 저장소라 루트 저장소에서는 추적하지 않는다(`.gitignore`).

---

## 처음 한 번만

    cp .env.example .env

`.env` 를 열어 `POSTGRES_PASSWORD` 와 `DB_PASSWORD` 를 같은 값으로 바꾼다.
`.env` 는 커밋되지 않는다. 커밋되는 건 `.env.example` 뿐이다.

Java 를 따로 깔 필요는 없다. Gradle 이 JDK 21 을 자동으로 내려받는다.

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

### 3. 웹

    cd FE
    npm install     # 처음 한 번
    npm run dev

http://localhost:3000 — 현재는 디자인 토큰 확인용 화면이다 (Phase 4 에서 실제 메인으로 교체).

---

## 종료

    docker compose down          # 컨테이너만 내림 (데이터 유지)
    docker compose down -v       # 데이터까지 삭제

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
| `ink-faint` | `#9A8E85` | 캡션 |
| `slate` | `#4F5660` | 수치 강조 |
| `slate-deep` | `#383E47` | 다크 섹션 |
| `berry` | `#35406B` | 블루베리 |

사용 예: `bg-clay`, `text-ink-soft`, `border-line`, `max-w-wrap`

서체는 `font-en` (Archivo) / `font-kr` (Noto Sans KR) / `font-script` (Kaushan Script, 로고 전용).

공용 컴포넌트는 `FE/src/components/ui` — `Button`, `Container`, `SectionTag`, `Card`.

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
