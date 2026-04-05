---
layout: post
title: notion-database 2.0.0 릴리즈 — 바이브 코딩으로 4년 묵은 숙제를 한 번에
date: 2026-04-05 15:00:00 +0900
---

안녕하세요! 2021년에 처음 만든 notion-database 패키지가 드디어 2.0.0이 됐습니다!

사실 업데이트하고 싶은 것들이 쌓여 있었는데, 이번에 바이브 코딩(Claude와의 AI 페어 프로그래밍)을 활용해서 한 번에 전부 쏟아냈어요.
이 글은 출시 후기이자, 기존 1.4 사용자를 위한 마이그레이션 가이드입니다.

## 왜 2.0인가

1.x 시절의 API 설계는 솔직히 말하면 투박했습니다. 각 리소스마다 클래스를 따로 임포트해야 했고, 프로퍼티를 설정하는 방식이 Notion 공식 API 문서와 전혀 닮지 않아서 문서를 봐도 코드가 자연스럽게 나오지 않았거든요.

```python
# 1.x 시절 — 이게 뭘 하는 코드인지 읽기 쉽지 않다
from notion_database.page import Page
from notion_database.properties import Properties

PAGE = Page(integrations_token=token)
PROPERTY = Properties()
PROPERTY.set_title("이름", "안녕하세요")
PROPERTY.set_select("상태", "진행중")
PAGE.create_page(database_id=db_id, properties=PROPERTY)
```

거기다 몇 가지 오래된 이슈들도 있었습니다:

- urllib3 < 2.0 핀을 걸어 두고 있어서 다른 패키지와 충돌하는 경우가 있었어요
- Filter/Sort 빌더가 없어서 딕셔너리를 직접 조립해야 했고
- Users API, Comments API가 아예 없었으며
- 에러가 발생해도 예외가 던져지지 않고 에러 JSON이 그냥 반환됐습니다
- 페이지네이션을 위해 커서를 직접 관리해야 했고
- Notion API 버전도 한참 오래됐었죠

이 모든 걸 한꺼번에 정리하기로 했습니다.

## 바이브 코딩 후기

솔직히 이 규모의 리팩토링을 혼자 했으면 몇 주는 걸렸을 것 같아요. Claude와 함께 작업하면서 설계 논의, 코드 작성, 테스트 케이스, 문서 작성을 동시에 진행했습니다. 특히 Notion API 2026-03-11 버전의 새로운 엔드포인트들을 한꺼번에 통합하는 작업이 하루만에 끝났어요.

다만 AI와 작업할 때의 특성상 많은 커밋이 fix → revert → fix again 패턴을 반복하는데, 이건 바이브 코딩의 현실적인 모습이라고 생각합니다. 중요한 건 최종 결과물이고, 결과물은 꽤 만족스럽습니다!

## 2.0.0의 핵심 변화

### 1. 단일 진입점: NotionClient

가장 큰 변화입니다. 더 이상 리소스별로 클래스를 임포트할 필요 없이 `NotionClient` 하나로 모든 것에 접근할 수 있어요.

```python
from notion_database import NotionClient

client = NotionClient("secret_xxx")

# 모든 리소스가 client 아래에
client.databases   # DatabasesAPI
client.pages       # PagesAPI
client.blocks      # BlocksAPI
client.search      # SearchAPI
client.users       # UsersAPI      ← 신규
client.comments    # CommentsAPI   ← 신규
```

### 2. Notion API와 1:1 매핑

메서드 이름과 파라미터가 Notion 공식 REST API 문서와 그대로 대응됩니다. 이제 developers.notion.com 문서를 읽으면서 바로 코드를 쓸 수 있어요.

```python
# API 문서의 엔드포인트:  POST /pages
# 2.0의 메서드:          client.pages.create(...)

# API 문서의 엔드포인트:  POST /databases/{id}/query
# 2.0의 메서드:          client.databases.query(database_id, ...)
```

### 3. Notion API 버전 업그레이드: 2026-03-11

기존 2022-06-28에서 2026-03-11로 업그레이드됐습니다. 이에 따라 새로운 엔드포인트들이 대거 추가됐어요 (아래에서 상세 설명합니다).

## 신규 클래스 소개

### PropertyValue — 페이지 프로퍼티 값

페이지를 만들거나 업데이트할 때 `properties` 딕셔너리에 들어갈 값을 만드는 정적 팩토리 메서드 모음입니다.

```python
from notion_database import PropertyValue

properties = {
    "이름":     PropertyValue.title("2.0.0 출시!"),
    "상태":     PropertyValue.select("완료"),
    "태그":     PropertyValue.multi_select(["python", "notion", "open-source"]),
    "점수":     PropertyValue.number(100),
    "완료여부":  PropertyValue.checkbox(True),
    "마감일":   PropertyValue.date("2026-04-05"),
    "마감범위":  PropertyValue.date("2026-04-01", end="2026-04-05"),
    "링크":     PropertyValue.url("https://github.com/minwook-shin/notion-database"),
    "담당자":   PropertyValue.people(["user-id-1", "user-id-2"]),
    "관련페이지": PropertyValue.relation(["page-id-1"]),
}
```

### PropertySchema — 데이터베이스 컬럼 스키마

데이터베이스를 만들거나 수정할 때 컬럼 구조를 정의하는 메서드 모음입니다.

```python
from notion_database import PropertySchema, RichText

client.databases.create(
    parent={"type": "page_id", "page_id": page_id},
    title=[RichText.text("내 데이터베이스")],
    properties={
        "이름":    PropertySchema.title(),
        "상태":    PropertySchema.select([
                      {"name": "할 일", "color": "gray"},
                      {"name": "진행중", "color": "blue"},
                      {"name": "완료", "color": "green"},
                  ]),
        "점수":    PropertySchema.number("number"),
        "마감일":  PropertySchema.date(),
        "완료":    PropertySchema.checkbox(),
        "담당자":  PropertySchema.people(),
        "관련DB":  PropertySchema.relation("other-database-id"),
        "집계":    PropertySchema.rollup("관련DB", "점수", "sum"),
        "수식":    PropertySchema.formula("if(prop('완료'), 100, 0)"),
    },
)
```

2026-03-11 API에서 새로 추가된 컬럼 타입들도 지원합니다:

```python
PropertySchema.button()            # 자동화 버튼
PropertySchema.location()          # 지리적 위치
PropertySchema.last_visited_time() # 마지막 방문 시간 (읽기 전용)
PropertySchema.verification()      # 위키 페이지 검증 상태
```

### BlockContent — 블록 콘텐츠

페이지에 추가할 블록을 만드는 메서드 모음입니다. 지원하는 블록 타입이 대폭 늘었어요.

```python
from notion_database import BlockContent

children = [
    BlockContent.heading_1("제목 1"),
    BlockContent.heading_2("제목 2"),
    BlockContent.heading_3("제목 3"),
    BlockContent.paragraph("일반 텍스트"),
    BlockContent.callout("중요한 내용", emoji="💡"),
    BlockContent.quote("인용구"),
    BlockContent.bulleted_list_item("목록 항목 1"),
    BlockContent.numbered_list_item("번호 목록 1"),
    BlockContent.to_do("할 일 항목", checked=False),
    BlockContent.toggle("접기/펼치기"),
    BlockContent.code("print('hello')", language="python"),
    BlockContent.divider(),
    BlockContent.table_of_contents(),
    BlockContent.equation("E = mc^2"),
    BlockContent.image("https://example.com/image.png"),
    BlockContent.bookmark("https://github.com"),
    # 2026-03-11 신규
    BlockContent.tab("탭 이름"),
    BlockContent.tab_group([...]),
]

client.pages.create(
    parent={"database_id": db_id},
    properties={"이름": PropertyValue.title("새 페이지")},
    children=children,
)
```

### Filter — 유창한 필터 빌더

딕셔너리를 직접 조립하던 시절은 끝났습니다! 체이닝 방식으로 읽기 좋은 필터를 작성할 수 있어요.

```python
from notion_database import Filter

# 단일 필터
results = client.databases.query(
    db_id,
    filter=Filter.select("상태").equals("진행중"),
)

# 복합 필터 (AND)
results = client.databases.query(
    db_id,
    filter=Filter.and_(
        Filter.select("상태").equals("진행중"),
        Filter.number("점수").greater_than(80),
        Filter.checkbox("완료").equals(False),
    ),
)

# 복합 필터 (OR)
results = client.databases.query(
    db_id,
    filter=Filter.or_(
        Filter.select("상태").equals("진행중"),
        Filter.select("상태").equals("검토중"),
    ),
)

# 날짜 필터
results = client.databases.query(
    db_id,
    filter=Filter.date("마감일").past_week(),
)

# 타임스탬프 필터 (생성/수정 시간)
results = client.databases.query(
    db_id,
    filter=Filter.created_time().on_or_after("2026-01-01"),
)

# 2026-03-11 신규: formula, rollup, verification 필터
results = client.databases.query(
    db_id,
    filter=Filter.formula("수식열", "number").greater_than(50),
)
```

지원하는 필터 타입은 다음과 같습니다:
`text`, `title`, `number`, `checkbox`, `select`, `multi_select`, `status`, `date`, `people`, `files`, `relation`, `url`, `email`, `phone_number`, `unique_id`, `created_by`, `last_edited_by`, `formula`, `rollup`, `verification`, `created_time`, `last_edited_time`

### Sort — 정렬 빌더

```python
from notion_database import Sort

results = client.databases.query(
    db_id,
    sorts=[
        Sort.by_property("점수", "descending"),          # 점수 내림차순
        Sort.by_property("이름"),                         # 이름 오름차순 (기본)
        Sort.by_timestamp("created_time", "descending"), # 생성일 내림차순
    ],
)
```

### RichText — 리치 텍스트 빌더

단순 문자열 대신 서식이 있는 텍스트, 멘션, 수식 등을 표현할 때 사용합니다.

```python
from notion_database import RichText

# 텍스트
RichText.text("일반 텍스트")

# 페이지/데이터베이스/유저 멘션
RichText.mention_page("page-id")
RichText.mention_database("database-id")
RichText.mention_user("user-id")

# 날짜 멘션
RichText.mention_date("2026-04-05")

# 수식
RichText.equation("\\sqrt{2}")
```

### 자동 페이지네이션

커서를 직접 관리할 필요 없이 `_all()` 메서드 계열이 전체 결과를 한 번에 가져옵니다.

```python
# 데이터베이스의 모든 페이지 (커서 자동 처리)
all_pages = client.databases.query_all(db_id, filter=some_filter)

# 블록의 모든 자식 블록
all_blocks = client.blocks.retrieve_all_children(block_id)

# 워크스페이스의 모든 사용자
all_users = client.users.list_all()

# 검색 결과 전체
all_results = client.search.search_all(query="키워드")
```

### 타입화된 예외 처리

1.x에서는 에러가 나도 예외가 던져지지 않고 에러 JSON이 반환됐는데요. 2.0에서는 HTTP 오류 상황에 맞는 전용 예외가 발생합니다.

```python
from notion_database.exceptions import (
    NotionAPIError,
    NotionValidationError,    # 400
    NotionUnauthorizedError,  # 401
    NotionForbiddenError,     # 403
    NotionNotFoundError,      # 404
    NotionConflictError,      # 409
    NotionRateLimitError,     # 429
    NotionInternalError,      # 5xx
)

try:
    page = client.pages.retrieve("존재하지-않는-id")
except NotionNotFoundError as e:
    print(f"페이지를 찾을 수 없습니다: {e.message}")
except NotionRateLimitError:
    print("요청이 너무 많습니다. 잠시 후 재시도하세요.")
except NotionAPIError as e:
    print(f"API 오류 [{e.status_code}] {e.code}: {e.message}")
```

## Notion API 2026-03-11 신규 기능

### Markdown 읽기/쓰기

```python
# 페이지 전체 내용을 Markdown으로 가져오기
markdown = client.pages.retrieve_markdown(page_id)

# Markdown으로 페이지 내용 교체
client.pages.update_markdown(page_id, "# 새 내용\n\n이 페이지가 업데이트됐습니다.")
```

### 블록 삽입 위치 지정

```python
# 맨 앞에 삽입
client.blocks.append_children(page_id, children=[...], position="start")

# 맨 뒤에 추가 (기본값)
client.blocks.append_children(page_id, children=[...], position="end")

# 특정 블록 뒤에 삽입
client.blocks.append_children(page_id, children=[...], position="after_block")
```

### 데이터베이스 쿼리 확장

```python
# 휴지통 항목만 조회
trashed = client.databases.query(db_id, in_trash=True)

# 데이터베이스 업데이트 옵션 추가
client.databases.update(
    db_id,
    is_inline=True,  # 인라인 레이아웃으로 전환
    is_locked=True,  # 편집 잠금
)
```

### 페이지 생성 시 타임존 지정

```python
# @now, @today 템플릿 변수 해석에 사용되는 타임존 지정
client.pages.create(
    parent={"database_id": db_id},
    properties={"이름": PropertyValue.title("새 페이지")},
    timezone="Asia/Seoul",
)
```

### Users API / Comments API

1.x에서 완전히 빠져있던 두 가지 리소스가 추가됐습니다.

```python
# 사용자 조회
me = client.users.me()
user = client.users.retrieve("user-id")
users = client.users.list()          # 페이지 단위
all_users = client.users.list_all()  # 전체 자동 페이지네이션

# 댓글
comments = client.comments.retrieve(page_id=page_id)
client.comments.create(
    parent={"page_id": page_id},
    rich_text=[RichText.text("코드 리뷰 코멘트입니다.")],
)
```

## 1.4 → 2.0 마이그레이션 가이드

### 설치

```bash
pip install --upgrade notion-database
```

Python 3.10 이상이 필요합니다. (3.8, 3.9 지원 종료)

### 임포트 변경

```python
# Before (1.x)
from notion_database.page import Page
from notion_database.database import Database
from notion_database.block import Block
from notion_database.search import Search
from notion_database.properties import Properties
from notion_database.children import Children
from notion_database.cover import Cover
from notion_database.icon import Icon
from notion_database.const.query import Direction, Timestamp

# After (2.0)
from notion_database import (
    NotionClient,
    PropertyValue,
    PropertySchema,
    BlockContent,
    Filter,
    Sort,
    RichText,
)
from notion_database.models.icons import Icon, Cover
```

### 클라이언트 초기화

```python
# Before
PAGE = Page(integrations_token="secret_xxx")
DB = Database(integrations_token="secret_xxx")
BLOCK = Block(integrations_token="secret_xxx")

# After
client = NotionClient("secret_xxx")
# client.pages, client.databases, client.blocks 로 접근
```

### 페이지 생성

```python
# Before
PROPERTY = Properties()
PROPERTY.set_title("이름", "안녕하세요")
PROPERTY.set_select("상태", "진행중")
PROPERTY.set_number("점수", 42)
PROPERTY.set_checkbox("완료", False)

CHILD = Children()
CHILD.set_heading(heading_type=1, body="제목")
CHILD.set_paragraph(body="내용")

PAGE.create_page(database_id=db_id, properties=PROPERTY, children=CHILD)

# After
client.pages.create(
    parent={"database_id": db_id},
    properties={
        "이름": PropertyValue.title("안녕하세요"),
        "상태": PropertyValue.select("진행중"),
        "점수": PropertyValue.number(42),
        "완료": PropertyValue.checkbox(False),
    },
    children=[
        BlockContent.heading_1("제목"),
        BlockContent.paragraph("내용"),
    ],
)
```

### 페이지 업데이트

```python
# Before
PROPERTY = Properties()
PROPERTY.set_title("이름", "수정된 제목")
PAGE.update_page(page_id=page_id, properties=PROPERTY)

# After
client.pages.update(
    page_id,
    properties={"이름": PropertyValue.title("수정된 제목")},
)
```

### 페이지 아카이브 (삭제)

```python
# Before
PAGE.archive_page(page_id=page_id)

# After
client.pages.archive(page_id)
```

### 데이터베이스 쿼리

```python
# Before
from notion_database.const.query import Direction, Timestamp

DB.find_all_page(database_id=db_id)
# 필터/정렬은 딕셔너리 직접 조립 필요

# After — 전체 자동 페이지네이션
all_pages = client.databases.query_all(db_id)

# After — 필터 + 정렬
results = client.databases.query(
    db_id,
    filter=Filter.and_(
        Filter.select("상태").equals("진행중"),
        Filter.date("마감일").next_week(),
    ),
    sorts=[Sort.by_property("점수", "descending")],
)
```

### 블록 추가

```python
# Before
CHILD = Children()
CHILD.set_paragraph(body="새 단락")
BLOCK.append_children(block_id=page_id, children=CHILD)

# After
client.blocks.append_children(
    page_id,
    children=[BlockContent.paragraph("새 단락")],
)
```

### Cover / Icon

```python
# Before
COVER = Cover()
COVER.set_cover_image(url="https://example.com/cover.jpg")

ICON = Icon()
ICON.set_icon_emoji(emoji="🚀")

PAGE.create_page(database_id=db_id, properties=PROPERTY, cover=COVER, icon=ICON)

# After
from notion_database.models.icons import Cover, Icon

client.pages.create(
    parent={"database_id": db_id},
    properties={...},
    cover=Cover.external("https://example.com/cover.jpg"),
    icon=Icon.emoji("🚀"),
)
```

### 검색

```python
# Before
from notion_database.search import Search

SEARCH = Search(integrations_token=token)
SEARCH.search_pages(query="키워드", sort_direction=Direction.ascending)

# After
results = client.search.search(query="키워드", filter={"property": "object", "value": "page"})

# 전체 결과
all_results = client.search.search_all(query="키워드")
```

## 마치며

4년 동안 조금씩 붙여가던 패키지를 이번에 제대로 한 번 갈아엎었습니다.
바이브 코딩 덕분에 혼자였으면 엄두도 못 냈을 규모의 작업을 한 번에 진행할 수 있었어요.
AI와 협업하는 방식이 오픈소스 유지보수에도 실질적으로 도움이 된다는 걸 직접 체감한 경험이었습니다.

2.0의 API는 Notion 공식 문서와 최대한 가깝게 설계했기 때문에, 앞으로 Notion이 새 API를 추가할 때 이 패키지도 자연스럽게 따라갈 수 있는 구조가 됐어요.
1.4.x 라인은 1.4.x 브랜치에서 보안 패치 위주로 별도 관리할 예정입니다.

피드백이나 버그 리포트는 GitHub 이슈로 남겨주세요!

- GitHub: https://github.com/minwook-shin/notion-database
- PyPI: `pip install notion-database`
