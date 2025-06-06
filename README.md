# Ph.D Peter 의 좌충우돌 코딩배우기

## 저장소 목적

- **코딩 입문자로서 배우는 모든 내용을 기록하고 성장 과정을 남기는 것**  
- **실습 예제, 마크다운 블로그 글, 문제 해결 과정, 블로그 운영 노하우 등을 정리**

## 주요 내용

- 입문자 관점에서 겪는 시행착오와 배운 점 기록
- 실습 코드 및 문제 해결 과정 정리
- 마크다운 기반 블로그 글 작성법 및 운영 팁 공유

## 기대 효과

- 나만의 개발 지식 베이스 구축
- 미래의 나와 다른 입문자들에게 도움이 되는 자료 제공
- 스스로의 성장을 돌아볼 수 있는 성장 아카이브 역할

- 블로그 주소: [https://phd-peter.github.io/](https://phd-peter.github.io/)

---

## 블로그 글 작성 및 배포 방법

### 1. 글 작성
- `_posts/` 폴더에 마크다운(`.md`) 파일로 글을 작성합니다.
- 파일명은 `YYYY-MM-DD-title.md` 형식으로 만듭니다. 영어로 작성하길 바람.
- **글 제목에는 아래와 같은 접두어를 사용할 수 있습니다:**
  - `[강의노트]` : 강의, 교재, 공식 문서 등 체계적인 학습 내용 정리
  - `[실습]` : 직접 해본 실습, 프로젝트, 코드 작성 등
  - `[문제해결]` : 에러, 버그, 시행착오, 해결 과정 기록
  - `[회고]` : 주간/월간 회고, 학습 정리 등
  - (필요시 자유롭게 추가)
- 예시:
  ```
  _posts/2024-06-10-my-first-post.md
  ```
- 파일 맨 위에는 아래와 같은 front matter가 필요합니다:
  ```markdown
  ---
  layout: post
  title: "[강의노트] 파이썬 변수와 자료형"
  date: 2024-06-10
  categories: [Python, 정형학습]
  ---
  여기에 본문을 작성하세요!
  ```
- **권장 categories 예시:**
  - `정형학습` : 강의, 교재, 공식 문서 등 체계적 학습
  - `비정형학습` : 실습, 문제해결, 자유주제 등 자발적 학습
    - `Python`, `Git`, `Web`, `Markdown` 등 주제별 카테고리
    - (복수 선택 가능, 필요시 자유롭게 추가)

### 2. 로컬에서 미리보기 (선택)
```sh
bundle exec jekyll serve
```
- 브라우저에서 `http://localhost:4000` 접속

### 3. 배포(publish)
- **GitHub Pages를 사용한다면 반드시 git push가 필요합니다!**
  1. 변경사항을 커밋
     ```sh
     git add .
     git commit -m "새 글 추가"
     ```
  2. 원격 저장소로 푸시
     ```sh
     git push
     ```
  3. 잠시 후(수십 초~수 분 내) GitHub Pages가 자동으로 배포됩니다.

---

- 커밋/푸시만 잘 해주면, 별도의 "배포" 명령은 필요 없습니다.
- 블로그 설정이나 테마를 바꿔도, push만 하면 자동 반영됩니다.
