# SASS / SCSS

---

### 1. SASS / SCSS 차이

- SASS보다 SCSS가 뒤에 나왔고 여러가지 문법의 차이가 있지만 SCSS가 더 넓은 범용성과 CSS의 호환성 등의 장점으로 SCSS를 사용하기를 권장하고 있다.(공식문법도 SCSS를 기준으로 나와있다고 함) 따라서 SASS는 통상적으로 SCSS라고 부르기도 함
  <br/>

### 2. 설치법(node-sass)

- **node.js** 설치
- **node-sass** 글로벌 설치
- **npm i -g node-sass** (오류 발생시 npm rebuild node-sass)
- **버전확인** : node-sass -v
- **기본 사용 명령어** : node-sass scss/style.scss css/style.css --output-style compressed
- **watch 명령어** : node-sass --watch scss/style.scss css/style.css --output-style compressed
- 파일 생성시 주의사항 : 최종 빌드파일을 제외하고는 파일명 앞에 \_를 붙힌다
  <br/>

### 3. SCSS 장점 및 활용

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

- #### 숫자연산
- #### 변수 설정(config 설정 및 전역변수 - color, 미디어쿼리 사이즈, font-family 등등)\*\*

  - 폰트설정 예제
    $body-base-font: 'Spoqa Han Sans Neo', arial, sans-serif, Arial, dotum, '돋움';

- #### mixin

  1. centerPosition<br/>

  ```scss
  @mixin centerPosition {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  ```

  2. size

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

  ### SCSS 사용시 주의사항

  - 이미지 경로는 css 기준으로
  - 로드맵이 간혹 틀릴경우가 있음
  - css작성시 클래스 단계를 많이 하는경우는 좋지않음(유니크한 클래스 작명)
  -
