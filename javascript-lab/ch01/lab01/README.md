# HTML과 자바스크립트 연결

HTML에 자바스크립트를 직접 작성하거나 외부 파일로 연결하고, 콘솔에서 실행 결과와 오류 메시지를 확인한다.

- HTML에 자바스크립트를 넣기
- 연결하고 외부 스크립트와 defer
- 콘솔, 오류 메시지 읽기

## 1. `<script>` 태그를 이용하여 HTML에 자바스크립트 넣기

HTML 문서에 `<script>` 태그를 작성하면 그 안에 자바스크립트 코드를 넣을 수 있다. 브라우저는 HTML을 해석하다가 `<script>` 태그를 만나면 **DOM Tree 생성을 멈추고 자바스크립트 코드를 실행한다.** 자바스크립트 실행이 완료되면 다시 HTML 해석을 이어간다. 

이처럼 HTML 문서 안에 코드를 직접 작성하는 방식을 **인라인 스크립트(inline script)** 라고 한다.

### 기본 문법

```html
<script>
  console.log("안녕하세요!");
</script>
```

- `<script>`와 `</script>` 사이에 자바스크립트 코드를 작성한다.
- `console.log()`는 괄호 안의 값을 개발자 도구의 **Console** 탭에 출력한다.
- `"안녕하세요!"`는 큰따옴표로 감싼 문자열이다.
- 세미콜론(`;`)은 문장의 끝을 나타낸다.
- 일반 자바스크립트를 실행할 때는 `type="text/javascript"` 속성을 생략할 수 있다.

### exam01.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>HTML에 자바스크립트 넣기</title>
</head>
<body>
  <h1>자바스크립트 실행</h1>
  <p>개발자 도구의 Console 탭에서 실행 결과를 확인하세요.</p>

  <script>
    console.log("안녕하세요!");
    console.log(10 + 20);
  </script>
</body>
</html>
```

브라우저 화면에는 `<h1>`과 `<p>`의 내용이 표시된다. `<script>` 안의 코드는 화면에 글자로 표시되지 않고 실행되며, 위 예제의 실행 결과는 콘솔에서 확인할 수 있다.

### 실행 결과 확인하기

1. 저장한 `exam01.html` 파일을 브라우저로 연다.
2. 개발자 도구를 열고 **Console** 탭을 선택한다.
3. 페이지를 새로고침하여 스크립트를 다시 실행한다.
4. 다음 두 값이 순서대로 출력되는지 확인한다.

```text
안녕하세요!
30
```

### 작성 위치와 실행 순서

`<script>` 태그는 `<head>`나 `<body>` 안에 작성할 수 있다. 이 예제에서는 `</body>` 바로 앞에 배치했다. 이 위치에서는 앞에 작성한 HTML 요소가 만들어진 뒤 스크립트가 실행되므로, 이후 화면 요소를 다루는 코드를 작성할 때 활용할 수 있다.

이 예제처럼 별도의 속성 없이 작성한 일반 인라인 스크립트는 다음 순서로 실행된다.

1. 브라우저가 HTML을 위에서부터 해석한다.
2. `<script>` 태그를 만나면 HTML 해석을 잠시 멈추고 그 안의 코드를 실행한다.
3. 스크립트 실행을 마치면 이어지는 HTML을 해석한다.

하나의 HTML 문서에 여러 개의 `<script>` 태그를 작성할 수도 있다. 다음 코드를 `<body>` 안에 넣으면 첫 번째 스크립트가 실행된 다음 두 번째 스크립트가 실행된다.

```html
<script>
  console.log("첫 번째 스크립트 실행");
</script>

<script>
  console.log("두 번째 스크립트 실행");
</script>
```

```text
첫 번째 스크립트 실행
두 번째 스크립트 실행
```

## 2. 외부 스크립트 연결하기

자바스크립트 코드를 별도의 `.js` 파일에 작성하고, `<script>` 태그의 `src` 속성으로 HTML 문서에 연결할 수 있다. 이렇게 연결한 파일을 **외부 스크립트(external script)**라고 한다.

HTML과 자바스크립트를 파일로 분리하면 코드를 관리하기 쉽고, 여러 HTML 문서에서 같은 자바스크립트 파일을 사용할 수 있다.

### 기본 문법

```html
<script src="exam02.js"></script>
```

- `src` 속성에 자바스크립트 파일의 경로를 지정한다.
- 외부 파일을 연결할 때도 닫는 태그인 `</script>`를 작성한다.
- `.js` 파일에는 자바스크립트 코드만 작성한다. `<script>` 태그는 넣지 않는다.
- `src`를 지정한 `<script>` 태그 내부에 함께 작성한 코드는 실행되지 않는다. 추가로 실행할 코드는 외부 파일에 넣거나 별도의 `<script>` 태그에 작성한다.

### 예제 파일 구성

다음 두 파일을 같은 디렉터리에 작성한다.

```text
lab01/
├── exam02.html
└── exam02.js
```

`exam01.html`의 `<script>` 안에 있던 코드를 `exam02.js`로 옮기고, `exam02.html`에서 이 파일을 연결한다.

### exam02.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>외부 스크립트 연결하기</title>
</head>
<body>
  <h1>외부 자바스크립트 실행</h1>
  <p>개발자 도구의 Console 탭에서 실행 결과를 확인하세요.</p>

  <script src="exam02.js"></script>
</body>
</html>
```

### exam02.js

```javascript
console.log("안녕하세요!");
console.log(10 + 20);
```

브라우저는 `exam02.html`을 해석하다가 `<script src="exam02.js">`를 만나면 지정한 파일을 불러와 실행한다. 자바스크립트 코드를 외부 파일로 옮겨도 실행 결과는 인라인 스크립트 예제와 같다.

### 외부 파일의 경로 지정하기

이 예제에서 `src`의 상대 경로는 HTML 파일의 위치를 기준으로 해석된다. 자바스크립트 파일의 위치에 따라 다음과 같이 작성한다.

| 자바스크립트 파일의 위치 | `src` 속성값 |
| --- | --- |
| HTML 파일과 같은 디렉터리 | `exam02.js` 또는 `./exam02.js` |
| HTML 파일이 있는 디렉터리의 `js` 하위 디렉터리 | `js/exam02.js` |
| HTML 파일이 있는 디렉터리의 상위 디렉터리 | `../exam02.js` |

예를 들어 `exam02.js`를 `js` 디렉터리로 옮겼다면 연결 코드를 다음과 같이 변경한다.

```html
<script src="js/exam02.js"></script>
```

## 3. 스크립트의 실행 시점과 defer

브라우저는 HTML을 해석하면서 각 요소를 객체로 만들고, 이 객체들을 연결하여 **DOM 트리(DOM Tree)**를 구성한다. 자바스크립트가 HTML 요소를 조회하거나 변경하려면 해당 요소가 먼저 만들어져 있어야 한다.

따라서 같은 코드라도 `<script>` 태그의 위치와 속성에 따라 실행 결과가 달라질 수 있다. 이 절에서는 HTML에 직접 작성한 일반 스크립트의 실행 시점을 살펴본다.

### 일반 외부 스크립트의 실행 시점

`src`만 지정한 일반 외부 스크립트는 다음 순서로 처리된다.

1. 브라우저가 HTML을 해석하다가 `<script>` 태그를 만나면 HTML 해석과 DOM 트리 생성을 잠시 멈춘다.
2. 외부 자바스크립트 파일이 준비될 때까지 기다린 뒤 그 안의 코드를 실행한다.
3. 스크립트 실행을 마치면 이어지는 HTML을 해석하여 DOM 트리 생성을 계속한다.

예를 들어 `<head>`에서 스크립트를 실행하면 뒤쪽 `<body>`에 작성한 요소는 아직 만들어지지 않았을 수 있다. 이때 해당 요소를 조회하면 `null`이 반환되고, 그 값을 이용하여 요소를 변경하려 하면 오류가 발생한다.

### exam03.html — DOM 요소가 만들어지기 전에 실행하기

다음 `exam03.html`과 `exam03.js`를 같은 디렉터리에 작성한다. 먼저 `defer` 없이 스크립트를 `<head>`에 연결한다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>스크립트의 실행 시점</title>
  <script src="exam03.js"></script>
</head>
<body>
  <h1 id="message">스크립트 실행 전</h1>
</body>
</html>
```

### exam03.js

```javascript
console.log("exam03.js 실행");
console.log(document.querySelector("#message"));
document.querySelector("#message").textContent = "스크립트 실행 완료";
```

이 예제에서 사용하는 DOM 관련 코드는 다음 두 가지다. 자세한 사용법은 DOM을 다루는 장에서 학습한다.

- `document.querySelector("#message")`: `id`가 `message`인 요소를 찾는다. 요소가 없으면 `null`을 반환한다.
- `textContent`: 찾은 요소의 텍스트 내용을 읽거나 변경하는 프로퍼티다.

`exam03.html`을 브라우저로 열고 개발자 도구의 **Console** 탭에서 결과를 확인한다.

1. 콘솔에 `exam03.js 실행`이 출력된다.
2. 아직 `<h1>` 요소가 만들어지지 않았으므로 다음 줄에는 `null`이 출력된다.
3. `null`의 `textContent`를 변경하려고 하여 `TypeError`가 발생한다.
4. 브라우저는 이어지는 HTML을 해석하므로 화면에는 원래 제목인 `스크립트 실행 전`이 표시된다.

이 오류는 **요소가 준비되기 전에 그 요소에 접근했기 때문에** 발생한다.

### defer로 HTML 해석이 끝난 뒤 실행하기

**`defer`** 속성을 사용하면 HTML 해석을 계속하면서 외부 스크립트 파일을 다운로드하고, HTML 해석이 끝난 뒤 스크립트를 실행한다.

```text
HTML 파싱:  ──────────────────────── 완료
JS 다운로드:     ────────── 완료
JS 실행:                              ───── 실행
```

`exam03.html`의 기존 `<script>` 태그에 `defer`를 추가한다. `exam03.js`의 코드는 그대로 사용한다.

```html
<script src="exam03.js" defer></script>
```

실행 흐름은 다음과 같다.

1. 브라우저가 `<script src="exam03.js" defer>`를 만나 외부 파일을 불러온다.
2. 파일을 다운로드하는 동안에도 HTML 해석과 DOM 트리 생성을 계속한다.
3. HTML 해석이 끝나고 스크립트 파일도 준비되면 자바스크립트 코드를 실행한다.
4. 이때는 `<h1 id="message">`가 만들어져 있으므로 요소를 찾아 텍스트를 변경할 수 있다.

파일을 저장하고 페이지를 새로고침하면 콘솔에는 `exam03.js 실행`과 찾은 `<h1>` 요소가 출력된다. 화면의 제목은 다음과 같이 바뀐다.

```text
스크립트 실행 완료
```

`defer`는 **다운로드와 실행 시점을 구분**하는 속성이다. 파일을 미리 다운로드하더라도 실행은 HTML 해석이 끝날 때까지 미룬다. 자바스크립트 코드 자체가 HTML 해석과 동시에 실행되는 것은 아니다.

## 4. 스크립트를 body 끝에 두기

`defer`를 제거하고 `<script>` 태그를 `<h1>` 뒤로 옮겨도 이 예제는 정상 동작한다. 다음은 `exam03.html`의 `<head>`에 있던 스크립트를 제거하고 `<body>` 끝에 배치한 경우다.

### exam04.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>스크립트의 실행 시점</title>
</head>
<body>
  <h1 id="message">스크립트 실행 전</h1>
  <script src="exam03.js"></script>
</body>
</html>
```

이 경우에는 `<h1>` 요소를 만든 다음 스크립트를 만나므로 해당 요소를 찾을 수 있다. 다만 스크립트가 HTML 해석의 완료를 기다리는 것은 아니며, 작성된 위치에서 실행된다.

### defer 방식과 비교

| 연결 방식 | 실행 시점 | 예제의 `#message` 조회 결과 |
| --- | --- | --- |
| `<head>`에 일반 외부 스크립트 연결 | 해당 위치에서 HTML 해석을 멈추고 파일이 준비되면 실행 | 아직 요소가 없어 `null` 반환 |
| `<body>` 끝에 일반 외부 스크립트 연결 | 앞쪽 요소를 만든 뒤 해당 위치에서 실행 | 앞에 작성한 `<h1>` 요소 반환 |
| `<head>`에 `defer`를 지정하여 연결 | HTML 해석이 끝나고 파일이 준비된 뒤 실행 | DOM 트리에 있는 `<h1>` 요소 반환 |

## 5. 여러 defer 스크립트의 실행 순서

여러 일반 외부 스크립트에 `defer`를 지정하면 **HTML 문서에 작성한 순서대로 실행**한다.

```html
<script src="first.js" defer></script>
<script src="second.js" defer></script>
```

`second.js`의 다운로드가 먼저 끝나더라도 `first.js`가 실행된 다음 `second.js`가 실행된다. 따라서 앞선 파일에서 준비한 기능을 다음 파일에서 사용하는 경우에도 실행 순서를 유지할 수 있다.

### defer 사용 시 확인할 점

- `defer`는 일반 외부 스크립트에 적용한다. **`src` 없이 코드를 직접 작성한 일반 인라인 스크립트에는 효과가 없다.**
- `defer` 스크립트는 HTML 해석이 끝난 뒤 실행되며, 이미지 등 모든 리소스의 다운로드 완료를 기다리지는 않는다.
- 일반 `defer` 스크립트의 실행이 끝난 뒤 `DOMContentLoaded` 이벤트가 발생한다. 이 이벤트는 HTML 해석과 관련 스크립트 실행이 완료되었음을 알린다.
- 같은 파일을 `<head>`와 `<body>`에 중복 연결하면 두 번 실행될 수 있으므로, 위치를 바꾸는 실습에서는 기존 `<script>` 태그를 옮긴다.


## 6. `DOMContentLoaded` 이벤트의 활용

**`DOMContentLoaded`** 는 브라우저가 HTML 해석을 마쳐 DOM 트리를 완성하고, `defer`로 연결한 일반 외부 스크립트의 실행을 마친 뒤 발생하는 이벤트다.

이 이벤트에 실행할 함수를 등록하면 `<head>`에 스크립트를 작성해도 DOM 요소가 준비된 뒤에 해당 요소를 조회하거나 변경할 수 있다.

### 기본 문법

```javascript
document.addEventListener("DOMContentLoaded", function() {
  // DOM 트리가 완성된 뒤 실행할 코드
  document.querySelector("#message").textContent = "DOM 준비 완료";
});
```

- `document`는 현재 HTML 문서를 나타내는 객체다.
- `addEventListener()`는 특정 이벤트가 발생했을 때 실행할 함수를 등록한다.
- `"DOMContentLoaded"`는 이벤트 이름이며, 대소문자를 구분하여 작성한다.
- `function() { ... }`은 실행할 코드를 묶은 함수다. 여기서는 브라우저가 이벤트 발생 시 호출하는 **이벤트 리스너(콜백 함수)**로 사용한다.

리스너를 등록하면 브라우저는 다음 문장을 계속 실행한다. 등록한 함수 안의 코드는 나중에 `DOMContentLoaded` 이벤트가 발생했을 때 실행된다.

### exam06.html

다음 코드를 `exam06.html` 파일로 작성한다. `<head>`의 인라인 스크립트에서 이벤트 리스너를 등록하고, 그 함수 안에서 `<body>`의 제목을 변경한다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>DOMContentLoaded 이벤트의 활용</title>
  <script>
    console.log("1. 이벤트 리스너 등록 전");

    document.addEventListener("DOMContentLoaded", function() {
      console.log("3. DOMContentLoaded 이벤트 발생");
      document.querySelector("#message").textContent = "DOM 준비 완료";
    });

    console.log("2. 이벤트 리스너 등록 후");
  </script>
</head>
<body>
  <h1 id="message">스크립트 실행 전</h1>
</body>
</html>
```

실행 순서는 다음과 같다.

1. 브라우저가 `<head>`의 `<script>`를 만나면 HTML 해석을 잠시 멈추고 스크립트 안의 코드를 실행한다.
2. `DOMContentLoaded` 이벤트 리스너를 등록한다. 이 시점에는 함수 안의 코드를 실행하지 않는다.
3. 이어지는 HTML을 해석하여 `<h1 id="message">`를 만들고 DOM 트리를 완성한다.
4. `defer`로 연결한 일반 외부 스크립트가 있다면 그 스크립트도 실행한다.
5. `DOMContentLoaded` 이벤트가 발생한다.
5. `DOMContentLoaded` 이벤트 시 호출되도록 등록한 함수를 실행하여 제목을 변경한다.

**DOM 요소를 조회하고 변경하는 코드를 콜백 함수 안에 작성하는 것**이 핵심이다. 리스너 등록 뒤에 DOM 변경 코드를 함수 밖으로 작성하면, 위 예제에서는 `<h1>`이 만들어지기 전에 실행되어 오류가 발생한다.

### defer와 비교하기

`defer`와 `DOMContentLoaded`는 모두 DOM이 준비된 뒤 코드를 실행할 때 활용할 수 있지만, 적용하는 대상이 다르다.

| 구분 | `defer` | `DOMContentLoaded` 이벤트 리스너 |
| --- | --- | --- |
| 지정 방법 | HTML의 `<script>` 태그에 속성 추가 | 자바스크립트에서 `document.addEventListener()` 호출 |
| 실행을 미루는 대상 | 일반 외부 스크립트 파일의 코드 | 이벤트 리스너로 등록한 함수 안의 코드 |
| 인라인 스크립트에서 활용 | `src`가 없는 일반 인라인 스크립트에는 효과 없음 | 인라인 스크립트에서도 리스너 등록 가능 |
| 실행 시점 | HTML 해석이 끝나고 파일이 준비된 뒤 | HTML 해석과 일반 `defer` 스크립트 실행이 끝난 뒤 이벤트 발생 시 |

`DOMContentLoaded` 리스너를 등록해도 그 스크립트의 다운로드 방식이나 함수 밖에 있는 코드의 실행 시점은 바뀌지 않는다. 위 예제의 인라인 스크립트도 HTML 해석 도중 실행되며, DOM을 변경하는 함수의 실행만 이벤트 발생 시점으로 미룬다.

`defer`로 연결한 일반 외부 스크립트는 실행 시점에 DOM 트리가 준비되어 있으므로, DOM 요소를 다루기 위한 목적으로 코드를 다시 `DOMContentLoaded` 리스너 안에 넣을 필요는 없다.

### load 이벤트와 구분하기

`DOMContentLoaded`는 DOM 요소가 준비되었을 때 발생하며, 이미지 등 모든 리소스의 다운로드 완료를 기다리지는 않는다.

페이지 로딩에 포함되는 이미지 등의 리소스까지 로딩을 마친 시점을 확인하려면 `window`의 `load` 이벤트를 사용한다.

```javascript
window.addEventListener("load", function() {
  console.log("페이지 로딩 완료");
});
```

위 리스너를 `exam06.html`의 `<script>` 안에 추가하면 `DOMContentLoaded` 콜백의 메시지가 출력된 뒤 `페이지 로딩 완료`가 출력된다. 지연 로딩 대상으로 지정한 이미지 등은 페이지의 `load` 이벤트가 기다리는 대상에서 제외될 수 있다.

### 참고: 이벤트 발생 후 코드가 실행되는 경우

`DOMContentLoaded`는 한 문서의 로딩 과정에서 한 번 발생한다. 이벤트가 이미 발생한 뒤 리스너를 등록하면 해당 함수는 그 이벤트로 호출되지 않는다. 예를 들어 페이지 로딩이 끝난 뒤 개발자 도구의 Console에서 리스너를 등록하면 이전 이벤트를 다시 받을 수 없다.

코드가 실행되는 시점이 일정하지 않다면 `document.readyState`로 HTML 해석 상태를 확인하여 DOM 초기화 함수를 실행할 수 있다.

```javascript
function initializePage() {
  document.querySelector("#message").textContent = "DOM 준비 완료";
}

if (document.readyState === "loading") {
  document.addEventListener("DOMContentLoaded", initializePage);
} else {
  initializePage();
}
```

- HTML을 해석 중인 `"loading"` 상태이면 리스너를 등록하여 DOM이 준비될 때까지 기다린다.
- HTML 해석이 끝난 상태이면 DOM 요소를 다루는 초기화 함수를 바로 실행한다.
- 리스너를 등록할 때는 `initializePage`처럼 함수 자체를 전달한다. `initializePage()`라고 작성하면 등록 과정에서 함수를 즉시 호출하게 된다.

이 패턴은 DOM 요소가 준비되었는지를 기준으로 초기화한다. 다른 `defer` 스크립트까지 모두 실행되었는지를 확인하는 코드는 아니다. 함수와 조건문의 자세한 문법은 이후 실습에서 학습한다.

## 7. 스크립트 실행 순서

인라인 스크립트와 일반 외부 스크립트, `defer` 스크립트, `DOMContentLoaded` 이벤트, `load` 이벤트 리스너가 섞여 있는 경우 실행 순서를 정리하면 다음과 같다.

아래 설명은 HTML 문서에 직접 작성한 일반 인라인 스크립트, `src`만 지정한 외부 스크립트, `defer` 외부 스크립트의 동기 코드와 문서 로딩 이벤트를 기준으로 한다.

### 실행 순서를 결정하는 기준

| 단계 | 실행 대상 | 실행 시점과 순서 |
| --- | --- | --- |
| 1 | 일반 인라인·외부 스크립트 | HTML을 해석하면서 `<script>` 태그를 만나는 순서대로 실행한다. 일반 외부 스크립트는 파일이 준비될 때까지 기다린다. |
| 2 | `defer` 외부 스크립트 | HTML 해석이 끝난 뒤, 파일이 준비되는 것을 기다려 HTML에 작성한 순서대로 실행한다. |
| 3 | `DOMContentLoaded` 이벤트 리스너 | HTML 해석과 일반 `defer` 스크립트 실행이 끝난 뒤 이벤트가 발생하면 실행한다. |
| 4 | `load` 이벤트 리스너 | 페이지 로딩에 포함되는 이미지 등의 리소스까지 로딩을 마친 뒤 이벤트가 발생하면 실행한다. |

**일반 인라인 스크립트와 일반 외부 스크립트 사이의 순서는 HTML에 작성한 위치로 결정된다.** 두 종류의 스크립트가 같은 단계에서 문서 순서대로 실행된다.

`defer` 스크립트가 `<head>` 앞쪽에 있어도 `<body>` 끝의 일반 스크립트보다 나중에 실행된다. `<body>` 끝의 일반 스크립트까지 실행하고 HTML 해석을 마쳐야 `defer` 스크립트의 실행을 시작할 수 있기 때문이다.

또한 이벤트 리스너를 등록하는 문장은 해당 스크립트가 실행될 때 처리되지만, 등록한 함수 안의 코드는 이벤트가 발생했을 때 실행된다.

### 예제 파일 구성

다음 네 파일을 같은 디렉터리에 작성한다. 각 로그의 숫자는 원래 예제에서 예상하는 실행 순서를 나타낸다.

```text
lab01/
├── exam07.html
├── exam07-normal.js
├── exam07-defer1.js
└── exam07-defer2.js
```

### exam07.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>스크립트 실행 순서</title>
  <script>
    console.log("1. head 인라인 스크립트");

    document.addEventListener("DOMContentLoaded", function() {
      console.log("6. DOMContentLoaded 리스너");
    });

    window.addEventListener("load", function() {
      console.log("7. load 리스너");
    });
  </script>

  <script src="exam07-defer1.js" defer></script>
  <script src="exam07-normal.js"></script>
  <script src="exam07-defer2.js" defer></script>
</head>
<body>
  <h1>스크립트 실행 순서</h1>
  <p>개발자 도구의 Console 탭에서 출력 순서를 확인하세요.</p>

  <script>
    console.log("3. body 인라인 스크립트");
  </script>
</body>
</html>
```

### exam07-normal.js

```javascript
console.log("2. 일반 외부 스크립트");
```

### exam07-defer1.js

```javascript
console.log("4. 첫 번째 defer 스크립트");
```

### exam07-defer2.js

```javascript
console.log("5. 두 번째 defer 스크립트");
```

### 실행 결과 확인하기

네 파일을 저장하고 `exam07.html`을 브라우저로 연다. 개발자 도구의 **Console** 탭을 열고 페이지를 새로고침하면, 파일이 모두 정상적으로 로드되는 경우 다음 순서로 출력된다.

```text
1. head 인라인 스크립트
2. 일반 외부 스크립트
3. body 인라인 스크립트
4. 첫 번째 defer 스크립트
5. 두 번째 defer 스크립트
6. DOMContentLoaded 리스너
7. load 리스너
```

실행 과정을 HTML의 위치와 연결하여 살펴보면 다음과 같다.

1. `<head>`의 인라인 스크립트에서 `1`을 출력하고 두 이벤트의 리스너를 등록한다.
2. 첫 번째 `defer` 파일을 불러오되 실행은 미루고, 다음 일반 외부 스크립트를 실행하여 `2`를 출력한다.
3. 두 번째 `defer` 파일도 실행을 미룬다. HTML 해석을 계속하여 `<body>`의 인라인 스크립트에서 `3`을 출력한다.
4. HTML 해석이 끝나면 두 `defer` 스크립트를 문서에 작성한 순서대로 실행하여 `4`, `5`를 출력한다.
5. `DOMContentLoaded` 이벤트의 리스너에서 `6`을 출력한다.
6. 페이지 로딩이 끝나면 `load` 이벤트의 리스너에서 `7`을 출력한다.

`exam07-defer1.js`가 `exam07-normal.js`보다 먼저 작성되어 있어도 `2`가 `4`보다 먼저 출력된다. 또한 `exam07-defer2.js`의 다운로드가 먼저 끝나더라도 두 `defer` 스크립트의 실행 순서는 `4`, `5`로 유지된다.

### 코드를 바꾸어 순서 비교하기

각 실습은 원래 예제에서 시작한다. 로그 메시지의 숫자는 그대로 두고 `<script>` 태그의 위치나 속성만 바꾼 뒤, 저장하고 페이지를 새로고침한다.

| 변경 내용 | 예상 로그 번호 순서 | 확인할 내용 |
| --- | --- | --- |
| 두 `defer` 태그의 위치를 서로 바꾼다. | `1 → 2 → 3 → 5 → 4 → 6 → 7` | `defer` 스크립트끼리는 문서에 작성한 순서대로 실행된다. |
| 일반 외부 스크립트 태그를 `<body>`의 인라인 스크립트 뒤로 옮긴다. | `1 → 3 → 2 → 4 → 5 → 6 → 7` | 일반 인라인·외부 스크립트는 HTML에서 만나는 순서대로 실행된다. |
| 첫 번째 외부 파일의 `defer` 속성만 제거한다. | `1 → 4 → 2 → 3 → 5 → 6 → 7` | `defer`를 제거한 파일은 해당 위치에서 실행된다. |

마지막 실습에서 파일 이름과 로그 메시지에는 `defer`가 남아 있어도, 실제 실행 방식은 `<script>` 태그의 속성으로 결정된다.

## 실습 프로젝트: 할 일 목록

앞에서 배운 스크립트 연결 방법을 적용하여 **할 일 목록 프로젝트의 기본 화면과 초기화 코드**를 작성한다. HTML로 목록을 표시하고, 외부 자바스크립트가 실행되면 콘솔에 시작 메시지를 출력하고 화면의 준비 상태를 변경한다.

이번 실습을 시작으로 변수, 배열, 함수, DOM과 이벤트를 학습하면서 할 일 추가·수정·삭제 등의 기능을 단계적으로 완성한다.

### 실습 목표

- HTML과 자바스크립트를 별도의 파일로 작성하고 상대 경로로 연결한다.
- `defer`를 이용하여 DOM 요소가 준비된 뒤 초기화 코드를 실행한다.
- 콘솔 출력과 화면의 안내 문구로 스크립트 실행 결과를 확인한다.
- 파일 경로 오류와 DOM 요소 접근 오류를 구분하고 수정한다.

### 구현할 내용

| 항목 | 요구 사항 |
| --- | --- |
| 기본 화면 | 제목과 미리 작성한 할 일 3개를 표시한다. |
| 스크립트 연결 | `<head>`에서 외부 자바스크립트 파일을 연결하고 `defer`를 지정한다. |
| 시작 메시지 | 자바스크립트가 실행되면 콘솔에 프로그램 시작 메시지를 출력한다. |
| 준비 상태 | DOM 요소의 텍스트를 변경하여 화면이 준비되었음을 알린다. |

### 파일 구성

`ch01/lab01` 디렉터리에 다음 두 파일을 작성한다.

```text
lab01/
├── todo.html
└── todo.js
```

### 1단계: todo.html 작성하기

할 일 목록과 준비 상태를 표시할 요소를 작성한다. `<head>`에는 `todo.js`를 연결하는 `<script>` 태그를 배치한다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>할 일 목록</title>
  <script src="todo.js" defer></script>
</head>
<body>
  <h1>할 일 목록</h1>
  <p id="status">프로그램을 준비하고 있습니다.</p>

  <ul id="todo-list">
    <li>HTML 문서 만들기</li>
    <li>외부 스크립트 연결하기</li>
    <li>스크립트 실행 순서 확인하기</li>
  </ul>
</body>
</html>
```

- `<ul>`과 `<li>`로 초기 할 일 목록을 표시한다.
- `id="status"`는 자바스크립트에서 준비 상태를 표시할 요소를 찾을 때 사용한다.
- `defer`를 지정하여 `<p id="status">`를 포함한 DOM 트리가 만들어진 뒤 `todo.js`를 실행한다.

### 2단계: todo.js 작성하기

외부 파일이 실행되었는지 확인할 시작 메시지를 출력하고, 준비 상태의 텍스트를 변경한다.

```javascript
console.log("할 일 목록 프로그램을 시작합니다.");

document.querySelector("#status").textContent = "할 일 목록을 사용할 준비가 되었습니다.";

console.log("할 일 목록 화면 초기화 완료");
```

`document.querySelector("#status")`로 안내 문구 요소를 찾고, `textContent`에 새 문자열을 지정한다. `todo.js`에는 `<script>` 태그를 작성하지 않는다.

### 3단계: 실행 결과 확인하기

1. 두 파일을 같은 디렉터리에 저장한다.
2. `todo.html`을 브라우저로 연다.
3. 개발자 도구의 **Console** 탭을 열고 페이지를 새로고침한다.
4. 다음 메시지가 순서대로 출력되는지 확인한다.

```text
할 일 목록 프로그램을 시작합니다.
할 일 목록 화면 초기화 완료
```

화면에는 다음 내용이 표시된다.

```text
할 일 목록
할 일 목록을 사용할 준비가 되었습니다.

• HTML 문서 만들기
• 외부 스크립트 연결하기
• 스크립트 실행 순서 확인하기
```

할 일 3개는 HTML에 작성한 내용이고, 준비 완료 문구는 자바스크립트가 변경한 결과다. 시작 메시지, 준비 상태 변경, 초기화 완료 메시지가 코드에 작성한 순서대로 처리된다.

### 4단계: 오류를 발생시키고 수정하기

각 항목은 정상 동작하는 원래 코드에서 시작한다. 한 가지씩 변경하여 결과를 확인하고, 원인을 설명한 뒤 원래 코드로 복원한다.

| 변경 내용 | 예상 결과 | 수정 방법 |
| --- | --- | --- |
| `src="todo.js"`를 존재하지 않는 파일 이름으로 변경한다. | 파일 로드 오류가 발생하고, 콘솔의 시작 메시지와 화면 초기화가 모두 실행되지 않는다. | 실제 파일 이름과 상대 경로에 맞게 `src`를 수정한다. |
| `<head>`의 `<script>`에서 `defer`를 제거한다. | 시작 메시지는 출력되지만, 아직 `#status` 요소가 없어 `TypeError`가 발생한다. 초기화 완료 메시지는 출력되지 않는다. | `defer`를 다시 추가하여 DOM이 준비된 뒤 실행한다. |
| HTML의 `id="status"`만 `id="state"`로 변경한다. | `defer`가 있어도 `#status`에 맞는 요소를 찾지 못하여 `TypeError`가 발생한다. | HTML의 `id`와 자바스크립트의 선택자를 일치시킨다. |

오류가 발생하면 콘솔에서 **오류가 발생한 파일과 줄 번호**를 확인한다. 시작 메시지가 출력되었는지도 함께 살펴보면, 파일을 불러오는 단계와 코드 실행 중 어느 단계에서 문제가 발생했는지 파악하는 데 도움이 된다.

### 선택 실습: DOMContentLoaded로 초기화하기

정상 동작하는 코드로 복원한 뒤, `todo.html`의 `<script>`에서 `defer`를 제거한다.

```html
<script src="todo.js"></script>
```

`todo.js`의 전체 내용을 다음 코드로 교체한다. DOM을 변경하는 코드를 이벤트 리스너 안으로 옮긴다.

```javascript
console.log("할 일 목록 프로그램을 시작합니다.");

document.addEventListener("DOMContentLoaded", function() {
  document.querySelector("#status").textContent = "할 일 목록을 사용할 준비가 되었습니다.";
  console.log("할 일 목록 화면 초기화 완료");
});
```

파일을 저장하고 페이지를 새로고침하면 앞선 실습과 같은 화면과 콘솔 출력을 확인할 수 있다. 이번에는 `<head>`에서 스크립트가 실행되어 시작 메시지를 출력하고 리스너를 등록한 뒤, DOM이 완성되면 등록한 함수가 화면을 초기화한다.

### 완료 확인

- [ ] 외부 파일을 연결하여 콘솔에 시작 메시지를 출력했다.
- [ ] 할 일 목록과 준비 완료 문구가 화면에 표시된다.
- [ ] 새로고침할 때 시작 메시지와 초기화 완료 메시지가 각각 한 번 출력된다.
- [ ] 파일 경로, 실행 시점, 선택자 오류를 각각 확인하고 수정했다.
- [ ] `defer`가 없는 `<head>`의 일반 스크립트에서 DOM 요소에 바로 접근하면 오류가 발생하는 이유를 설명할 수 있다.
