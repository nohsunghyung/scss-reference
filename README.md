# SASS / SCSS

### 1. SASS / SCSS 차이

- SASS보다 SCSS가 뒤에 나왔고 여러가지 문법의 차이가 있지만 SCSS가 더 넓은 범용성과 CSS의 호환성 등의 장점으로 SCSS를 사용하기를 권장하고 있다.(공식문법도 SCSS를 기준으로 나와있다고 함) 따라서 SASS는 통상적으로 SCSS라고 부르기도 함
  - **sass**
  ```scss
    .list-item {
      width: 100px
      float: left
      li
        margin: 0 auto;
        &:last-child
          margin-right: 20px;
    }
  ```
  - **scss**
  ```scss
  .list-item {
    width: 100px;
    float: left;
    li {
      margin: 0 auto;
      &:last-child {
        margin-right: 20px;
      }
    }
  }
  ```

### 2. 설치법(node-sass)

- **node.js** 설치
- **node-sass** 글로벌 설치
- **npm i -g node-sass** (오류 발생시 npm rebuild node-sass)
- **버전확인** : node-sass -v
- **기본 사용 명령어** : node-sass scss/style.scss css/style.css
- **css 정렬 변경** : node-sass scss/style.scss css/style.css --output-style compressed
- **watch 명령어** : node-sass --watch scss/style.scss css/style.css --output-style compressed
- **source map 보기** : node-sass --watch scss/style.scss css/style.css --source-map true --output-style compressed
- **파일 생성시 주의사항 : 최종 빌드파일을 제외하고는 파일명 앞에 \_를 붙힌다**
  <br/>

### 3. SCSS 활용

- #### 클래스 들여쓰기

  - 기존 css로 작업시

  ```css
  .photo-list-container {
    width: 1200px;
    margin: 0 auto;
  }
  .photo-list-container .list-item {
    margin-top: 10px;
  }
  .photo-list-container .list-item + .list-item {
    margin-left: 20px;
  }
  .photo-list-container .list-item .title {
    color: red;
    font-size: 20px;
  }
  ```

  - scss로 작업시

  ```scss
  .photo-list-container {
    width: 1200px;
    margin: 0 auto;
    .list-item {
      margin-top: 10px;
      + .list-item {
        margin-left: 20px;
      }
      .title {
        color: red;
        font-size: 20px;
      }
    }
  }
  ```

- #### 선택자

  ```scss
  .list-content {
    li {
      > a {
        // 선택자 바로 아래 오는 선택자
      }
      + li {
        // 선택자의 바로 다음에 오는 선택자
      }
      ~ li {
        // 선택자의 다음에 오는 선택자
      }
      &.active {
        // 본인선택자
      }
      &:after,
      &:first-child {
        // 본인 가상선택자
      }
      #content & {
        // 상위 안에 포함된 본인선택자
      }
    }
  }
  ```

- #### 사칙(숫자)연산<br/>

  ```scss
  $headerHeight: 80px;

  .content {
    width: (120px / 4); // 나눗셈은 괄호 필수
    height: 200px - $headerHeight;
  }
  ```

- #### 변수 설정(config 설정(폰트,미디어쿼리) / 전역변수 )

  - config.scss 설정

  ```scss
  $body-base-font: 'NotoSansKR', arial, sans-serif, Arial, dotum, '돋움'; // 기본 폰트
  $spoqa-font: 'Spoqa Han Sans Neo', arial, sans-serif, Arial, dotum, '돋움'; // 특정 폰트
  $base-font-size: 20px; // 기본 폰트 사이즈
  $base-color: #000; // 기본 색상
  $line-height: 1.5; // 기본 line-height

  body {
    font-family: $body-base-font;
  }

  .content {
    font-family: $spoqa-font;
  }

  // 미디어 쿼리 셋팅
  $tablet: 1200px;
  $mobile: 768px;

  .list-item {
    width: 1200px;
    margin: 0 auto;
    @media (max-width: $mobile) {
      width: 100%;
    }
  }
  ```

  - 전역변수

  ```scss
  $slightly: #666;
  $primary: #ff237a;
  $warning: #ff9582;

  // 사용법
  .list-item {
    li {
      color: $primary;
    }
  }
  ```

  <br/>

- #### extend - 지정한 스타일 상속

  ```scss
  .list-a {
    width: 100px;
    height: 100px;
    font-size: 20px;
    background-color: #fff;
  }
  .list-b {
    @extend .list-a; // list-a의 css를 그대로 상속
    border: 1px solid #ddd;
  }
  ```

- #### mixin - 지정한 스타일 반환

  1. centerPosition(요소를 중앙으로)

  ```scss
  @mixin centerPosition {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  // 사용법
  .list-item {
    @include centerPosition();
  }
  ```

  2. size(width 와 height를 쉽게)

  ```scss
  @mixin size($width, $height: $width) {
    width: $width;
    height: $height;
  }

  // 사용법
  .list-item {
    @include size(100px);
  }
  ```

  3. 여러줄 말줄임

  ```scss
  @mixin multi-ellipsis($line, $line-height: 1.5em, $height-fixed: false) {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $line;
    line-height: $line-height * 1em;
    @if $height-fixed == true {
      // 고정 높이일경우
      height: ($line * $line-height) * 1em;
      max-height: ($line * $line-height) * 1em;
    } @else {
      // 높이 가변일경우
      max-height: ($line * $line-height) * 1em;
    }
  }

  // 사용법
  .text {
    @include multi-ellipsis(2, 1.5, true);
  }
  ```

  4. 스크린리더에서만 이용하게끔

  ```scss
  @mixin blind() {
    position: absolute !important;
    display: block;
    width: 0 !important;
    height: 0 !important;
    padding: 0 !important;
    margin: -1px !important;
    border: 0 !important;
    overflow: hidden !important;
    clip: rect(0 0 0 0) !important;
    &.focusable:active,
    &.focusable:focus {
      position: static;
      height: auto;
      width: auto;
      margin: 0;
      clip: auto;
      overflow: visible;
    }
  }

  // 사용법
  .text {
    @include blind;
  }
  ```

- #### 함수 - 연산된 값 반환

  1. rem 계산

  ```scss
  @function rem($px, $base: $base-font-size) {
    @return $px / $base * 1rem;
  }

  // 사용법
  .list-item {
    font-size: rem(16px);****
  }
  ```

  - 참고: SASS 내장함수
    https://lunuloopp.tistory.com/entry/Sass-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC-8-Sass%EB%82%B4%EC%9E%A5%ED%95%A8%EC%88%98%EB%93%A4

    <br/>

### 4.단축키 설정으로 빠른 SCSS 작업

ctrl + shift + p -> snippets -> 언어별 설정

```json
"Print to console": [
    {
      "prefix": "+es;",
      "body": [
        "@include ellipsis;",
      ],
      "description": "ellipsis"
    },
    {
      "prefix": "+me;",
      "body": [
        "@include multi-ellipsis($1);",
      ],
      "description": "multi ellipsis"
    },
    {
      "prefix": "+cf;",
      "body": [
        "@include cf;",
      ],
      "description": "clearfix"
    },
    {
      "prefix": "+sz;",
      "body": [
        "@include size($1);",
      ],
      "description": "size"
    },
    {
      "prefix": "+bs;",
      "body": [
        "box-sizing: border-box;",
      ],
      "description": "box-sizing"
    },
    {
      "prefix": "+mq;",
      "body": [
        "@media (max-width: $1) {$2}",
      ],
      "description": "mediaquery"
    },
    {
      "prefix": "+minq;",
      "body": [
        "@media (min-width: $1) {$2}",
      ],
      "description": "mediaquery"
    },
    {
      "prefix": "+bgi;",
      "body": [
        "@include _background-image('$1');",
      ],
      "description": "background-image"
    },
    {
      "prefix": "+bg;",
      "body": [
        "@include _background('$1');",
      ],
      "description": "background"
    },
    {
      "prefix": "+vw;",
      "body": [
        "@include _vw($1,$2);",
      ],
      "description": "vw"
    }
  ]
```

### 5.SCSS 사용시 주의사항

- 이미지 경로는 css파일 기준으로
- css작성시 클래스 단계를 많이 하는경우는 좋지않음(유니크한 클래스 작명)
- SCSS 작업후 다른사람이 css만 변경하게 될경우 SCSS와의 싱크가 맞지않아 코드가 빠질 우려가있음
  (css만 변경되었을경우는 css문법을 SCSS문법으로 교체 후 다시 SCSS작성 - 혼자 작업일경우는 상관없음)

### 6. node-sass 사용시 단점

- 저장을 눌렀을때 에러가 종종 발생(터미널 열어 에러확인 되면 다시 저장)
- 프로젝트에 맞는 명령어를 직접 입력해야 하는 번거로움
  - (node-sass --watch scss/style.scss css/style.css --output-style compressed)
- 로드맵이 간혹 틀릴경우가 있음 (클래스명으로 검색)
  - 변환 사이트 - http://css2sass.herokuapp.com/

### 7. lite-server 사용

#### 개요

- 프로젝트 작업시 가상서버를 열어주고 작업물 저장시 자동으로 새로고침 해주는 node 패키지
- 모바일에서도 같은 Wi-fi 사용시 IP로 접근가능

#### 설치법

- 해당 프로젝트 경로에서 **npm init**으로 **package.json** 생성
- **npm i --save-dev lite-server** (save-dev란? 배포시 필요없고 작업할때만 필요한 프로그램)
- package.json 파일 열어 **"dev": "lite-server"** 명령어 추가(아래참조)

```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "lite-server"
  },
```

- **npm run dev** 명령어를 치면 가상서버 작동
- url경로에 알맞은 html경로 입력

# <span style="color: #e04114;">gulp</span>

#### 개요

- 반복적이고 자주 사용되는 일을 자동화시켜주는 빌드 시스템(scss컴파일링, include, for문, if문 등등)

#### 설치법

- `npm i -g gulp` 전역설치(전역설치 후에도 로컬 설치 해줘야 할수있음)
- gulp 로컬설치 명령어 `npm i gulp --save-dev`
- gulp.js 및 package.json이 있는 프로젝트에 `npm install 또는 npm i`
- gulp 설치가 안될경우 `node -v` 확인후 node.js 홈페이지에서 14.14.0 버전으로 재설치
- gulp dev 입력했을때 gulp를 찾을수 없다는 문구나 설치가 안될 경우는 기본터미널을 git bash로 변경해서 진행(git 설치시 git bash 설치됨)

#### 사용법

- 프로젝트 실행 > 작업 시작시 명령어 입력
  - `gulp dev`
- 빌드(scss / html) > 작업완료후 html 및 css 빌드를 위함
  - `gulp build`

1. <span style="background-color: red"></span> ##### include

- **header / footer / resources / popup** 등 모든 파일에 들어가야하는 파일들을 하나의 파일로 관리하며 수정시 include의 파일만 수정함으로써 모든 페이지에 적용됨
- include 할 파일을 만든후 `@@include('../../includes/header.html')` 형식으로 작성
- 파라미터를 넘겨주어 상황에 따라 다르게 작업

```html
<!-- include 하면서 값 넘겨주기 -->
@@include('../../includes/sectKeyvisual.html', {title:'제목입니다.', subText:'내용입니다.'})

<!-- 변수를 이용해 값 보여주기 -->
<div class="sub-keyvisual">
  <h2>@@title</h2>
  <p>@@subText</p>
</div>
```

2. ##### for문

- 반복적인 엘리먼트를 모두 작성하지 않고 for문을 이용해 간결하게 작업

```html
<ul class="list">
  @@for(var i = `; i < 9; i++) {
  <li class="list-item">
    <div class="title">제목입니다.`+i+`</div>
    <div class="sub-text">내용입니다.</div>
    <img src="../images/img-${i}.png" />
  </li>
  }
</ul>
```

3. ##### if문

- 조건에 따라 다르게 엘리먼트 생성 / 클래스 추가 등을 작업

```html
<!-- include시 타입 넘겨주기 -->
@@include('../../includes/test.html',{type:'primary'})

<!-- 태그 분기처리 -->
@@if(type == 'primary') {
<header id="header" class="primary"></header>
} @@if(type == 'fix') {
<header id="header" class="fix"></header>
}

<!-- 변수 보내주는 파일 -->
@@include('../../includes/test.html',{activeIndex:7})

<!-- 클래스에 if문 넣기 -->
<div class="tab-item @@if(activeIndex == 7) {active}"></div>
```

#### gulp 유의사항

- include / if문 / for문등을 사용할때 문법 오류가 나거나 파일경로가 다를때는 watch가 풀리게 되므로 자동 새로고침이 안될때는 **<span style="color:red">터미널을 열어 오류 확인</span>**
- gulp dev했을때 안될경우 html폴더가 있는지 확인 하고 없으면 **<span style="color:red">빈 html 폴더를 만들어준다.</span>**
- 작업완료 후 파일 전달시는 `gulp build` 명령어를 입력해 파일 빌드후 <span style="color:red">html 폴더</span>와 <span style="color:red">resources 폴더</span>를 전달한다.(css에 소스맵 제거 및 html의 필요없는 파일을 제거하기 위함)
- 마크업은 **<span style="color:red">html-dev 폴더내에서만 작업</span>** (html폴더 내 작업시 빌드 후 작업내용 삭제됨)
- 스타일도 **<span style="color:red">scss폴더</span>** 내에서만 작업(style.css에 작업시 빌드 후 작업내용 삭제됨)
- 파일 검색시 html/ 내부의 파일이 검색안되게끔 vscode 환경설정 > exclude 검색 > `html/**/**.html` 패턴추가

#### 

#### prettier 적용안될때

https://yjg-lab.tistory.com/91

https://velog.io/@elliottjeong/VSC-Extension-Prettier%EC%9D%B4-%EC%A0%81%EC%9A%A9%EB%90%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EC%98%A4%EB%A5%98
