# 객체 리터럴

객체란 ?

자바는 객체(object)기반의 언어이며, 자바스크립트를 구성하는 모든것이 객체라고 한다.
타입에는 변경이 불가능한 immutable value(원시 타입)가 있고 변경이 가능한 mutable value가 있는데 객체는 mutable value이다.

#### 객체는 0개 이상의 프로퍼티로 구성되어 있고, 프로퍼티는 키와 값이다. 프로퍼티 값은 모든게 될 수있는데 만약 함수가 프로퍼티 값이 된다면 일반 함수와 구분하기 위해 메서드라고 부른다.

- 프로퍼티 : 객체의 상태를 나타내는 값 ( 키 : 값 )
- 메서드: 프로퍼티를 참조하고 조작할 수 있는 동작 (함수)

### 객체 리터럴

객체 리터럴은 객체를 생성하기 위한 표기법이다. 그리고 값으로 표현되는 표현식

```tsx
const person = {
  name: "Choi",
  sayHello: function () {
    console.log("hello");
  },
};
```

객체 리터럴은 중괄호내에 0개이상의 프로퍼티를 정의한다.

### 프로퍼티

객체는 프로퍼티의 집합이고, 키와 값으로 구성된다. <br/>
프로퍼티 키는 프로퍼티 값에 접근할 수 있는 식별자의 역할을 한다.<br/>
식별자 네이밍규칙을 준수하면 큰따옴표로 감쌀 필요가 없지만 식별자 네이밍규칙을 준수하지 않는다면 큰따옴표로 감싸야 한다.

### 메서드

메서드는 객체에 묶여 있는 함수를 의미한다.

```tsx
const circle = {
  radius: 5,
  getDiameter: function () {
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

### 프로퍼티 접근

프로퍼티에 접근하는 방법은 두가지가 있다.

- 마침표 표기법
- 대괄호 표기법

```tsx
const person = {
  name: "EUNJI",
};

console.log(person.name); // 마침표 표기법
console.log(person["name"]); // 대괄호 표기법
```

대괄호 프로퍼티 접근연산자 내부에 지정하는 프로퍼티 키는 반드시 '문자열'이여야 한다. 따옴표로 감싸지 않으면 자바스크립트 엔진에서 식별자로 해석하여 reference error를 띄우게 된다.

### 프로퍼티 생성 및 삭제

```tsx
const person = {
  name: "Eunji",
};

person.age = 20; // 프로퍼티 생성

delete person.age; // 프로퍼티 삭제
```

### ES6에 추가된 객체 리터럴의 확장 기능

#### 프로퍼티 축약 표현

변수 이름과 프로퍼티 키가 동일한 이름일때 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```tsx
let x = 1,
  y = 2;

const obj = { x, y };
console.log(obj); // { x: 1, y: 2 }
```

#### 메서드 축약 표현

ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약표현을 사용 할 수있다.

```tsx
const obj = {
  name: "EUNJI",
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};

obj.sayHi(); // Hi EUNJI
```
