# 1. HTML & Web 기본

## 1-1. HTML Form과 HTTP 메서드 우회(Method Override)

HTML `<form>` 태그는 **GET**과 **POST** 메서드만을 공식적으로 지원합니다.  
그러나 RESTful API 디자인에서는 **PUT**, **PATCH**, **DELETE**와 같은 메서드가 필요합니다.  
이를 브라우저에서 직접 보낼 수 없기 때문에, **히든 필드 기반 메서드 오버라이드(Method Spoofing)** 기법을 사용합니다.

---

### ❓ 왜 필요한가? `<form>`의 기본 한계

HTML의 form이 기본적으로 지원하는 method 값:

- **GET** → 조회(Read)
- **POST** → 생성(Create), 일반 데이터 제출

하지만 RESTful API 설계에서는 다음 메서드를 요구:

| 목적 | 메서드 |
|------|---------|
| 전체 수정 | PUT |
| 부분 수정 | PATCH |
| 삭제 | DELETE |

브라우저의 `<form>`은 이 메서드를 **직접 전송할 수 없음**.

---

### 💡 해결: Hidden 필드를 이용한 Method Spoofing

서버 프레임워크(Spring, Express 등)는 다음 규칙을 통해  
POST 요청을 PUT/PATCH/DELETE로 “변환”할 수 있음.

#### 핵심 원리

1. form의 method는 반드시 **POST**
2. hidden input에 `_method` 저장
3. 서버에서 `_method` 값을 읽어 실제 HTTP 메서드로 처리

---

### 📝 DELETE 요청 예시

```html
<form action="/posts/100" method="post">
    <input type="hidden" name="_method" value="DELETE">
    <button type="submit">게시물 삭제</button>
</form>
```


# 2. JavaScript 기초

## 2-1. JavaScript 코드 컨벤션
JavaScript 코드를 읽기 쉽고 유지보수하기 쉽게 만들기 위한 기본적인 작성 규칙입니다.  
(Do it! HTML+CSS+JavaScript 웹 표준의 정석 기반)

---

### 🔹 1) 들여쓰기(Indentation)
- 코드 블록 내부는 반드시 들여쓰기합니다.  
- 스페이스 2칸 또는 4칸 중 하나로 통일합니다.


function greet() {
    console.log("Hello!");
}


---

### 🔹 2) 세미콜론 사용
JavaScript에는 ASI(자동 세미콜론 삽입)가 존재하지만,  
예측 불가능한 오류를 방지하기 위해 **세미콜론 사용을 권장합니다.**


let x = 10;
console.log(x);

---

### 🔹 3) 가독성을 위한 공백(Whitespace)
✔ 연산자 주변에 공백 추가  
✔ 쉼표(,) 뒤에 공백 추가  
✔ 함수 선언 시 괄호 앞에는 공백을 넣지 않음  


let sum = a + b;

function add(a, b) {
    return a + b;
}


---

### 🔹 4) 주석(Comment)을 활용해 코드 설명
필요한 부분에는 주석을 추가하여 코드의 의도를 명확히 합니다.

**한 줄 주석**

// 사용자 이름 출력
function showName(name) {
    console.log(name);
}


**여러 줄 주석**

/*
  사용자 정보를 등록하는 함수
  <br> 작성자: 최종민
  <br> 작성일: 2025-12-10
*/


---

### 🔹 5) 식별자(변수명, 함수명) 작성 규칙
- camelCase 사용 → userName, totalPrice  
- 의미 있는 이름 사용  
- 함수 이름은 동사로 시작  
  - getUser(), calculateTotal(), renderList()  
- 상수(const)는 대문자 + 언더스코어 사용 → MAX_SIZE  

ex)
const MAX_SIZE = 100;


❌ 피해야 하는 이름  
- a, x1, data1 처럼 모호한 이름

---

### 🔹 6) 예약어(Reserved Words)는 식별자로 사용 불가
사용하면 오류 발생함.

예:  
break, case, catch, class, const, continue,  
default, delete, do, else, export, extends,  
finally, for, function, if, import, new,  
return, switch, this, throw, try, typeof,  
var, void, while, yield

let for = 10;    // ❌ 오류
let class = "A"; // ❌ 오류


---

## ✔ 정리
- 들여쓰기, 공백, 세미콜론, 주석, 변수명 규칙 등 기본적인 코드 품질 요소를 지키면 협업과 유지보수가 쉬워진다.



## 2-2. 함수 선언문(Function Declaration) vs 함수 표현식(Function Expression)

JavaScript에서 함수를 정의하는 기본 방식은 크게 두 가지이며,  
특히 **호이스팅(Hoisting)**에서 큰 차이가 존재합니다.

---

### 🔥 비교 요약

| 구분 | 함수 선언문 | 함수 표현식 |
|------|--------------|--------------|
| 형식 | function 키워드 + 함수 이름 | 함수를 변수에 할당 |
| 문법 | function f() { } | const f = function() { }; |
| 호이스팅 | 전체 함수가 호이스팅됨 | 변수 선언만 호이스팅됨 |
| 호출 시점 | 선언 전/후 모두 호출 가능 | 변수에 함수가 할당된 이후에만 호출 가능 |
| 사용 용도 | 기본적인 일반 함수 정의 | 콜백, 클로저, 고차 함수에 자주 사용 |

---

## 1) 함수 선언문 (Function Declaration)

전통적인 함수 정의 방식이며, 함수 이름을 반드시 포함합니다.

### ✔ 특징
- **함수 전체가 호이스팅됨**
- 따라서 **선언 전에도 호출 가능**
- 코드 상단에 공통 함수들을 배치할 때 유용

### ✔ 예시


// 선언보다 먼저 호출해도 문제 없음
logMessage("선언 전 호출"); // 출력: 선언 전 호출

// 함수 선언문
function logMessage(msg) {
    console.log(msg);
}


---

## 2) 함수 표현식 (Function Expression)

함수를 값(value)처럼 취급하여 변수에 할당하는 방식입니다.

### ✔ 특징
- 호이스팅 시 **변수 선언만 올라감**
  - 즉, 함수 정의는 호이스팅되지 않음
- 따라서 **할당되기 전에는 호출 불가능**
- 주로 const를 사용하여 재할당을 막음
- 콜백 함수나 익명 함수 형태로 많이 활용됨

### ✔ 예시


// 아래 코드는 오류 발생! (할당되기 전에 호출)
// calculateArea(5); 
// ❌ ReferenceError: Cannot access 'calculateArea' before initialization

// 함수 표현식
const calculateArea = function(width) {
    return width * width;
};

// 정상 호출
calculateArea(5);


---

## 💡 왜 함수 표현식을 더 선호할까?

- **호출 시점 명확**  
  → 함수가 정의되기 전에는 호출 불가 → 코드 흐름이 더 예측 가능함  
- **콜백 함수에서 필수적**  
  → addEventListener, map, filter 등에서 익명 함수 전달이 자연스러움  
- **화살표 함수(Arrow Function)과 함께 쓰기 편함**

---

## ✔ 정리

- **함수 선언문** → 호이스팅 O → 선언 전에 호출 가능  
- **함수 표현식** → 호이스팅 변수 선언만 O → 정의 이후에만 호출 가능  
- 요즘 개발에서는 **함수 표현식 + const + 화살표 함수** 조합을 표준처럼 사용함



## 2-3. JavaScript 변수 사용법

JavaScript에서 변수는 코드의 안정성과 가독성에 매우 큰 영향을 미칩니다.  
따라서 다음 규칙을 지키는 것이 권장됩니다.

---

### 🔹 1) 전역 변수는 최소한으로 사용한다

#### ✔ 이유
- 전역 변수는 **모든 스코프에서 접근 가능**하다는 특징 때문에  
  코드가 길어질수록 **어디서 값이 변경되었는지 추적하기 어려움**  
- 변수명이 충돌하거나 실수로 덮어쓰기(overwrite)하기 쉬움  
- 메모리 해제 시점이 불명확해져 **예기치 않은 버그**가 발생할 가능성이 높음  
- 스코프 오염(Scope Pollution) → 유지보수성 저하

➡ **가능한 지역 변수(let, const)를 사용하여 스코프를 제한하는 것이 안정적**

---

### 🔹 2) 변수를 선언할 때는 var보다 let 혹은 const를 사용한다

#### ✔ 이유
- `var`는 **함수 스코프(function scope)**  
  → 블록 스코프 무시 `{}` 안에서도 밖에서 접근 가능 (예상치 못한 동작)
- `var`는 **중복 선언 허용**  
  → 실수로 동일한 변수명을 선언해도 오류가 나지 않아 버그의 원인
- `var` 선언은 **호이스팅(hoisting)이 안전하지 않음**  
  값이 undefined로 초기화되기 때문에 예측이 어려움

반면

- `let` → 블록 스코프 + 재할당 가능  
- `const` → 블록 스코프 + 재할당 불가  
- 중복 선언 불가  
- 호이스팅은 되지만 TDZ(Temporal Dead Zone) 때문에 안전함

➡ **요약: var는 위험하고 예측 불가능 → 현대 JS에서는 let/const가 표준**

---

### 🔹 3) for문의 카운터 변수는 let으로 선언한다

#### ✔ 이유
- `let`은 블록 스코프이므로 **반복문 내부에서만 사용됨**  
- 반복마다 새로운 스코프가 생성되기 때문에  
  `i` 변수가 반복마다 안전하게 유지됨 (클로저에서 특히 중요)
- `var`를 사용하면 i가 **함수 스코프**로 묶여서  
  비동기 코드(setTimeout 등)에서 의도치 않은 값이 출력되는 문제가 발생함

예시:


// var 사용 → 의도와 다르게 동작
for (var i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// 출력: 4 4 4

// let 사용 → 정상적으로 동작
for (let i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// 출력: 1 2 3


➡ **for 카운터에는 반드시 let을 사용해야 안전**

---

## ✔ 요약

- **전역 변수는 위험함 → 최소로 사용하기**
- **var는 오래된 방식 → let과 const 사용하기**
- **반복문 카운터는 let 사용이 필수적**

JavaScript의 스코프와 호이스팅 이해를 기반으로 변수 사용 규칙을 지키면  
버그가 크게 줄고 코드가 훨씬 예측 가능해짐.


## 2-4. DOM 요소 접근 & 조작

### 4-1. DOM 요소 선택 방법

### getElementById()
- id로 요소 1개 선택
- 가장 빠르고 단순한 선택 방식

const title = document.getElementById("main-title")

### getElementsByClassName()
- 클래스 이름으로 여러 요소 선택
- 반환: HTMLCollection (Live)

const items = document.getElementsByClassName("menu-item");

### getElementsByTagName()
- 특정 태그 이름으로 요소 선택
- 반환: HTMLCollection

const paragraphs = document.getElementsByTagName("p");

### querySelector()
- CSS 선택자로 요소 선택

const header = document.querySelector("#header");
const firstItem = document.querySelector(".item");

### querySelectorAll()
- CSS 선택자로 여러 요소 선택
- 반환: NodeList (forEach 사용 가능)

const listItems = document.querySelectorAll("ul li");


### 4-2. DOM 내용 변경하기

### innerText
- 화면에 보이는 텍스트만 변경

title.innerText = "새로운 제목";

### textConten
- 숨겨진 텍스트 포함 모든 텍스트 변경

title.textContent = "새로운 제목";

### innerHTML
- HTML 포함하여 내부 구조 변경 가능

content.innerHTML = "<strong>강조된 텍스트</strong>";


### 4-3. DOM 속성 & 스타일 변경하기

### 일반 속성 변경
- HTML 요소의 기본 속성(src, href, value 등)을 변경할 때 사용합니다.

img.src = "profile.png";
link.href = "/home";
input.value = "기본값";

### getAttribute / setAttribute
- 커스텀 속성(data-*) 또는 일반 HTML 속성을 다룰 때 사용합니다.

box.setAttribute("data-id", "10");
const id = box.getAttribute("data-id");

### classList (클래스 제어)
- 요소의 CSS 클래스를 추가/삭제할 때 사용합니다.
- 스타일 제어 시 가장 많이 사용하는 방식입니다.

#### 주요 메서드
주요 메서드

| 메서드 | 기능 | 반환값 | 예시 코드 |
| :--- | :--- | :--- | :--- |
| **`add()`** | 지정된 클래스를 추가합니다. 이미 존재하는 클래스는 무시됩니다. | `undefined` | `element.classList.add("active");` |
| **`remove()`** | 지정된 클래스를 제거합니다. | `undefined` | `element.classList.remove("active");` |
| **`toggle()`** | 지정된 클래스가 있으면 제거하고, 없으면 추가합니다. | `boolean` (클래스가 추가되었으면 `true`, 제거되었으면 `false`) | `element.classList.toggle("active");` |
| **`contains()`**| 지정된 클래스가 현재 요소의 `classList`에 포함되어 있는지 확인합니다. | `boolean` (포함되어 있으면 `true`, 아니면 `false`) | `element.classList.contains("active");` |


### style (inline 스타일 변경)
- 요소의 inline CSS를 직접 변경합니다.
- CSS 속성명은 카멜케이스(camelCase) 사용

box.style.color = "red";
box.style.backgroundColor = "black";
box.style.fontSize = "20px";


### style.cssText (여러 스타일을 한 번에 설정)
- 여러 CSS 속성을 한 번에 입력할 수 있습니다.

box.style.cssText = `
  color: blue;
  background: yellow;
  padding: 10px;
`