# 1. 문서 객체 모델(DOM)

---

문서객체모델이란 자바스크립트를 이용하여 웹 문서에 접근하고 제어할 수 있도록 객체를 사용해 웹 문서를 체계적으로 정리하는 방법이다.

## 1.2 DOM 트리

DM은 웹 문서의 요소를 부모 - 자식 요소로 구분한다.

![image.png](attachment:3f124cf9-59f8-4a1f-9962-a1a62a4e629b:image.png)

---

## 1.3 DOM 요소에 접근하기

웹 문서에 있는요소를 찾아가는 것을 웹 요소에 접근한다고 한다.

### 1.3.1 `getElement-*` 함수 사용하기

CSS의 id 선택자, class 선택자, 타입 선택자에 따라 사용하는 함수가 달라진다.

```jsx
document.getElementById("id명")
document.getElementByClassName("class명")
document.getElementByTagName("태그명")
```

class 선택자나 타입 선택자는 여러번 사용할 수 있기 때문에 getElementsByClassName(), getElementsByTagName() 함수에서 반환하는 값이 2개 이상일 수 있다. 이런 값은 HTMLCollection 객체로 저장되며 배열처럼 사용 가능하다.

**<예제>**

```jsx
<p class="item"></p>
<p class="item"></p>
```

`document.getElementsByClassName("item")` 결과 = `[p, p]` (2개)

### 1.3.2 querySelector() 함수 사용하기

```jsx
document.querySelector(선택자)
```

id, class, 타입 선택자 뿐만 아니라 하위 선택자, 형제 선택자 등 CSS의 다양한 선택자를 모두 사용할 수 있다.

**기호**

- `#` : id 선택자
- `.` : class 선택자
- 둘 이상인 경우 그대로 사용

### 1.3.3 querySelectorAll() 함수 사용하기

```jsx
document.querySelectorAll(선택자)
```

id를 제외하고 선택자는 문서에서 여러번 사용되며 여러 요소에 접근할 수 있으며 이 때 사용한다.

---

## 1.4 DOM 요소 내용 가져오고 수정하기

### 1.4.1 내용 가져오기

DOM 요소에 접근할 때는 아래와 같은 속성을 사용한다.

- `요소.innerText` : 웹 브라우저 창에 보이는 내용만 가져온다.
- `요소.innerHTML` : HTML 코드를 모두 가져온다.
- `요소.textContent` : 요소의 모든 내용을 가져온다.

### 1.4.2 내용 수정하기

```jsx
요소.innerText = 내용
요소.innerHTML = 내용
요소.textContent = 내용
이미지요소.src = 파일 경로
```

---

## 1.5 DOM 요소 이벤트 처리하기(1)

### 1.5.1 DOM 요소에 함수 연결하기

**방법(1)**

```jsx
let cat = document.querySelector("#cat");
cat.onclick = () => alert("이벤트1")
```

**방법(2)**

```jsx

let cat = document.querySelector("#cat");
cat.onclick = changeImg;

function changeImg(){
	cat.src = "imges/kitty-2.png"
}
```

### 1.5.2 DOM의 event 객체

event객체는 DOM의 이벤트 정보를 저장한다. 이벤트가 발생한 요소가 무엇인지, 어떤 이벤트가 발생했는지 등의 정보가 들어있다.

- event 객체는 이벤트 정보만 담고 있기 때문에 이벤트가 발생한 대상에 접근하려면 this를 사용해야한다.

**속성**

| 프로퍼티 이름 | 설명 | 예시 |
| --- | --- | --- |
| `type` | 이벤트 유형(예: `"click"`, `"keydown"`) | `event.type` |
| `target` | 이벤트가 실제로 발생한 요소 | `event.target` |
| `currentTarget` | 이벤트 리스너가 연결된 요소 | `event.currentTarget` |
| `timeStamp` | 이벤트가 발생한 시각(ms) | `event.timeStamp` |
| `bubbles` | 이벤트가 버블링 되는지(Boolean) | `event.bubbles` |
| `cancelable` | 이벤트 취소가 가능한지 | `event.cancelable` |
| `defaultPrevented` | 기본 동작이 이미 취소되었는지 여부 | `event.defaultPrevented` |
| `key` *(키보드 이벤트)* | 눌린 키 문자 | `event.key` |
| `keyCode` *(키보드 이벤트 — 구식)* | 눌린 키의 코드 | `event.keyCode` |
| `clientX / clientY` *(마우스 이벤트)* | 브라우저 화면 내 좌표 | `event.clientX` |
| `pageX / pageY` *(마우스 이벤트)* | 문서 기준 좌표 | `event.pageX` |
| `offsetX / offsetY` *(마우스 이벤트)* | 이벤트가 발생한 요소 기준 좌표 | `event.offsetX` |
| `button` *(마우스 이벤트)* | 클릭된 버튼(0:좌,1:휠,2:우) | `event.button` |

**메서드**

| 메서드 이름 | 설명 | 예시 |
| --- | --- | --- |
| `preventDefault()` | 요소의 기본 동작을 막음 | `event.preventDefault()` |
| `stopPropagation()` | 이벤트 버블링을 중단 | `event.stopPropagation()` |
| `stopImmediatePropagation()` | 같은 요소의 다음 리스너도 막음 | `event.stopImmediatePropagation()` |
| `composedPath()` | 이벤트가 전달된 경로 반환 | `event.composedPath()` |

## 1.6 DOM 요소에서 이벤트 처리하기 (2)

앞선 방법은 1. HTML 태그 안에서 이벤트 처리기 연결하거나 2. DOM 요소에 함수를 직접 연결하는 방식이다.

이 방법은 유지 보수가 어렵고, 여러 이벤트 처리기를 추가할 수 없다.

이에 사용하는 방법이 `addEventListener()`이다.

```jsx
요소.addEventListener(이벤트, 함수, 캡처 여부);
```

- 이벤트 : 이벤트 유형 지정 (on을 붙이지 않는다.)
- 함수 : 이벤트 발생시 실행할 명령어나 함수 (함수 객체, 함수 둘 다 넣을 수 있다.)
- 캡처 여부 : true = 캡처링, false = 버블링

## 1.7 CSS 속성에 접근하기

```jsx
document.요소명.style.속성명
```

- color 같은 한 단어는 그대로 사용, background-coplor 같은 두 단어는 carmal로 변경해 사용한다.

# 2. DOM에서 노드 추가하기

---

## 2.1 노드 리스트

노드리스트란 querySelectorAll() 메서드를 사용해 노드를 여러 개 가져올 때 이를 저장한 것이 노드 리스트다. 배열은 아니지만 접근 방법은 배열과 유사하다.

```jsx
document.querySelectorAll("li")[i]
```

## 2.2 텍스트 노드를 갖는 노드 추가하기

- `document.createElement(요소명)` : 요소 노드 만들기
- `ducument.createText(내용)` : 텍스트 노드 만들기
- `부모노드.appendChild(자식노드)` : 자식 노드 연결하기
    - 기존 자식이 있는 경우 자식 노드의 맨 끝에 추가된다.

**<예시 - ul 요소에 li 추가하기>**

```jsx
// 1. 요소 추가
let newItem = document.createElement('li')

// 2. 텍스트 노드 만들기
let textNode = document.createTextNode("Typescript")

// 3. 요소 노드 - 텍스트 노드 연결
newItem.appendChild(textNode)

//4. 완성된 노드를 원하는 부모 노드와 연결
document.querySelector('ul').appendChild(newItem)

```

## 2.3 텍스트 노드를 갖지 않는 노드 추가하기

요소를 만들면 그와 관련된 속성 노드도 자식 노드로 연결해야 한다.

예로, <img>를 추가하면 src 속성도 넣어야 한다.

**<방법 1>**

```jsx
// 1. 요소 만들기
let newImg = document.createElement("img")

// 2. 속성 추가하기
newImg.src = "imges/books.png"

// 3. 자식 노드 연결하기
document.body.appendChild(newImg)
```

- 웹 표준 혹은 웹 브라우저에서 지원하는 태그의 속성에만 정의 가능

**<방법 2>**

```jsx
let newImg = document.createElement("img")

newImg.setAttribute('src', "imges/books.png")

document.body.appendChild(newImg)

```

- 웹 브라우저가 지원하지 않는 속성에도 적용 가능
- **속성 메서드 종류**
    - `setAttribute(name, value)` : 객체의 속성을 지정
    - `getAttribute(name)` : 객체의 속성 가져옴

***위 방법은 노드를 만들고 연결하는 방법이고 일반적으로는 innerHTML 속성을 이용해 더 간단하게 속성을 사용한다.***

# 3. DOM 에서 노드 삭제하기

---

- 노드.remove() : 노드를 삭제한다.

```jsx
// 1. 요소 접근하기
heading = document.querySelector('h1');

// 2. 삭제하기
heading.remove()
or
heading.removeChild()
```

# 4. Class 속성 추가/삭제하기

## 4.1 classList 속성

classList 속성은 해당 요소에 있는 class 속성을 다룰 때 사용한다.

```jsx
document.querySelector('요소')/querySelectorAll('요소').classList
```

### 4.1.1 classList 속성 추가/삭제하기

| 함수 | 설명 |
| --- | --- |
| add(클래스) | 클래스 추가 |
| remove(클래스) | 클래스 제거 |
| toggle(클래스명) | 클래스 있으면 제거, 없으면 추가 |
| contains(클래스명) | 클래스 있는지 확인 |

# 5. 추가

---

참고로 문서 객체 배열에는 for in을 사용하면 안 된다. 이를 이용해 출력하면 객체 의외의 속성에도 접근하기 때문이다.