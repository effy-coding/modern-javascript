# 소개 💁‍♂️

<p align="center">
    <img src="Modern JS.png" width="150" title="Modern JS">
</p>

<img src="https://cdn.rawgit.com/standard/standard/master/badge.svg" width="75" title="Standard JS">

<img src="https://camo.githubusercontent.com/567e52200713e0f0c05a5238d91e1d096292b338/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f65732d362b2d627269676874677265656e2e737667" width="60" title="ES6+">

모던 자바스크립트를 소개합니다. 모던 자바스크립트는 점차 현대적이고 실용적으로 변화하고 있는 ECMAScript 에 적응하는 방법을 알려줍니다.

현재 글을 쓰고 있는 시점인 2020년의 자바스크립트는 이미 풍부한 표준 내장 객체(Standard built-in objects)들을 가지고 있습니다. 덕분에 우리는 더 이상 전통적이고 거대한 라이브러리(lodash 와 같은)들을 사용할 필요성이 줄어들고 있습니다.

*참고: 모던 자바스크립트는 ES6+ 환경에서 진행됩니다.*

## 모던 자바스크립트의 목적

- 개발자의 실수 줄이기
- 의도치 않은 동작 방지
- 간결하고 읽기 쉬운 코드 작성

## 모던 자바스크립트가 좋아하는 것 👍

- 단순함
- 가벼움
- 본질
- MDN 문서
- TC39

## 모던 자바스크립트가 싫어하는 것 👎

- 복잡함
- 무거움
- 장황함 (Verbose)
- 불필요한 라이브러리 사용

모던 자바스크립트는 아래 개념들의 본질을 최대 한두 문장 이내로 간단 명료하게 설명합니다. 본질을 꿰뚫으면 길게 설명할 필요도 없습니다.

## 모던 자바스크립트가 다루는 개념

[Let Declaration](#let-declaration)

Const Declaration

[Spread Syntax](#spread-syntax)

[Destructuring Assignment](#destructuring-assignment)

[Rest Parameters](#rest-parameters)

[Rest Elements](#rest-elements)

[Default Parameter](#default-parameter)

[Named Parameter](#named-parameter)

[Promise](#promise)

[Async Function](#async-function)

---

# Let Declaration

### 정의

블록 레벨 유효 범위를 갖는 지역 변수를 선언합니다.

### 특징

`let` 으로 선언한 변수는 재선언할 수 없습니다.

`let` 으로 선언한 변수는 값을 재할당 할 수 있습니다.

`let` 은 블록 레벨 유효 범위를 가집니다. 블록 레벨은 모든 코드 블록을 의미합니다. 예를 들어 `함수`, `if`, `switch`, `for`, `while`, `try-catch`, ... 내부에서 선언된 `let` 변수는 외부에서 접근할 수 없습니다.

### 참고

`let` 으로 선언된 변수들은 코드를 읽을 때 변함(`mutable`)을 기대하게 만듭니다. 이는 코드 리뷰어가 해당 변수가 언제 바뀌는지 계속해서 추적하게 만듭니다. 이는 굉장히 피곤한 일입니다. 따라서 변하지 않는 변수들은 **언제나** 상수(`const`)로 선언하세요.

### 1. 재선언 불가능

👉시나리오: `let` 이 `var` 과 다르게 재선언 할 수 없다는 걸 증명하세요.

```js
var a
var a // 재선언 통과

let b
let b // -> SyntaxError: Identifier 'b' has already been declared 👍
```

### 2. 재할당

👉시나리오: `let` 이 `const` 와 다르게 재할당 가능한 걸 증명하세요.

```js
let a = 1
a = 2 // 재할당 가능 👍

const b = 1
b = 2 // -> TypeError: Assignment to constant variable.
```

### 3. 유효 범위

👉시나리오: `let` 이 `var` 과 다르게 블록 유효 범위를 가지는걸 증명하세요.

```js
if (true) {
  var a = 1
  let b = 1
}

console.log(a) // -> 1
console.log(b) // -> ReferenceError: b is not defined 👍
```

---

# Spread Syntax

### 정의

Spread Syntax 는 특정 객체 혹은 배열의 나머지 요소들을 어떤 객체 혹은 배열 리터럴에 풀어 놓습니다.

### 특징

Spread Syntax 로 새로운 변수가 선언되지 않습니다.

## 1. 배열 전개구문

## 활용 1: 안전한 복사

👉 시나리오:  `arr1` 과 똑같은 `arr2` 를 만드세요.

👉 조건:  `arr1` 의 변형이 `arr2` 에 영향을 미치지 않게 하세요.

### 불—편

```js
const arr1 = [1, 2, 3]
const arr2 = arr1 // arr2 는 arr1 를 참조합니다.

arr1.push(4)

console.log(arr1) // -> [ 1, 2, 3, 4 ]
console.log(arr2) // -> [ 1, 2, 3, 4 ] arr2 까지 변형됐습니다. 👎
```

### 편—안 ✅

```js
const arr1 = [1, 2, 3]
const arr2 = [...arr1] // arr1 를 구조 분해시켜 '새로운' 배열을 만듭니다. 👍

arr1.push(4)

console.log(arr1) // -> [ 1, 2, 3, 4 ]
console.log(arr2) // -> [ 1, 2, 3 ] 👍
```

## 2. 객체 전개구문

## 활용 1: 안전한 객체 얕은 병합

👉 시나리오: 주어진 `obj1` 에 `obj2` 를 병합하세요.

👉 조건1: 중복 필드가 있다면 `obj2` 의 값으로 덮어씌우세요.

👉 조건2: 주어진 객체를 변형하지 마세요.

### 불—편

```js
const obj1 = { a: 1, b: 1 }
const obj2 = { b: 2, c: 2 }

// 나쁘지 않습니다만 obj1 이 수정됩니다. 👎
const merged = Object.assign(obj1, obj2)
```

### 편—안 ✅

```js
const obj1 = { a: 1, b: 1 }
const obj2 = { b: 2, c: 2 }

// 모든 조건을 만족하며 심플합니다! 👍
const merged = { ...obj1, ...obj2 }
```

---

# Destructuring Assignment

### 정의

배열이나 객체의 속성을 해체하여 그 값을 **개별 변수**에 담습니다.

## 1. 객체 구조 분해 할당

객체를 구조 분해해봅시다.

## 활용 1: 기본값 할당

👉 시나리오: `me` 객체를 출력하세요.

👉 조건: `me.car` 가 존재하지 않으면 `없음` 을 출력하세요. 

### 불—편

```js
const me = {
  firstName: '당근',
  lastName: '손',
  hobby: ['🐳', '🍏', '🎒', '🌼'],
  location: '지구',

  car: '테슬라'
}

// 윽... 내 눈...!
console.log('이름', me.lastName, me.firstName)
console.log('지역', me.location)
console.log('취미', me.hobby)

// 더 심플하게 안될까요?
if (me.car) {
  console.log('자동차', me.car)
} else {
  console.log('자동차', '없음')
}
```

### 편—안 ✅

```js
const me = {
  firstName: '당근',
  lastName: '손',
  hobby: ['🐳', '🍏', '🎒', '🌼'],
  location: '지구',

  car: '테슬라'
}

const { firstName, lastName, hobby, location, car = '없음' } = me // 좋습니다. 👍

console.log('이름', lastName, firstName)
console.log('지역', location)
console.log('취미', hobby)
console.log('자동차', car)
```

## 2. 배열 구조 분해 할당

배열을 구조 분해해봅시다.

👉 시나리오: 배열 `arr` 의 첫 번째와 세 번째 원소를 각각 `first` 와 `third` 변수에 할당 및 선언하세요.

### 불—편

```js
const arr = [1, 2, 3, 4]

const first = arr[0] // 좀 더 심플하게 안될까요?
const third = arr[2] // 인덱스로 접근하지 않으면서요
```

### 편—안 ✅

```js
const arr = [1, 2, 3, 4]

const [first, , third] = arr // 좋습니다. 👍
```

---

# Rest Parameters

### 정의

Rest Parameters 는 정해지지 않은 수 매개변수를 **실제 배열**로 나타낼 수 있게 합니다.

## 1. 나머지 매개변수 예시

👉 시나리오: 개수가 정해지지 않은 매개변수를 입력 받아 이의 총 합을 구하는 함수를 작성하세요.

### 불—편
    
```js
function sum (a, b, ... y, z) {
  return a + b + ... + y + z
} // [a-z] 까지 입력받을 수 있습니다. 즉 매개변수의 개수가 정해져 있습니다. 👎
    
console.log(sum(1, 2, 3, 4, 5, 6, 7, 8)) // [i-z] 까지 undefined, 때문에 NaN 출력 👎
```

### 편—안 ✅

```js
function sum (...params) {
  return params.reduce((prev, curr) => prev + curr)
} // 매개변수가 몇 개든 합을 잘 구합니다.

console.log(sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)) // -> 55 👍
```

## 2. 또 다른 나머지 매개변수 예시

👉 시나리오: 첫번째 매개변수와 나머지 매개변수를 곱한 배열을 반환하는 함수를 작성하세요.

### 편—안 ✅

```js
function multiply (multiplier, ...params) {
  return params.map(param => multiplier * param)
}

console.log(multiply(5, 1, 2, 3, 4)) // -> [ 5, 10, 15, 20 ] 👍
```

## 3. arguments 와 다른점

`arguments` 는 유사 배열입니다. 때문에 `map, reduce, forEach` 와 같은 배열 메소드를 호출 할 수 없습니다.

```js
function sum (a, b, c) {
  // -> TypeError: arguments.reduce is not a function
  return arguments.reduce((prev, curr) => prev + curr)
}

sum(1, 2, 3)
```

---


# Rest Elements

### 정의

Rest Elements 는 객체나 배열의 나머지 요소들을 선언과 동시에 할당합니다.

### 특징

나머지 요소에서 명시한 변수명이 선언되는 효과를 가지며 구조 분해 할당과 함께 사용 시 맨 마지막에 한 번만 위치할 수 있습니다.

## 활용 1: 배열 나머지 요소

👉 시나리오:  `arr` 배열의 첫번째 요소를 `first` 에 할당하고 나머지 요소를 `rest` 에 풀어 넣으세요.

### 편—안 ✅

```js
const arr = [1, 2, 3, 4, 5]
const [first, ...rest] = arr

console.log(first) // -> 1
console.log(rest) // -> [ 2, 3, 4, 5 ] 👍
```

## 활용 2: 객체 나머지 요소

👉 시나리오: `me` 객체의 `name` 필드만 `name` 에 할당하고 나머지 요소를 `rest` 에 풀어 넣으세요.

### 불—편

```js
const me = {
  name: '손당근',
  hobby: '산책',
  location: 'earth'
}

// 더 심플하게 안될까요?
const name = me.name
const rest = {
  hobby: me.hobby,
  location: me.location
}
```

### 편—안 ✅

```js
const me = {
  name: '손당근',
  hobby: '산책',
  location: 'earth'
}

const { name, ...rest } = me // 👍
```

---

# Default Parameter

### 정의

Default Parameter 는 매개변수에 기본 값을 할당합니다.

## 활용1: 매개변수 기본 값

👉 시나리오: 이름 그리고 나이를 출력하는 함수 `aboutMe` 를 작성하세요.

👉 조건 1: 나이를 매개변수로 넘겨주지 않았다면 `비공개` 로 출력하세요.

👉 조건 2:  `0 살` 도 입력 가능하도록 만드세요.

### 불—편

```js
function aboutMe (name, age) {
  // 윽... 내 눈...! 👎
  // 더 심플하게 안 될까요?
  age = (typeof age !== 'undefined') ? age : '비공개'

  console.log(name, age)
}

aboutMe('손당근') // -> 손당근 비공개
aboutMe('손당근', 0) // -> 손당근 0
aboutMe('손당근', 20) // -> 손당근 20
```

### 불—편

```js
function aboutMe (name, age) {
  if (!age) { //  0 은 '거짓 값' 이기 때문에 0 살을 입력해도 '비공개'를 출력합니다. 👎
    age = '비공개'
  }

  console.log(name, age)
}

aboutMe('손당근') // -> 손당근 비공개
aboutMe('손당근', 0) // -> 손당근 비공개 ... 0 살을 입력했지만 '비공개'가 출력됐네요? 👎
aboutMe('손당근', 20) // -> 손당근 20
```

### 편—안 ✅

```js
function aboutMe (name, age = '비공개') { // 모든 조건을 만족하며 심플합니다. 👍 
  console.log(name, age)
}

aboutMe('손당근') // -> 손당근 비공개
aboutMe('손당근', 0) // -> 손당근 0
aboutMe('손당근', 20) // -> 손당근 20
```

## 🤷‍♀️ 하지만...

이름은 입력하지 않고 나이만 입력하고 싶다면 어떻게 해야할까요? 그건 불가능합니다.

왜냐면 `aboutMe` 의 매개변수는 **순서가 정해진** 매개변수이기 때문이죠. 

### 불—편

    aboutMe(, 10) // 이런건 불가능합니다.
    aboutMe(10) // 그렇다고 이렇게 하자니 name 매개변수에 숫자 10 이 들어가버립니다.

## 🤷‍♂️ 해결법은요?

**Destructuring Assignment** 를 활용하면 자바스크립트에서도 **Named Parameter** 를 사용할 수 있습니다. 즉 매개변수에 순서가 없어지고 이름이 붙게 되는 거죠.

이 개념은 다음 단원에서 다룹니다. 그러니 맛보기 코드만 훑고 넘어가 보죠.

### 편—안 ✅

```js
function aboutMe ({ name = '비공개', age = '비공개' }) { // '순서' 가 사라진 매개변수들 👍👍👍
  console.log(name, age)
}

aboutMe({ name: '손당근', age: 20 }) // -> 손당근 20
aboutMe({ age: 20, name: '손당근' }) // -> 손당근 20 ... 심지어 매개변수 순서까지 바껴도 잘 출력합니다!

aboutMe({ name: '손당근' }) // -> 손당근 비공개 ... 나이를 넘기지 않았으므로 '비공개' 를 출력합니다.
aboutMe({ age: 20 }) // -> 비공개 20

aboutMe({}) // -> 비공개 비공개 ... 둘 다 '비공개' 로 출력 가능합니다!
```

---

# Named Parameter

### 정의

Named Parameter 는 매개변수에 이름을 지정해줍니다.

## 활용1: 선택적 매개변수

👉 시나리오: 나이와 직업을 매개변수로 받고 출력하는 함수 `aboutMe` 를 작성하세요.

👉 조건 1: 나이만 입력할 수도, 직업만 입력할 수도, 둘 다 입력할 수도 그리고 둘 다 입력 안 할 수도 있게 함수를 작성하세요.

### 불—편

```js
// 매개변수에 순서가 정해져있기 때문에 조건을 만족하지 못합니다. 👎
function aboutMe (age, job) {
  age && console.log('나이', age)
  job && console.log('직업', job)
}

aboutMe(20) // -> 나이 20 ... 나쁘지 않아 보입니다. 하지만 다음 라인을 봐보세요.
// 함수가 기대한 것처럼 작동하지 않습니다!
aboutMe('개발자') // -> 나이 개발자 ... 나이가 개발자라고 나왔군요. 👎
```

### 편—안 ✅

```js
// Named Parameter 덕분에 조건을 만족합니다.
// 선택적으로 매개변수를 입력할 수 있습니다. 👍
function aboutMe ({ age, job }) {
  age && console.log('나이', age)
  job && console.log('직업', job)
}

// 나이와 직업 중 하나만 입력도 가능합니다. 👍
aboutMe({ age: 20 }) // -> 나이 20
aboutMe({ job: '개발자' }) // -> 직업 개발자
```

## 활용2: 순서 없는 매개변수

매개변수가 3개 이상이면 순서를 잘못 입력할 가능성이 큽니다.

특히 매개변수에 타입을 지정 할 수 없기때문에 아무런 경고 없이 함수가 실행되고, 예상치 못한 일이 발생하겠죠.

이를 방지하려면 순서 없는 매개변수를 사용해야합니다.

👉 시나리오: 창 이름, 가로, 세로 사이즈, 배경색, 테두리 색, 테두리 두께 그리고 폰트를 입력 받아 새로운 창을 렌더링하는 함수를 작성하세요.

### 불—편

```js
function render (name, width, height, backgroundColor, borderColor, borderWidth, font) {
  // doSomething ...
}

// 매개변수 순서를 하나라도 틀리면 큰 일납니다.
render('새 창', 640, 480, 'red', 'green', 1, 'Apple Bold')
```

### 편—안 ✅

```js
function render ({ name, width, height, backgroundColor, borderColor, borderWidth, font }) {
  // doSomething ...
}

// 이제 매개변수 순서와 상관없이 안전하게 호출 가능합니다. 👍
render({
  height: 480,
  borderColor: 'green',
  backgroundColor: 'red',
  font: 'Apple Bold',
  name: '새 창',
  borderWidth: 1,
  width: 640
})
```

---

# Promise

### 정의

Promise 는 **비동기 작업**의 **상태**를 보관하고 추적합니다.

### 특징 (중요 🐉)

Promise 는 다음 중 하나의 **상태**를 가집니다.

- 대기(*pending)*: 이행하거나 거부되지 않은 초기 상태.
- 이행(*fulfilled)*: 연산이 성공적으로 완료됨.
- 거부(*rejected)*: 연산이 실패함.

[출처: [MDN - Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)]

Promise 는 주어진 콜백 함수를 **단 한 번** 실행하며 그 **결과**를 보관합니다. 이러한 특성 때문에 Promise 의 상태는 한 번 결정되면 수정되지 않습니다.

다시 말해 이미 소비(이행되거나 거부) 된 Promise 는 재사용 할 수 없습니다.

### 1. 성공 케이스 (fulfilled)

### 활용 1: 비동기 호출의 상태를 추적하기

👉시나리오: 3초 후 주문한 음식이 배달됩니다. 도착 시 이를 출력하세요.

👉조건1: 배달이 도착하기 전에는 절대로 빨래를 시작하지 마세요.

### 불—편

```js
function delivery () {
  setTimeout(() => { console.log('배달 도착') }, 3000)
}

// 배달을 요청합니다. (비동기 함수 호출)
// 그러나 배달의 현재 상태를 추적하지는 않습니다.
delivery() 
console.log('빨래 시작')

// 결과 👎
// -> 빨래 시작 ... 배달이 도착하기도 전에 빨래를 시작했습니다.
// -> 배달 도착
```


### 편—안 ✅

```js
function delivery () {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('배달 도착')
      resolve()
    }, 3000)
  })
}

delivery().then(() => {
  console.log('빨래 시작') // 배달이 도착 한 후 빨래를 시작합니다.
})

// 결과 👍
// -> 배달 도착
// -> 빨래 시작

```

### 2. 실패 케이스 (rejected)

👉시나리오: 3초 후 주문한 음식이 배달됩니다. 그러나 월요일은 휴무입니다.

👉조건1: 휴무일에는(배달 거부 시) 맥도날드에 방문하세요.

### 편—안 ✅

```js
function delivery (day) {
  return new Promise((resolve, reject) => {
    if (day === '월요일') {
      reject(new Error())
    }

    setTimeout(() => {
      resolve()
    }, 3000)
  })
}

delivery('월요일').then(() => {
  console.log('음식 받기')
}).catch(() => {
  console.log('맥도날드 방문하기')
})

// 결과 👍
// -> 맥도날드 방문하기
```
    

### 3. 일회성

Promise 는 비동기 함수 호출의 결과를 정확히 보관할 의무를 가집니다. 한 번 정해진 결과는 수정되지 않으며 이를 지키기 위해서 주어진 실행 함수(`executor`)를 단 한 번만 실행합니다.

👉시나리오: Promise 가 일회성이라는 것을 증명하세요.

```js
// new Promise 를 한순간 이미 프로미스 소진
const promise = new Promise((resolve) => {
  console.log('안녕')
  resolve()
})

promise.then()
promise.then()
promise.then()

// 결과 👍
// -> 안녕 ... 단 한 번 출력됨
```

---

# Async Function

### 정의

Promise 객체를 반환하는 함수

### 특징

Async Function 은 `await` 키워드를 사용 가능케 해줍니다. `await` 키워드는 Async Function 에서만 유효합니다.

`await` 키워드는 Async Function 이 끝나길(`return or throw`) 기다립니다.

Async Function 는 `return` 혹은 `throw` 문으로 끝낼 수 있습니다.

Async/Await 을 사용하면 일련의 비동기 작업을 비교적 쉽게 동기 방식으로 호출 가능케 해줍니다.

### 참고

예시 코드들은 간결한 코드를 보여주기 위해 `top-level-await` 기능이 탑재된 환경에서 진행했습니다. `top-level-await` 은 ECMAScript proposal stage 3 상태며 자세한 내용은 아래 링크를 참고해 주세요.

[tc39/proposal-top-level-await - GitHub](https://github.com/tc39/proposal-top-level-await)

### 1. 비동기 작업하기

👉시나리오: 구글 검색 페이지를 크롤링 해오세요. 크롤링을 완료하는 것을 기다리지 말고 `이메일을 전송` 하세요.

👉조건: Async Function 을 활용하세요.

### 편—안 ✅

```js
async function google () {
  // ...
  console.log('구글 크롤링 완료')
}

google() // 비동기적으로 실행됩니다.
console.log('이메일 전송')

// 결과 👍
// -> 이메일 전송
// -> 구글 크롤링 완료
```

### 2. 비동기 작업을 기다리기

👉시나리오: 구글 검색 페이지를 크롤링 해오세요. 크롤링을 완료한 뒤 `이메일을 전송` 하세요.

👉조건: Async Function 을 활용하세요.

### 편—안 ✅
```js
async function google () {
  // ...
  console.log('구글 크롤링 완료')
}

await google() // await 키워드가 동기적으로 실행하도록 만들어줍니다. 👍
console.log('이메일 전송')

// 결과 👍
// -> 구글 크롤링 완료
// -> 이메일 전송
```
### 3. 결과 다루기

### 활용 1: 성공 (return)

👉시나리오: `getUser` 함수로 사용자 정보를 가져오세요. 결과를 받아 `name` 필드의 값을 출력하세요.

👉조건: `await` 키워드로 `getUser` 함수가 반환하는 값을 받아오세요.

### 편—안 ✅

```js
async function getUser () {
  const result = await dbQuery()
  return result
}

const user = await getUser() // await 키워드가 getUser 함수의 반환을 기다립니다. 👍

console.log(user.name)
```

### 활용2: 실패 (throw)

👉시나리오: 배달 음식을 주문하세요. 배달이 도착하면(`return`) 음식을 먹고, 만약 주문이 취소되면(`throw`) 방문 포장을 해오세요.

### 편—안 ✅

```js
async function delivery () {
  // ...
  throw new Error('주문 취소!')
}

try {
  await delivery() // await 키워드가 덕분에 비동기 호출의 실패를 다룰수 있습니다. 👍
  // ... 배달 음식 먹기
} catch (error) {
  // ... 방문 포장 해오기
}
```
