# NEO Lab 홈페이지 업데이트 가이드

이 홈페이지의 **내용(구성원, 장비, 소식, 논문)은 거의 다 `data/` 폴더 안의 JSON 파일**로 관리됩니다.
HTML이나 프로그래밍을 몰라도, **JSON 파일만 고치면 홈페이지가 바뀝니다.**

> 흐름은 항상 똑같아요: **① 파일 수정 → ② GitHub에 저장(Commit) → ③ 1~2분 뒤 홈페이지 반영 → ④ `Ctrl+Shift+R`로 새로고침**

---

## 시작하기 전에 — 꼭 알아둘 5가지

**1) 파일을 어떻게 고치나요?**
GitHub 저장소에서 파일을 클릭 → 오른쪽 위 **연필(✏️) 아이콘** → 내용 수정 → 아래 **Commit changes** 버튼을 누르면 끝이에요.
새 파일을 만들 때는 폴더에서 **Add file → Create new file**, 사진을 올릴 때는 **Add file → Upload files** 를 씁니다.

**2) JSON 문법 규칙 (이것만 지키면 됩니다)**

- 모든 글자값은 **큰따옴표 `" "`** 로 감쌉니다. (작은따옴표 ✗)
- 항목과 항목 사이는 **쉼표 `,`** 로 구분합니다.
- **맨 마지막 항목 뒤에는 쉼표를 넣지 않습니다.** (가장 흔한 실수예요)
- 여는 괄호 `{ [` 와 닫는 괄호 `} ]` 는 **짝이 맞아야** 합니다.

**3) 한글 이름도 파일명으로 그대로 쓸 수 있어요.** (예: `최유석.json`)

**4) 어딘가 문법이 틀리면 그 부분만 화면에 안 나옵니다.** (사이트 전체가 깨지진 않아요)
저장 전에 불안하면 [jsonlint.com](https://jsonlint.com) 에 붙여넣어 `Valid JSON` 인지 확인하세요.

**5) 순서는 대부분 `_index.json`(목록 파일)이 정합니다.** (뒤에서 자세히)

> **📝 글자 강조 서식** — 소식 제목/본문, 멤버 `note` 등에는 아래 서식을 쓸 수 있어요.
> `**굵게**` · `{red|빨강}` `{green|초록}` `{blue|파랑}` `{amber|주황}` `{gray|회색}` (또는 `{#D42A1A|직접색}`) · `[보이는 글자](https://링크)`

---

## 1. Members — 구성원 추가 / 수정  ⭐가장 자주 쓰는 부분

### 폴더 구조 한눈에 보기

```
data/members/
├── postdoc/          박사후연구원
├── msphd/            석·박사 통합과정
├── masters/          석사과정
├── undergraduate/    학부연구생
├── alumni/           졸업생
└── interns.json      단기 인턴 명단(한 파일)
```

각 그룹 폴더 안에는 두 종류의 파일이 있어요.

- `_index.json` … **누구를 어떤 순서로 보여줄지** 정하는 이름 목록
- `<이름>.json` … **그 사람의 실제 정보** (한 사람당 파일 하나)

> ※ **교수님(Professor)** 정보는 JSON이 아니라 `index.html` 파일에서 관리됩니다. (맨 아래 참고)

---

### ✅ 구성원 추가하기 (딱 3단계)

예시로 석사과정(`masters`)에 **홍길동**을 추가한다고 해볼게요.

**1단계 — 개인 정보 파일 만들기**
`data/members/masters/홍길동.json` 파일을 새로 만들고 아래 내용을 넣습니다.

```json
{
  "id": "홍길동",
  "ko": "홍길동",
  "en": "Gildong Hong",
  "init": "GH",
  "role": "M.S. Student",
  "admit": "2026-03",
  "email": "gildong@gm.gist.ac.kr"
}
```

**2단계 — 목록에 이름 추가하기**
`data/members/masters/_index.json` 을 열어, 원하는 위치에 이름을 추가합니다.
**여기 적힌 순서 그대로 홈페이지에 표시돼요.**

```json
[
  "윤성일",
  "최유석",
  "홍길동"
]
```

**3단계 — (선택) 사진 넣기**
`images/members/` 폴더에 사진을 올립니다. 자세한 규칙은 아래 "사진 넣기"를 보세요.
사진이 없으면 이니셜(`init`) 동그라미로 자동 표시됩니다.

이게 끝이에요. 저장(Commit)하면 곧 홈페이지에 나옵니다.

---

### 정보 항목(필드) 설명

**꼭 필요한 항목**

| 항목 | 뜻 | 예시 |
|------|------|------|
| `id` | 내부 식별용 이름. 보통 **파일명과 똑같이** 한글 이름 | `"홍길동"` |
| `ko` | 화면에 보일 한글 이름 | `"홍길동"` |
| `en` | 영문 이름 (사진 파일명 자동 생성에도 쓰임) | `"Gildong Hong"` |
| `init` | 사진이 없을 때 표시할 이니셜 | `"GH"` |

**필요할 때만 넣는 항목**

| 항목 | 뜻 | 예시 |
|------|------|------|
| `role` | 직함. 없으면 그룹 기본값이 표시됨 | `"M.S. Student"` |
| `admit` | 입학 시기 `"연도-월"`. 넣으면 **“N학기차”가 자동 계산**됨 | `"2025-09"` |
| `email` | 이메일 주소 | `"abc@gm.gist.ac.kr"` |
| `img` | 사진 파일명(직접 지정). 생략하면 `en`으로 자동 | `"gildong.jpg"` |
| `tags` | 역할 배지(여러 개 가능) | `["Lab leader"]` |
| `note` | 이름 아래 한 줄 부가 설명 (강조 서식 사용 가능) | `"Co-advisor: [Prof. Song](https://...)"` |
| `cv` | 이력서 PDF 파일명(`files/cv/`에 저장) | `"gildonghong.pdf"` |
| `education` | 학력 목록(상세 페이지) | `["Ph.D., GIST (2020–2024)"]` |
| `experience` | 경력 목록(상세 페이지) | `["Intern, ○○ (2019)"]` |
| `pubs` | 대표 논문 목록 | 아래 참고 |
| `awards` | 수상 목록 | `["Best Poster Award (2025)"]` |

**졸업생(`alumni`)에서 추가로 쓰는 항목**

| 항목 | 뜻 | 예시 |
|------|------|------|
| `period` | 재학 기간 | `"2023.03 - 2025.02"` |
| `now` | 현재 소속 (없으면 빈칸 `""`) | `"SK hynix"` |

`pubs`(대표 논문)는 이렇게 생겼어요.

```json
"pubs": [
  {
    "y": "2025",
    "a": "<b>G. Hong</b>, D.-H. Kang*",
    "t": "논문 제목을 여기에",
    "v": "Nature Electronics 8, 100–110"
  }
]
```
- `y` 연도 · `a` 저자(본인은 `<b> </b>`로 굵게) · `t` 제목 · `v` 저널·권·페이지

> **💡 상세 페이지는 자동으로 채워지는 부분이 있어요.**
> 멤버 카드를 누르면 열리는 상세 페이지에는, 위 내용에 더해
> **논문·특허**(`ref.bib`·`patents.json`의 저자 이름에서 그 멤버를 자동 감지)와
> **수상·과제**(`award.json`·`grant.json`의 `members` 목록에 그 멤버 `id`가 있으면)가 자동으로 붙습니다.

---

### 사진 넣기

- 사진은 **`images/members/`** 폴더에 올립니다.
- 파일 이름 규칙은 둘 중 하나예요.
  1. 개인 JSON에 `"img": "파일명.jpg"` 로 **직접 지정**하거나,
  2. 지정하지 않으면 **`en`(영문 이름) 기준으로 자동**으로 찾습니다 → **성 + 이름을 붙여 소문자, 공백·기호 제거**.
     예) `"Haein Cho"` → `haeincho.jpg`, `"Haneul Go"` → `gohaneul.jpg`
- 사진이 없으면 이니셜(`init`) 동그라미가 대신 나옵니다.
- 권장: **세로형(3:4 비율)** 인물 사진.

---

### 순서 바꾸기 · 내보내기(삭제)

- **순서 변경**: 해당 그룹의 `_index.json` 안에서 이름 순서를 바꾸면 됩니다.
- **화면에서 빼기**: `_index.json`에서 그 이름만 지우면 홈페이지에 안 나옵니다. (개인 JSON 파일은 남겨둬도 괜찮아요 — 나중에 다시 넣기 편함)
- **졸업 처리**: `masters` 등에서 이름을 빼고 `alumni/_index.json`에 추가한 뒤, 개인 파일을 `alumni` 폴더로 옮기고 `period`·`now`를 채우면 됩니다.

---

### 인턴 명단 — `data/members/interns.json`

인턴은 개인 파일 없이 **연도별 명단** 한 파일로 관리해요.

```json
[
  {
    "year": "2026",
    "people": [
      { "name": "정동현", "note": "군휴학" },
      { "name": "최재우" }
    ]
  }
]
```
- `year` 연도 · `people` 그 해 인턴들 · 각 사람은 `name`(이름)과 필요하면 `note`(비고).

---

### 교수님(Professor) 정보

교수님 소개는 JSON이 아니라 **`index.html`** 안의 `Professor` 부분에서 직접 고칩니다.
사진은 `images/members/donghokang.jpg` 예요. 이 부분 수정이 필요하면 편하게 요청하세요.

---

## 2. Facility — 연구 장비

**파일: `data/research/facility.json`**

장비는 **카테고리(구역)별로 묶여** 있어요. 구조는 이렇습니다.

```json
[
  {
    "cat": "Facilities in NEO Lab",
    "cards": [
      { "img": "B1500A", "name": "High-Precision Source-Meter Units", "desc": "Keysight B1500A" },
      { "img": "Pulse",  "name": "Pulse Measurement Setup", "desc": "Keysight B2912B" }
    ]
  }
]
```

- `cat` … 구역 이름(제목)
- `cards` … 그 구역의 장비들
  - `img` … 장비 사진 파일명. **`images/facility/` 폴더**에 넣습니다. 확장자는 생략 가능(예: `"B1500A"` → `B1500A.jpg`나 `.png`를 자동으로 찾음).
  - `name` … 장비 이름
  - `desc` … 모델명 등 설명 (없으면 빈칸 `""`, 사진 없으면 회색 아이콘 표시)

**장비 추가**: 원하는 구역의 `cards` 안에 `{ "img": ..., "name": ..., "desc": ... }` 한 줄을 더하면 됩니다.
**새 구역 추가**: 맨 바깥 대괄호 `[ ]` 안에 `{ "cat": ..., "cards": [ ... ] }` 블록을 하나 더 넣으면 됩니다.

---

## 3. News — 소식

News는 크게 **① 짧은 소식 게시판 3종**과 **② 보도자료(긴 기사)** 로 나뉩니다.

### ① 짧은 소식 게시판 — 논문 / 수상 / 과제

세 파일이 각각의 게시판이에요. 형식은 셋 다 똑같습니다.

- `data/news/paper.json` … 논문 게재·억셉 소식
- `data/news/award.json` … 수상 소식
- `data/news/grant.json` … 연구과제 선정 소식

```json
[
  {
    "id": "2026-07-08-nano",
    "posted": "2026-07-08",
    "title": "Nano Letters (JCR <15%, IF = 9.1), Accepted - Hyeonchang Son"
  }
]
```

- `id` … 겹치지 않는 고유값(보통 `날짜-키워드`)
- `posted` … 날짜 `"연도-월-일"` (이 날짜로 자동 정렬됩니다)
- `title` … 표시할 내용 (강조 서식 사용 가능 — 예: `{red|Advanced Materials}`)
- (선택) `members` … 관련 구성원 `id` 목록. 넣으면 **그 사람의 상세 페이지에도 이 수상/과제가 자동으로 연결**돼요.

```json
{
  "id": "2025-11-03-kps-award",
  "posted": "2025-11-03",
  "title": "2025 한국물리학회 추계학술대회 우수발표상 - 손현창",
  "members": ["손현창"]
}
```

**소식 추가**: 해당 파일의 대괄호 `[ ]` 안에 위 형식의 `{ }` 블록을 하나 더 넣으면 됩니다. (위치는 상관없어요 — 날짜순으로 자동 정렬)

### ② 보도자료(긴 기사) — `data/news/media/` 폴더

언론 보도처럼 사진과 여러 문단이 있는 큰 기사는 이 폴더에서 관리해요.
`_index.json`(목록) + 기사별 파일 하나씩 구조입니다. (Members와 같은 방식)

**기사 파일 예시** (`data/news/media/2025-11-12-af-synapse.json`)

```json
{
  "id": "2025-11-12-af-synapse",
  "posted": "2025-11-12",
  "term": "Paper Published",
  "title": "GIST-경북대, 뇌처럼 학습하는 AI 반도체 기술 개발",
  "hero": "2025-11-12-af-synapse.jpg",
  "blocks": [
    { "p": "첫 번째 문단 내용을 여기에 씁니다." },
    { "p": "두 번째 문단. [헤럴드경제](https://example.com) 처럼 링크도 됩니다." }
  ]
}
```

- `term` … 작은 분류 라벨(예: `"Paper Published"`)
- `title` … 기사 제목
- `hero` … 대표 이미지 파일명. **`images/news/` 폴더**에 넣습니다.
- `blocks` … 본문 문단들. 각 문단은 `{ "p": "..." }`. (여기서도 강조·링크 서식 사용 가능)

**추가 시 주의**: 기사 파일을 만든 뒤 **반드시 `data/news/media/_index.json` 목록에 그 `id`를 추가**해야 화면에 나옵니다.

```json
[
  "2025-11-12-af-synapse",
  "2025-08-18-am-in-sensor"
]
```

---

## 4. Publications — 논문 · 특허

### ① 학술지 논문 — `data/publications/ref.bib`

논문 목록은 **BibTeX** 형식 파일이에요. `@article{ ... }` 블록 하나가 논문 하나입니다.

```bibtex
@article{hong2026x,
  title={Bidirectional optical synapse for neuromorphic vision},
  author={Hong, Gildong and Park, Soeun and Kang, Dong-Ho},
  author+an={3=corresponding},
  journal={Advanced Functional Materials},
  volume={36},
  number={12},
  pages={100--110},
  year={2026},
  keyword={시냅스},
  doi={10.1002/adfm.2026xxxxx}
}
```

- `@article{` 바로 뒤의 **`hong2026x`가 “인용키”** 예요. **PDF·표지 파일 이름에 이 키를 씁니다.**
- 주요 항목: `title`(제목) · `author`(저자) · `journal`(저널) · `volume`/`number`/`pages` · `year`(연도) · `doi` · `keyword`(분류 태그)
- `author+an={3=corresponding}` … **3번째 저자가 교신저자**라는 뜻
- **연구실 멤버 이름은 자동으로 굵게** 표시됩니다.
- **진행 중인 논문**은 `year={In Progress}` 로 하면 목록 맨 위에 나옵니다.
- **PDF 전문**: `files/publication/<인용키>.pdf` 로 올리면 자동으로 `PDF` 버튼이 생겨요. (예: `files/publication/hong2026x.pdf`)
- **표지 이미지**: `images/covers/<인용키>.jpg` 를 넣으면 표지가 함께 표시됩니다.

**논문 추가**: 위 `@article{ ... }` 블록을 파일에 하나 더 붙여 넣으면 됩니다.

### ② 특허 — `data/publications/patents.json`

```json
{
  "patents": [
    {
      "id": "2026-bipolar-us",
      "y": "2026",
      "region": "intl",
      "a": "Dong-Ho Kang, Hyejin Yoon, Soeun Park",
      "t": "Bipolar Optical Synaptic Device",
      "app": "US 19/237,694",
      "reg": ""
    }
  ]
}
```

- `id` … 고유값 · `y` … 연도
- `region` … `"intl"`(해외) 또는 `"domestic"`(국내)
- `a` … 발명자 · `t` … 특허 제목
- `app` … 출원번호 · `reg` … 등록번호(아직 없으면 빈칸 `""`)
- (선택) 인증서 PDF: `files/patent/<id>.pdf`

**특허 추가**: `"patents": [ ... ]` 대괄호 안에 위 형식의 `{ }` 블록을 하나 더 넣습니다.

### ③ JCR 등급표 — `data/publications/jcr.json` (선택)

논문 옆의 **“상위 %” 배지**를 만들어 주는 참고표예요. 연도별로 저널의 백분위(`pct`)를 적어둡니다.

```json
{
  "2026": {
    "Advanced Functional Materials": { "field": "Materials Science", "pct": 4.2, "rank": "12/300" }
  }
}
```

- `pct`(백분위)가 낮을수록 상위 저널로 강조 표시됩니다.
- 새로 넣은 논문에 배지를 붙이고 싶으면, **그 논문의 연도**에 해당 저널을 여기에 추가하면 돼요. (없어도 논문 자체는 잘 나옵니다)

---

## 저장하고 홈페이지에 반영하기

1. 파일을 고친 뒤 GitHub에서 **Commit changes** 를 누릅니다.
2. **1~2분** 정도 기다립니다. (GitHub가 사이트를 다시 만드는 시간)
3. 홈페이지에서 **`Ctrl+Shift+R`** (Mac은 `Cmd+Shift+R`)로 강력 새로고침 하면 바뀐 내용이 보입니다.

### 잘 안 보일 때 체크리스트

- 새로 만든 항목이 **`_index.json` 목록에 추가**됐나요? (Members, 보도자료는 목록에 넣어야 나옵니다)
- **쉼표**를 빠뜨렸거나, **마지막 항목 뒤에 쉼표**를 넣지 않았나요?
- **큰따옴표** `" "` 를 썼나요? (작은따옴표는 안 됩니다)
- 사진·PDF 파일을 **올바른 폴더**에 **정확한 이름**으로 올렸나요?
- 그래도 이상하면 파일 내용을 [jsonlint.com](https://jsonlint.com) 에 붙여넣어 문법을 확인하세요.

---

### 폴더 요약

| 내용 | 파일 / 폴더 | 사진·파일 위치 |
|------|-------------|----------------|
| 구성원 | `data/members/<그룹>/` (+ `_index.json`) | 사진 `images/members/` · CV `files/cv/` |
| 인턴 | `data/members/interns.json` | — |
| 장비 | `data/research/facility.json` | `images/facility/` |
| 소식(논문/수상/과제) | `data/news/paper.json` · `award.json` · `grant.json` | — |
| 보도자료 | `data/news/media/` (+ `_index.json`) | `images/news/` |
| 논문 | `data/publications/ref.bib` | 표지 `images/covers/` · 전문 `files/publication/` |
| 특허 | `data/publications/patents.json` | 인증서 `files/patent/` |
| 교수님 | `index.html` (JSON 아님) | `images/members/` |
