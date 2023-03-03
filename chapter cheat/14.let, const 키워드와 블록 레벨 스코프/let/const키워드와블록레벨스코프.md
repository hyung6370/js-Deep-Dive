# **14.let, const 키워드와 블록 레벨 스코프**

## 🚀 **14.1 var 키워드로 선언한 변수의 문제점**
ES6까지는 `var` 키워드로만 변수를 선언할 수 있었다.
<br>`var` 키워드로 선언한 변수는 다음과 같은 특징을 갖는다.
<br><br>

### 📌 **원시 값과 객체의 비교** 
`var` 키워드로 선언한 변수는 중복 선언이 가능하며, 에러를 발생시키지 않는다.
<br>**초기화문이 있는 변수 선언문** 은 자바스크립트 엔진에 의해 `var` 키워드가 없는 것처럼 동작한다.
<br>**초기화문이 없는 변수 선언문** 은 무시된다.
- 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고, 변수를 중복 선언 및 할당하면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

```javascript
var x = 1;
var y = 1;
var x = 100;  // 에러 없이 재할당된다.
var y;  // 에러 없이 무시된다.
```

### 📌 **함수 레벨 스코프** 
`var` 키워드로 선언한 변수는 `함수 레벨 스코프`만을 인정한다.<br>
`if 문`, `for 문` 등을 포함한 코드 블록 내에서 선언해도 해당 지역 변수로 인정되지 않는다.
- `함수 레벨 스코프`는 전역 변수를 남발할 가능성을 높이며, **의도치 않게 전역 변수가 중복 선언될 수 있다.**

```javascript
var i = 10;

for (var i = 0; i < 5; i++) {
    console.log(i); // 0 1 2 3 4
}

console.log(i);  // 5
```

### 📌 **변수 호이스팅**
`var` 키워드로 변수를 선언하명 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.<br>
따라서 변수 선언문 이전에 참조할 수 있고, `undefined`를 반환한다.
- 이는 에러를 발생시키지는 않지만, 프로그램의 흐름상 맞지 않아 **가독성을 떨어뜨리고, 오류를 발생시킬 여지** 를 남긴다.
```javascript
console.log(foo);  // undefined
var foo;
```

<br>

___

## 🚀 **14.2 let 키워드**
ES6에서 새로운 변수 선언 키워드 `let`과 `const`가 도입되었다.<br>
`var` 키워드와 `let` 키워드의 주요 차이점은 다음과 같다.

### 📌 **변수 중복 선언 금지**
`let` 키워드는 이름과 같은 변수를 중복 선언하면 `문법 에러(SyntaxError)`가 발생한다.
```javascript
let foo = 123;
let foo = 456;  // SyntaxError
```

### 📌 **블록 레벨 스코프**
`let` 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 `블록 레벨 스코프(block-level-scope)`를 따른다.
```javascript
{
    let foo = 1;
}
console.log(foo);  // ReferenceError: foo is not defined
```

```javascript
let i = 10;  // 전역 스코프

function foo() {  // 함수 레벨 스코프
    let i = 100;

    for (let i = 1; i < 3; i++) {  // 블록 레벨 스코프
        console.log(i);  // out: 1 2
    }
    console.log(i);  // out: 100
}

foo();
console.log(i);  // out: 10
```

### 📌 **변수 호이스팅**
- `var` 키워드로 선언한 변수는 런타임 이전에 `선언 단계`와 `초기화 단계`가 한번에 진행되었다
- `let` 키워드로 선언한 변수는 런타임 이전에 `선언 단계`만 실행되고, 런타임에 변수 선언문에 도달했을 때, `초기화 단계`가 실행된다.
![](https://user-images.githubusercontent.com/66757141/214543399-bb1df4ad-789f-4b7f-924f-6bfd5c960fba.png)
- `초기화 단계`가 실행되기 이전에 변수에 접근하려고 하면, `참조 에러(ReferenceError)`가 발생한다.
- `let` 키워드로 선언한 변수를 참조할 수 없는 사이 구간을 `일시적 사각지대(Temporal Dead Zone, TDZ)` 라고 한다.

```javascript
console.log(foo);  // ReferenceError: foo is not defined
let foo;
```
- 변수 선언문(초기화) 이전에는 참조할 수 없어 호이스팅이 일어나지 않는 것처럼 보일 수 있다. 하지만 호이스팅은 명확히 발생한다.<br>
아래 예시에서 호이스팅이 발생하지 않는다, 전역변수 foo의 값을 출력해야하지만, 호이스팅으로 불록 스코프의 foo의 선언 단계까 완료되었기 때문에 `참조 에러(ReferenceErro)`가 발생했다.
```javascript
let foo = 1;
{
    console.log(foo);  // ReferenceError: Cannot access 'foo' before initialization
    let foo = 2;
}
```
### 📌 **전역 객체와 let**
`var` 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역객체 `window`의 프로퍼티가 된다.

```javascript
var x = 1;  // 전역 변수
y = 2;  // 암묵적 전역
function foo() {}  // 전역 함수

console.log(window.x);  // 1
console.log(window.y);  // 2
console.log(window.foo);  // foo() {}
```

`let` 키워드로 선언한 변수는 전역 객체의 프로퍼티가 아니다. `let` 전역변수는 보이지 않는 개념적인 블록 내에 존재한다.
```javascript
let x = 1;
console.log(window.x);  // undefined

// 객체에 존재하지 않는 프로퍼티에 접근하면, undefined를 반환하며, ReferenceError가 발생하지 않는다.
```
<br>

___

## 🚀 **14.3 const 키워드**
`const` 키워드의 특징은 `let` 키워드와 대부분 동일하지만, 다음과 같은 차이점을 갖는다

### 📌 **선언과 초기화**
`const` 키워드로 선언한 변수 역시 `블록 레벨 스코프`를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.<Br>
단, `const` 키워드로 선언한 변수는 **반드시 선언과 동시에 초기화 해야한다.**
```javascript
const foo = 1;
const bar;  // SyntaxError: Missing initializer in const declaration
```

### 📌 **재할당 금지**
`const` 키워드로 선언한 변수는 재할당이 금지된다.
```javascript
const foo = 1;
foo = 2;  // TypeError : Assignment to constant variable.
```

### 📌 **상수**
`const` 키워드는 상수를 표현하기에 적합한 특징을 갖고 있다.
- `const` 키워드로 선언된 변수에 원시 값을 할당한 경우, 원시 값은 변경할 수 없는 값(immutable value)이고, `const` 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.<br><br>
- 상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야한다. <br><br>
- 일반적으로 상수는 대문자로 선언하고, 여러 단어로 이뤄진 경우에는 언더스코어 `_`로 구분해서 스네이크 케이스로 표현한다.

```javascript
const TAX_RATE = 0.1;
let preTaxPrice = 100;
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);
```

### 📌 **const 키워드와 객체**
`const` 키워드로 선언된 변수에 **객체를 할당한 경우 값을 변경할 수 있다.**
- 객체는 변수 값의 재할당 없이도 직접 변경이 가능하기 때문이다.
- 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.
```javascript
const person = {
    name: 'Lee',
};

person.name = 'Kim';
console.log(person);  // {name: 'Kim'}
```
따라서 `const` 키워드는 재할당을 금지할 뿐, '불변'을 의미하지 않는다는 것에 유의!

<br>

___

## 🚀 **14.3 `var` vs `let` vs `const`**
`var` 와 `let`, `const` 키워드는 다음과 같이 사용하는 것을 권장한다.
- ES6를 사용한다면 `var` 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 `let` 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고, 읽기 전용으로 사용하는 상수 원시 값과 객체에는 `const` 키워드를 사용한다.

변수를 선언하는 시점에는 대부분 재할당 필요 여부를 알기 어려우므로 일단 `const` 키워드를 사용하자.

