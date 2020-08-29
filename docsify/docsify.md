# [Docsify 🎩](https://docsify.js.org/)

> Docsify로 쉽게 블로그 만들기
> https://docsify.js.org/

이 TIL 블로그는 docsify를 이용해서 만들고, netlify를 통해 배포했습니다.
마크다운 문법과 깃허브만 있으면 쉽게 맞춤형 블로그를 만들 수 있으니 필요하신 분들은 따라해보세용

<br /><br />

# 🙆‍♀️ Docsify 시작하기

> npm을 사용하기 때문에 node.js가 설치 되어있나 확인 후,
> docsify 설치 시작 🤟

### docsify 명령어 설치

```bash
npm i docsify-cli -g

docsify // 설치 확인
```

<br /><br />

### 프로젝트 시작

> 먼저 프로젝트를 시작할 프로젝트 폴더를 만들어 주는 게 좋다.
> 안 만들고 바로 폴더 안에 시작했더니 파일이 너저분해짐 ㅠ\_ㅠ

```bash
docsify init [프로젝트 명]
```

<br /><br />

### 로컬 서버로 확인하기

```bash
docsify serve [프로젝트 명]
```

<br /><br />

# 💅원하는 대로 꾸미기

<br />

### index.html 설정

```html
<body>
  <div id="app"></div>
  <script>
    //window.$docsify부분을 바꿔줘야 Main이 바뀌어요
    window.$docsify = {
      name: "TIL_Kaya",
      repo: "https://github.com/Kayaleeeee/TIL",
    };
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
</body>
```

<br /><br />

### 사이드 바 만들기

#### - index.html 설정

```html
<script>
  window.$docsify = {
    name: "TIL_Kaya",
    repo: "https://github.com/Kayaleeeee/TIL",

    //Sidebar 만들어주기
    loadSidebar: true,
  };
</script>
```

<br />

#### - \_sidebar.md파일 만들기

```md
- [Home](_coverpage.md)

- [📌 **마크다운**]()
- - [- 마크다운 기초](./Markdown/basic_md.md)

** 기본 형식 md 문법 사용 ** -[메뉴 제목]()

- - [포스팅 제목](url)
```

<br /><br />

### 커버 페이지 꾸미기

#### - index.html 설정

```html
<script>
  window.$docsify = {
    name: "TIL_Kaya",
    repo: "https://github.com/Kayaleeeee/TIL",

    //Sidebar 만들어주기
    loadSidebar: true,

    //Cover page만들기
    coverpage: true,
  };
</script>
```

<br />

#### - \_coverpage.md파일 생성

```
# 👩‍💻Today I Learned 🍒

#### run by Kaya Lee

### 하루 하루 공부한 것을 기록하는 공간입니다.
```

<br /><br />

기본적으로 마크다운 문법을 알면 쉽게 블로그를 꾸밀 수 있습니다.
다양한 설정들이 있으니 입맛에 맞게 꾸며보셔요 🌝

#### [마크다운 문법 보기](../Markdown/basic_md.md)

<br /><br />

## 나머지 설정

> https://docsify.js.org/ 참조
