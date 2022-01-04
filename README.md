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
- **기본 사용 명령어** : node-sass scss/style.scss css/style.css --output-style compressed
- **watch 명령어** : node-sass --watch scss/style.scss css/style.css --output-style compressed
- 파일 생성시 주의사항 : 최종 빌드파일을 제외하고는 파일명 앞에 \_를 붙힌다
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

- ### 선택자

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
      &:after {
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
  $tb: 1200px;
  $mo-l: 768px;
  $mo-s: 400px;

  .list-item {
    width: 1200px;
    margin: 0 auto;
    @media (max-width: $mo-l) {
      width: 100%;
    }
  }
  ```

  - 전역변수

  ```scss
  $slightly: #666;
  $primary: #ff237a;
  $warning: #ff9582;

  .list-item {
    li {
      color: $primary;
    }
  }
  ```

  <br/>

- #### mixin

  1. centerPosition(요소를 중앙으로)

  ```scss
  @mixin centerPosition {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  ```

  2. size(width 와 height를 쉽게)

  ```scss
  @mixin size($width, $height: $width) {
    width: $width;
    height: $height;
  }
  ```

  3. 여러줄 말줄임<br/>

  ```scss
  @mixin multi-ellipsis($line, $line-height: 1.5em, $height-fixed: false) {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $line;
    line-height: $line-height * 1em;
    @if $height-fixed == true {
      height: ($line * $line-height) * 1em;
      max-height: ($line * $line-height) * 1em;
    } @else {
      max-height: ($line * $line-height) * 1em;
    }
  }
  ```

  <br/>

  ### 4.SCSS 사용시 주의사항

  - 이미지 경로는 css파일 기준으로
  - 로드맵이 간혹 틀릴경우가 있음 (클래스명으로 검색)
  - css작성시 클래스 단계를 많이 하는경우는 좋지않음(유니크한 클래스 작명)
  - SCSS 작업후 다른사람이 css만 변경하게 될경우 SCSS와의 싱크가 맞지않아 코드가 빠질 우려가있음
    (css만 변경되었을경우는 css문법을 SCSS문법으로 교체 후 다시 SCSS작성 - 혼자 작업일경우는 상관없음)
    변환 사이트 - http://css2sass.herokuapp.com/

  ### 5. gulp 사용
