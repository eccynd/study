# 클래스

**내용**

- [여기로이동](#여기로이동)

---

## 클래스와 기본 문법

클래스는 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀로, 객체를 정의하기 위한 상태(멤버 변수)와 메서드(함수)로 구성된다.

### 기본 문법

클래스는 다음과 같은 기본 문법을 사용해 만들 수 있습니다.

```javascript
class MyClass {
  // 여러 메서드를 정의할 수 있다.
  constructor() {...}
  method() {...}
  method() {...}
  method() {...}
  ...
}
```

이렇게 클래스를 만들고, new MyClass()를 호출하면 내부에서 정의한 메서드가 들어 있는 객체가 생성된다. `객체의 기본 상태를 설정해주는 생성자 메서드 constructor()는 new에 의해 자동으로 호출되므로, 특별한 절차 없이 객체를 초기화 할 수 있다.`

```javascript
class User {

  constructor(name) {
    this.name =name;
  }

  sayHi() {
    alert(this.name);
  }

// 사용법
let user = new User("John");
user.sayHi();
}
```

new User("John")을 호출하면 다음과 같은 일이 일어난다.

1. 새로운 객체 생성
2. 넘거받은 인수와 함께 constructor가 자동으로 실행된다. 이때 인수 "John"이 this.name에 할당된다. 이런 과정을 거친후 user.sayHi()같은 객체 메서드를 호출할 수 있다.

### 클래스가 정확히 뭔가?

자바스크립트에서 클래스는 함수의 한종류이다.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    alert(this.name);
  }
}

// User가 함수라는 증거
alert(typeof User); // function
```

class user {...} 문법 구조가 진짜 하는 일은 다음과 같다.

1. User라는 이름을 가진 함수를 만든다. 함수 본문은 생성자 메서드 constructor에서 가져온다. 생성자 메서드가 없다면 본문이 비워진 채로 함수가 만들어진다.
2. sayHi 같은 클래스 내에서 정의한 메서드를 User.prototype에 저장한다.

new User를 통해 호출해 객체를 만들고, 메서드를 호출하면 메서드를 프로토타입에서 가져온다. 이 과정이 있기에 객체에서 클래스 메서드에 접근할 수 있다.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    alert(this.name);
  }
}

// 클래스는 함수입니다.
alert(typeof User); // function

// 정확히는 생성자 메서드와 동일합니다.
alert(User === User.prototype.constructor); // true

// 클래스 내부에서 정의한 메서드는 User.prototype에 저장됩니다.
alert(User.prototype.sayHi); // alert(this.name);

// 현재 프로토타입에는 메서드가 두 개입니다.
alert(Object.getOwnPropertyNames(User.prototype)); // constructor, sayHi
```

### 클래스 표현식

함수처럼 클래스도 다른 표현식 내부에서 정의, 전달, 반환, 할당할 수 있다.

```javascript
let User = class {
  sayHi() {
    alert('Hello');
  }
};
```

기명 함수 표현식(Named Function Expression)과 유사하게 클래스 표현식에도 이름을 붙일수 있다. `클래스 표현식에 이름을 붙이면, 이 이름은 클래스 내부에서만 사용할 수 있다.`

```javascript
// 기명 클래스 표현식(Named Class Expression)
// (명세서엔 없는 용어이지만, 기명 함수 표현식과 유사하게 동작합니다.)
let User = class MyClass {
  sayHi() {
    alert(MyClass); // MyClass라는 이름은 오직 클래스 안에서만 사용할 수 있습니다.
  }
};

new User().sayHi(); // 제대로 동작합니다(MyClass의 정의를 보여줌).

alert(MyClass); // ReferenceError: MyClass is not defined, MyClass는 클래스 밖에서 사용할 수 없습니다.
```

아래와 같이 '필요에 따라' 클래스를 동적으로 생성하는 것도 가능하다.

```javascript
function makeClass(phrase) {
  // 클래스를 선언하고 이를 반환함
  return class {
    sayHi() {
      alert(phrase);
    }
  };
}

// 새로운 클래스를 만듦
let User = makeClass('Hello');

new User().sayHi(); // Hello
```

### getter와 setter

리터럴을 사용해 만든 객체처럼 클래스도 getter나 setter, 계산된 프로퍼티(cmoputeed property)를 포함할 수 있다. `get`과 `set`을 이용해 user.name을 조작해보자.

```javascript
class User {
  constructor(name) {
    // setter를 활성화합니다.
    this.name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      alert('이름이 너무 짧습니다.');
      return;
    }
    this._name = value;
  }
}

let user = new User('John');
alert(user.name); // John

user = new User(''); // 이름이 너무 짧습니다.
```

### 계산된 메서드 이름 [...]

대괄호 [...]를 이용해 계산된 메서드 이름`(computed method name)`을 만드는 예시를 살펴보자.

```javascript
class User {
  ['say' + 'Hi']() {
    alert('Hello');
  }
}

new User().sayHi();
```

### 클래스 필드

클래스 필드 문법을 사용하면 어떤 종류의 프로퍼티도 클래스에 추가할 수 있다.

```javascript
class User {
  name = 'John';

  sayHi() {
    alert(`Hello, ${this.name}!`);
  }
}

new User().sayHi(); // Hello, John!
```

클래스를 정희할 때 <프로퍼티 이름> = <값>을 써주면 간단히 클래스 필드를 만들 수 있다.
클래스 필드의 중요한 특징 하나는 User.prototype이 아닌 개별 객체에만 클래스 필드가 설정된다.

```javascript
class User {
  name = 'John';
}

let user = new User();
alert(user.name); // John
alert(User.prototype.name); // undefined
```

아울러 클래스 필드엔 복잡한 표현식이나 함수 호출 결과를 사용할 수 있다.

```javascript
class User {
  name = prompt('이름을 알려주세요.', '보라');
}

let user = new User();
alert(user.name); // 보라
```

### 클래스 필드로 바인딩 된 메서드 만들기

자바스크립트의 함수는 동적인 this를 갖습니다. 따라서 객체 메서드를 여기저기 전달해 전혀 다른 컨텍스트에서 호출하게 되면 this는 원래 객체를 참조하지 않습니다

```javascript
class Button {
  constructor(value) {
    this.value = value;
  }

  click() {
    alert(this.value);
  }
}

let button = new Button('hello');

setTimeout(button.click, 1000); // undefined
```

위 문제를 해결 하기위해 두 가지의 방법이 있다.

1. setTimeout(() => button.click(), 1000) 같이 `래퍼 함수를 전달`하기
2. 생성자 안 등에서 메서드를 객체에 `바인딩`하기
   > 바인딩 참조 -> https://ko.javascript.info/bind

그러나 클래스 필드는 또 다른 훌륭한 방법을 제공한다.

```javascript
class Button {
  constructor(value) {
    this.value = value;
  }

  click = () => {
    alert(this.value);
  };
}

let button = new Button('hello');

setTimeout(button.click, 1000); // hello
```

클래스 필드 click = () => {...}는 각 Button 객체마다 독립적인 함수를 만들고 함수의 this를 해당 객체에 바인딩시켜준다. 따라서 개발자는 button.click을 아무 곳에나 전달할 수 있고, this엔 항상 의도한 값이 들어가게 된다.

클래스 필드의 이런 기능은 브라우저 환경에서 메서드를 이벤트 리스너로 설정해야 할 때 특히 유용하다.

---

## 요약

```javascript
class MyClass {
  prop = value; // 프로퍼티

  constructor(...) { // 생성자 메서드
    // ...
  }

  method(...) {} // 메서드

  get something(...) {} // getter 메서드
  set something(...) {} // setter 메서드

  [Symbol.iterator]() {} // 계산된 이름(computed name)을 사용해 만드는 메서드 (심볼)
  // ...
}
```

MyClass는 constructor의 코드를 본문으로 갖는 함수이다. MyClass에서 정의한 일반 메서드나 getter, setter는 MyClass.prototype에 쓰인다.
