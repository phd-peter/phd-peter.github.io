---
title: "Chirpy 테마에서 포스트 작성 가이드"
date: 2025-07-09 20:00:00 +0900
categories: [Blogging, Chirpy]
tags: [writing, jekyll, chirpy]
---

> 본 글은 [Chirpy 공식 문서](https://chirpy.cotes.page/posts/write-a-new-post/)의 "Writing a New Post" 내용을 기반으로 정리한 포스트 작성 팁입니다.

## 1. 파일명과 경로

* `_posts` 디렉터리 하위에 `YYYY-MM-DD-title.md` 형식으로 파일을 생성합니다.
* 확장자는 `md` 또는 `markdown`만 지원됩니다.

```bash
# 예시
_posts/2025-07-09-post-writing.md
```

## 2. Front Matter 필수 항목

Chirpy 테마에서는 아래 항목을 Front Matter 에 필수로 기재해야 합니다.

| 키(key) | 설명 |
|---------|------|
| `title` | 글 제목 |
| `date`  | 작성(발행) 일시 `YYYY-MM-DD HH:MM:SS +/-TTTT` 형식, *타임존* 포함 |
| `categories` | 최대 2단계: `[대분류, 소분류]` |
| `tags` | 0개 이상, 모두 *소문자* 권장 |

```yaml
---
title: "글 제목"
date: 2025-07-09 12:34:56 +0900
categories: [Tech, Jekyll]
tags: [jekyll, chirpy]
---
```

> `layout` 은 기본값이 `post` 이므로 별도 설정 필요 없습니다.

### 날짜 · 타임존

`_config.yml` 의 `timezone` 값을 설정했더라도, 각 포스트의 `date` 필드에 타임존 오프셋(`+0900` 등)을 반드시 명시해야 실제 공개 시간이 정확히 기록됩니다.

## 3. 카테고리 & 태그 작성 규칙

* **카테고리**는 대분류와 소분류로 최대 2개까지만 작성합니다.
  * 예) `[Life, Travel]`
* **태그**는 개수 제한이 없으며 소문자로 작성합니다.
  * 예) `[jekyll, chirpy, writing]`

## 4. 저자 정보(선택)

별도 지정하지 않으면 `_config.yml` 의 `social.name` 및 `social.links[0]` 값이 자동으로 사용됩니다. 여러 명의 저자를 표시하려면 `_data/authors.yml` 파일을 만들고 `author`(단일) 또는 `authors`(다중) 필드를 사용하세요.

## 5. 설명(description)

포스트 첫 문단이 기본 설명으로 사용됩니다. 별도 설명을 넣고 싶다면 Front Matter 에 `description` 필드를 추가하세요.

```yaml
---
description: Chirpy 테마에서 포스트를 작성할 때 알아두면 좋은 설정 팁 정리.
---
```

## 6. 목차(TOC)

기본적으로 우측 패널에 목차가 표시됩니다. 포스트 단위로 비활성화하려면 Front Matter 에 `toc: false` 를 추가합니다.

## 7. 댓글(Comments)

사이트 전역 댓글 시스템은 `_config.yml` 의 `comments.provider` 로 설정합니다. 특정 글에서만 댓글을 닫고 싶다면 `comments: false` 를 지정하세요.

## 8. 미디어(Image, Video, Audio)

* **URL 프리픽스**
  * CDN 을 사용한다면 `_config.yml` 의 `cdn` 값을 설정해 모든 미디어 경로 앞에 자동으로 붙입니다.
  * 글 단위 경로를 지정하려면 `media_subpath` 를 Front Matter 에 지정하세요.
* **이미지**
  * 캡션: 이미지 다음 줄에 _이탤릭_ 문장을 추가합니다.
  * 크기 지정: `{ :w="700" h="400" }` 형태로 가로·세로 픽셀을 지정해 레이아웃 흔들림을 방지합니다.
  * 위치: `.normal`, `.left`, `.right` 클래스로 정렬을 지정할 수 있습니다.
* **비디오 / 오디오**
  * `embed/video.html`, `embed/audio.html` 인클루드를 사용해 속성을 세부 조정할 수 있습니다.

## 9. 수식(MathJax) & 다이어그램(Mermaid)

수식이 필요하면 `math: true`, 다이어그램이 필요하면 `mermaid: true` 를 Front Matter 에 추가하여 기능을 활성화합니다.

## 10. 고정글(Pinned Post)

홈 화면 최상단에 고정하려면 `pin: true` 를 추가합니다. 고정된 글은 날짜 역순으로 정렬됩니다.

---

이상으로 Chirpy 테마에서 포스트를 작성할 때 고려해야 할 주요 팁을 살펴보았습니다. 이 글이 블로그 운영에 도움이 되길 바랍니다! 