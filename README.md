# 모바일 청첩장

정적 HTML 한 장짜리 모바일 청첩장입니다. 빌드 도구 없이 `index.html` 하나만 있으면 동작합니다.

## 폴더 구조

```
wedding-invitation/
├── index.html      # 기본 버전 (보라 · 민트)
├── wedding.html    # 버전 1. 웨딩 스타일 (화이트 · 핑크 · 퍼플 · 플라워)
├── modern.html     # 버전 2. 모던 스타일 (화이트 · 블랙 · 미니멀)
├── summer.html     # 버전 3. 여름 스타일 (화이트 · 하늘 · 민트 · 바다)
├── images/         # 사진 (직접 만들어서 넣기)
│   ├── main.jpg    # 커버 사진 (권장 1200×1800, 세로)
│   ├── 1.jpg ~ 5.jpg
│   └── og.jpg      # 카톡 공유 썸네일 (권장 1200×630, 가로)
└── README.md
```

---

## 0. 디자인 버전 3가지

내용(글·사진·연락처·계좌·달력·지도·BGM)과 기능은 **네 파일 모두 완전히 동일**하고, 색과 장식만 다릅니다.

| 파일 | 분위기 | 특징 |
|---|---|---|
| `wedding.html` | 화이트 · 핑크 · 퍼플 | 코너 플라워, 꽃 장식 라벨, 흩날리는 꽃잎, 세리프 이탤릭 영문 |
| `modern.html` | 화이트 · 블랙 | 장식 없음, 각진 카드, 넓은 자간, 블랙 푸터, 전화·문자 버튼은 단색 SVG 아이콘 |
| `summer.html` | 화이트 · 하늘 · 민트 | 섹션마다 파도 물결, 바다빛 그러데이션, 시원한 톤 |

**고르는 법** — 브라우저에서 세 파일을 직접 열어 비교한 뒤, 마음에 드는 파일을 `index.html` 로 바꾸면 됩니다.

```bash
cp index.html index-original.html   # 기존 버전 백업
cp summer.html index.html           # 예: 여름 버전 채택
```

> 배포 주소(`.../wedding/`)는 항상 `index.html` 을 보여줍니다. 나머지 파일도 같이 올려두면 `.../wedding/modern.html` 처럼 개별 주소로도 열 수 있어, 가족끼리 비교할 때 편합니다.

**주의** — 채택한 뒤 내용을 고칠 때는 **그 파일 하나만** 고치면 됩니다. 네 파일은 서로 연결돼 있지 않아서, 예를 들어 계좌번호를 `index.html` 에만 넣으면 나머지 파일에는 반영되지 않습니다. 헷갈리지 않게 **버전을 정한 뒤 안 쓰는 파일은 지우는 걸 권합니다.**

각 파일의 `<style>` 맨 아래 `THEME —` 주석 블록이 그 버전의 색·장식 정의입니다. 색만 손보고 싶다면 그 블록의 `:root` 값만 바꾸면 됩니다.

---

## 1. 내용 수정하기

### 1-1. 텍스트

`index.html`을 열고 아래 값들을 바꾸면 됩니다.

| 위치 | 바꿀 내용 |
|---|---|
| `<title>`, `og:*` 메타태그 | 이름·날짜·장소·공유 URL |
| `<header class="cover">` | 커버의 이름 / 날짜 / 장소 |
| `section.greeting` | 인사말, 혼주 성함 |
| `.contact-list` | 신랑·신부·혼주 전화번호 (`tel:`, `sms:` 둘 다) |
| `section.venue` | 예식장 이름·주소·전화·교통편 |
| `.acc` (마음 전하실 곳) | 계좌번호, `data-copy` 속성값도 같이 |
| `<script>`의 `CONFIG` | 예식 일시, 이름, 장소 (달력·D-day가 여기서 자동 계산됨) |

**중요**: 달력과 D-day는 `CONFIG.weddingDate` 하나만 고치면 자동으로 다시 그려집니다.

```js
const CONFIG = {
  weddingDate: '2026-11-07T13:00:00+09:00',
  groom: '김민준',
  bride: '이서연',
  venue: '그랜드홀 웨딩홀 3층 그랜드볼룸',
  kakaoJsKey: '',
};
```

### 1-2. 색상 / 폰트

`:root` 의 CSS 변수만 바꾸면 전체 톤이 한 번에 바뀝니다.

```css
:root {
  --bg:    #faf8f5;  /* 배경 */
  --ink:   #33302c;  /* 본문 글자 */
  --point: #a8836a;  /* 포인트 색 (달력 원, 라벨 등) */
}
```

### 1-3. 사진 넣기

**커버 사진** — CSS에서 주석 처리된 줄의 주석을 풀어주세요.

```css
.cover-photo { background-image: url('images/main.jpg'); }
```

**갤러리** — `<div class="ph ph1">PHOTO 1</div>` 를 `<img>` 로 교체합니다.

```html
<figure><img src="images/1.jpg" alt="" loading="lazy" /></figure>
```

> 사진은 업로드 전에 **1200px 이하 / 300KB 내외**로 압축하세요. 모바일 데이터로 여는 하객이 많습니다. (squoosh.app 등 사용)

### 1-4. 배경음악 넣기 (선택)

1. MP3 파일을 **`music/`** 폴더에 넣습니다.
2. `index.html` 의 `CONFIG.bgm` 을 그 파일 경로로 맞춥니다.
3. 끝. 오른쪽 위에 음표 버튼이 자동으로 나타납니다.

```js
bgm: 'music/leberch-invitation-wedding-375839.mp3',   // '' 로 두면 버튼이 안 보입니다
bgmVolume: 0.35,                                      // 음량 0 ~ 1
```

> 현재 곡: **Invitation Wedding** (leberch, Pixabay Content License — 출처 표기 불필요)

- **기본 상태는 '켜짐'** 입니다. 하객이 음표 버튼을 눌러 직접 끄기 전까지 계속 켜짐으로 유지됩니다. (다른 탭에 갔다 와도, 자동재생이 잠시 막혀도 '켜짐' 그대로)
- **자동재생은 브라우저가 막습니다.** (iOS·안드로이드·크롬 모두) 그래서 자동재생을 먼저 시도하고, 막히면 **하객이 화면을 처음 터치/스크롤할 때** 재생됩니다. 이 동안에도 버튼은 '켜짐'으로 보입니다.
- 파일이 없거나 재생할 수 없는 형식이면 버튼은 **자동으로 숨겨집니다.**
- 파일 크기는 **3MB 이하**를 권합니다. (128kbps · 2~3분 정도) 모바일 데이터로 여는 하객이 많습니다.
- 형식은 **MP3** 가 가장 안전합니다. (모든 브라우저 지원)
- 저작권 있는 곡을 웹에 올리는 건 문제가 될 수 있습니다. 유튜브 오디오 보관함, Pixabay Music, Bensound 등 **무료/저작권 프리 음원**을 쓰는 편이 안전합니다.

---

## 2. 지도 넣기 (선택)

`<div class="map-embed" id="map">` 안에 임베드 코드를 넣습니다.

**카카오맵 (가장 간단)**
1. https://map.kakao.com 에서 예식장 검색
2. 우측 `공유` → `지도 크게 보기` → HTML 태그 복사
3. 복사한 `<iframe>`/`<div>` 코드를 `map-embed` 안에 붙여넣기

**구글 지도** — 장소 검색 → 공유 → 지도 퍼가기 → HTML 복사 → 붙여넣기.

임베드가 번거로우면 지금처럼 지도 자리는 정적 이미지(캡처)로 두고, 아래 네이버지도/카카오맵/티맵 **바로가기 버튼만 실제 주소로 바꿔도 충분합니다.** 버튼의 검색어를 예식장 이름으로 교체하세요.

---

## 3. 카카오톡 공유 (선택)

기본 상태에서는 "카카오톡 공유" 버튼이 **기기 기본 공유 시트 → 링크 복사** 순으로 폴백하므로 그대로 써도 동작합니다. 카카오톡 전용 카드(썸네일+버튼)를 쓰려면:

1. https://developers.kakao.com → 애플리케이션 추가
2. **앱 키 → JavaScript 키** 복사 → `CONFIG.kakaoJsKey` 에 입력
3. **플랫폼 → Web → 사이트 도메인** 에 `https://USERNAME.github.io` 등록
4. `index.html` 맨 아래 카카오 SDK `<script>` 두 줄의 주석 해제

> JavaScript 키는 공개돼도 되는 키입니다(도메인으로 제한됨). REST API 키는 절대 넣지 마세요.

---

## 4. GitHub Pages 배포

### 방법 A. 웹에서만 (터미널 없이, 가장 쉬움)

1. GitHub 로그인 → 우상단 `+` → **New repository**
2. Repository name: `wedding` (원하는 이름), **Public** 선택 → Create
   - Private 저장소는 무료 플랜에서 Pages를 못 씁니다. **Public 필수**
3. 생성된 저장소에서 **Add file → Upload files**
4. `index.html` 과 `images/` 폴더를 드래그해서 올리고 **Commit changes**
5. **Settings → Pages** 이동
6. `Source` 를 **Deploy from a branch**, Branch 를 **`main` / `/ (root)`** 로 설정 → **Save**
7. 1~2분 뒤 상단에 URL이 뜹니다 → `https://USERNAME.github.io/wedding/`

### 방법 B. 터미널에서

```bash
cd /Users/subeenchang/Documents/subeen/wedding-invitation

git init
git add .
git commit -m "모바일 청첩장 초안"
git branch -M main

# GitHub에서 만든 빈 저장소 주소로 교체
git remote add origin https://github.com/USERNAME/wedding.git
git push -u origin main
```

푸시 후 **Settings → Pages → Deploy from a branch → main / (root) → Save**.

### 주소 형태 두 가지

| 저장소 이름 | 배포 주소 |
|---|---|
| `wedding` | `https://USERNAME.github.io/wedding/` |
| `USERNAME.github.io` | `https://USERNAME.github.io/` ← 짧은 주소 |

청첩장 링크는 짧을수록 좋으니, 이 저장소만 쓸 거라면 **`USERNAME.github.io`** 라는 이름으로 만드는 걸 추천합니다.

### 배포 후 반드시 할 일

`og:url` / `og:image` 메타태그를 **실제 배포 주소로** 바꿔야 카톡 미리보기가 나옵니다.

```html
<meta property="og:image" content="https://USERNAME.github.io/wedding/images/og.jpg" />
<meta property="og:url"   content="https://USERNAME.github.io/wedding/" />
```

- og 이미지는 **절대경로(https://…)** 여야 합니다. 상대경로는 카카오가 못 읽습니다.
- 한 번 공유된 링크는 카카오가 캐시합니다. 수정 후에는 [카카오 디버거](https://developers.kakao.com/tool/debugger/sharing)에서 **캐시 초기화**를 눌러주세요.

### 수정 후 재배포

파일 고치고 push(또는 웹에서 재업로드)하면 **1~2분 안에 자동 반영**됩니다. 안 바뀌어 보이면 브라우저 강력 새로고침(모바일은 시크릿 탭)으로 확인하세요.

---

## 5. 커스텀 도메인 (선택)

가비아 등에서 도메인을 샀다면:

1. DNS에 A 레코드 4개 추가 → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   (서브도메인이면 CNAME → `USERNAME.github.io`)
2. **Settings → Pages → Custom domain** 에 도메인 입력 → Save
3. **Enforce HTTPS** 체크 (인증서 발급까지 최대 24시간)

---

## 6. 배포 전 체크리스트

- [ ] 이름·날짜·시간·장소 전부 실제 값으로 교체
- [ ] 전화번호 `tel:` / `sms:` **둘 다** 수정 (한쪽만 고치는 실수 잦음)
- [ ] 계좌번호 표시 텍스트와 `data-copy` 값 **일치** 확인
- [ ] 사진 압축 후 교체, 커버 `background-image` 주석 해제
- [ ] `og:url`, `og:image` 를 실제 배포 주소로
- [ ] 실제 휴대폰에서 열어보기 — 전화 걸기, 계좌 복사, 지도 링크, 카톡 공유
- [ ] iOS Safari + Android Chrome 양쪽 확인

---

## 참고: 없는 기능

- **방명록 / 참석 여부(RSVP)** — 서버가 필요해 정적 페이지만으로는 불가능합니다.
  - 가장 쉬운 방법: **Google Forms** 를 만들어 "참석 여부 알려주기" 버튼으로 링크
  - 페이지 안에 넣고 싶다면 Firebase Firestore 또는 Supabase 무료 티어 연동
- **BGM** — 모바일 브라우저는 자동재생을 막으므로 재생 버튼이 필요합니다. 하객이 조용한 곳에서 열 수 있어 기본 제외했습니다.
