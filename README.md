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

[Rest Parameters](#rest-parameters)

[Destructuring Assignment](#destructuring-assignment)

[Default Parameter](#default-parameter)

[Named Parameter](#named-parameter)

[Truthy and Falsy](#truthy-and-falsy)

Array.prototype.map

Array.prototype.reduce

Array.prototype.filter

Array.prototype.includes

Array.prototype.every

Array.prototype.some

String.prototype.split

String.prototype.join

Object.entries

Object.values

Object.keys

---

# Rest Parameters

### 정의

Rest Parameters 는 정해지지 않은 수 매개변수를 **실제 배열**로 나타낼 수 있게 합니다.

## 1. 나머지 매개변수 예시

👉시나리오: 개수가 정해지지 않은 매개변수를 입력 받아 이의 총 합을 구하는 함수를 작성하세요.

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

👉시나리오: 첫번째 매개변수와 나머지 매개변수를 곱한 배열을 반환하는 함수를 작성하세요.

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

# Destructuring Assignment

### 정의

구문은 배열이나 객체의 속성을 해체하여 그 값을 **개별 변수**에 담습니다.

## 1. 객체 구조 분해 할당

객체를 구조 분해해봅시다.

### 활용 1: 기본값 할당

👉시나리오: `me` 객체를 출력하세요.

👉조건: `me.car` 가 존재하지 않으면 `없음` 을 출력하세요. 

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

### 활용 1: 안전한 복사

👉시나리오:  `arr1` 과 똑같은 `arr2` 를 만드세요.

👉조건:  `arr1` 의 변형이 `arr2` 에 영향을 미치지 않게 하세요.

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

### 활용 2: 또 다른 안전한 복사

👉시나리오: 주어진 `fruits` 배열을 이용해 첫번째 과일과 나머지 과일을 출력하세요.

### 불—편

```js
const fruits = ['🍎', '🍌', '🥝']

// apple 과 rest 는 fruits 를 참조합니다.
const apple = fruits.shift() // fruits 의 첫번째 원소를 제거하고, apple 에 할당
const rest = fruits

console.log('첫번째 과일', apple) // -> 🍎
console.log('나머지 과일', ...rest) // -> 🍌 🥝

console.log(fruits) // -> [ '🍌', '🥝'] fruits 배열이 변형됨 👎
```

### 편—안 ✅

```js
const fruits = ['🍎', '🍌', '🥝']

const [apple, ...rest] = fruits // 배열을 구조 분해시켜 apple 과 rest 가 fruits 를 참조하지 않도록 해줍니다.

console.log('첫번째 과일', apple)  // -> 🍎
console.log('나머지 과일', ...rest)  // -> 🍌 🥝

console.log(fruits) // -> [ '🍎', '🍌', '🥝' ] fruits 배열 변형 안됨 👍
```

---

# Default Parameter

### 정의

Default Parameter 는 매개변수에 기본 값을 할당합니다.

## 1. 매개변수 기본 값

👉시나리오: 이름 그리고 나이를 출력하는 함수 `aboutMe` 를 작성하세요.

👉조건 1: 나이를 매개변수로 넘겨주지 않았다면 `비공개` 로 출력하세요.

👉조건 2:  `0 살` 도 입력 가능하도록 만드세요.

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
aboutMe('손당근') // -> 손당근 0
aboutMe('손당근') // -> 손당근 20
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
aboutMe({ age: 20 }) // -> 비공개 비공개

aboutMe({}) // -> 비공개 비공개 ... 둘 다 '비공개' 로 출력 가능합니다!
```

---

# Named Parameter

### 정의

Named Parameter 는 매개변수에 이름을 지정해줍니다.

### 활용1: 선택적 매개변수

👉시나리오: 나이와 직업을 매개변수로 받고 출력하는 함수 `aboutMe` 를 작성하세요.

👉조건 1: 나이만 입력할 수도, 직업만 입력할 수도, 둘 다 입력할 수도 그리고 둘 다 입력 안 할 수도 있게 함수를 작성하세요.

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

### 활용2: 순서 없는 매개변수

매개변수가 3개 이상이면 순서를 잘못 입력할 가능성이 큽니다.

특히 매개변수에 타입을 지정 할 수 없기때문에 아무런 경고 없이 함수가 실행되고, 예상치 못한 일이 발생하겠죠.

이를 방지하려면 순서 없는 매개변수를 사용해야합니다.

👉시나리오: 창 이름, 가로, 세로 사이즈, 배경색, 테두리 색, 테두리 두께 그리고 폰트를 입력 받아 새로운 창을 렌더링하는 함수를 작성하세요.

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

# Truthy and Falsy

참과 거짓은 Boolean 을 기대하는 문맥에서 `true` `false` 로 평가되는 값입니다.

## 1. 거짓 값

자바스크립트에는 총 7 가지 거짓 값이 존재합니다.

`false`

`0`

`0n`

`''`

`null`

`undefined`

`NaN`

### 활용 1: 배열이 비어있나 확인하기

배열은 `length` 프로퍼티를 가지고 있습니다. 이를 검사해 배열이 비어있는지 확인 가능합니다.

### 불—편

```js
const arr = [1, 2, 3]

if (arr.length === 0) { // 좀 더 심플하게 안될까요?
  console.log('배열이 비어있음!')
}
```

### 편—안 ✅

```js
const arr = [1, 2, 3]

if (!arr.length) { // 👍 좋습니다! 0 은 Falsy 기 때문에 배열이 비어있다면 조건을 만족합니다.
  console.log('배열이 비어있음!')
}
```

### 활용 2: 안전히 메소드 호출하기

존재하지 않는 함수를 호출하면 `TypeError: ... is not a function` 를 `throw` 합니다.

이를 방지하고자 방어적인 코드를 작성해봅시다.

### 불—편

```js
const notArray = 5000

notArray.forEach(it => console.log(it)) // -> TypeError: notArray.forEach is not a function
```

### 편—안 ✅

```js
const iAmNotArray = 5000

iAmNotArray.forEach && iAmNotArray.forEach(it => console.log(it)) // 👍
```

## 2. 참 값

위에서 언급한 거짓 값을 제외하면 모두 참 값입니다.

### 활용1: 객체 요소 검사

`movie` 객체를 참조해 <아이언맨> 이 한글 자막을 지원하는지 확인해 봅시다.

### 불—편

```js
const movie = {
  ironman: {
    ko: '아이언맨 4',
    en: 'Iron Man 4'
  }
}

// 좀 더 심플하게 안 될까요?
if (movie.ironman.ko) {
  console.log('한글 자막 개봉')
}
```

### 편—안 ✅

```js
const movie = {
  ironman: {
    ko: '아이언맨 4',
    en: 'Iron Man 4'
  }
}

movie.ironman.ko && console.log('한글 자막 개봉') // 좋습니다. 👍
```
