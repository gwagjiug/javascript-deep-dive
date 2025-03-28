## 09장

---

### 9.1 타입 변환이란?

---

- 자바스크립트의 모든 값은 타입이 있다.
- 값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있다.
- 개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅** 이라고 한다.

```jsx
//명시적 타입 변환

var x = 10;
var str = x.toString();
console.log(typeof str, str); //string 10
console.log(typeof x, x); //number 10
```

- 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다.
- 이를 암묵적 타입 변환 또는 타입 강제 변환이라 한다.

```jsx
// 암묵적 타입 변환의 예

var x = 10;

//문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성해버림.
var str = x + "";
console.log(typeof str, str); //string 10

//변수 x 의 값이 변경된 것은 아님
console.log(typeof x, x); //number 10
```

- 명시적 타입 변환이나, 암묵적 타입 변환이 기존 원시 값을 직접 변경하는 것은 아니다.
- 원시 값은 변경 불가능한 값이므로 변경할 수 없다.
- 타입 변환이란 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.

### 9.2 암묵적 타입 변환

---

JS 엔진은 코드의 문맥을 고려해서 암묵적으로 데이터 타입을 강제 변환할 때가 있다.

```jsx
// 피연산자가 모두 문자열 타입이어야 하는 문맥
"10" + 2; -> 102

//피연산자가 모두 숫자 타입이어야 하는 문맥
5 * "10" -> 50

//피연산자 또는 표현식이 boolean 타입이어야 하는 문맥
!0; // -> true
if(1){

}
```

- 표현식을 평가할 때 코드의 문맥에 부합하지 않는 다양한 상황이 발생할 수 있다. 자바스크립트는 이러한 상황에서 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가하려고 한다.

#### 9.2.1 문자열 타입으로 변환

```jsx
1 + "2"; // 12
```

위 예제에서 + 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.

문자열 연결 연산자의 역할을 문자열 값을 만드는 것이다.

```jsx
// 숫자 타입
0 + ''         // -> "0"
-0 + ''        // -> "0"
1 + ''         // -> "1"
-1 + ''        // -> "-1"
NaN + ''       // -> "NaN"
Infinity + ''  // -> "Infinity"
-Infinity + '' // -> "-Infinity"

// 불리언 타입
true + ''  // -> "true"
false + '' // -> "false"

// null 타입
null + '' // -> "null"

// undefined 타입
undefined + '' // -> "undefined"

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // -> "[object Object]"
Math + ''           // -> "[object Math]"
[] + ''             // -> ""
[10, 20] + ''       // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + ''          // -> "function Array() { [native code] }"
```

### 9.2.2 숫자 타입으로 변환

```jsx
1 - "1"; // 0
1 * "10"; // 10
1 / "one"; // NaN
```

- 산술 연산자는 숫자를 만드는 역할을 하기 때문에 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.

- 자바스크립트 엔진은 산술 연산자의 피연산자 중에서 숫자타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환을 한다.
- 이때 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가가 `NaN` 이 되어 버린다.

```jsx
"1" > 0; //true
```

비교 연산자는 boolean 값을 만드는 역할을 한다. 비교연산자는 피연산자의 크기를 비교하므로 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.

- 즉, 자바스크립트 엔진은 비교 연산자 표현식을 평가하기 위해 비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 `암묵적 타입 변환` 한다.

```jsx
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 0
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  Symbol() + // -> ypeError: Cannot convert a Symbol value to a number
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

- 여기서 빈문자열, 빈 배열, null, false 는 0으로 true 는 1로 변환된다. 객체와 빈 배열이 아닌 배열, undefined 는 변환되지 않기에 `NaN` 이 된다.

### 9.2.3 boolean 타입으로 변환

```jsx
if ("") console.log(x);
```

- if 문이나 for문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 boolean 값으로 평가되어야 하는 표현식이다.
- 자바스크립트 엔진은 조건식의 평가 결과를 boolean 타입으로 암묵적 타입 변환한다.

```jsx
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

// 2 4
```

- 자바스크립트 엔진은 boolean 타입이 아닌 값을 Truthy 값 또는 Falsy 값 으로 구분한다.
- Falsy 로 평가되는 값은 다음과 같다.

- false, undefined , null , 0, -0 , NaN , ''(빈 문자열)
- Falsy 값 외의 모든 값은 true 로 평가되는 Truthy 값이다.

## 9.3 명시적 타입 변환

---

- 개발자의 의도에 따라 명시적으로 타입을 변경하는 방법을 말한다. 생성자 함수를 `new` 연산자 없이 호출하는 방법, 빌트인 메서드를 사용하는 방법, 암묵적 타입 변환을 사용하는 방법 등이 있다.

### 9.3.1 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법을 말한다.

1. String 생성자 함수를 `new` 연산자 없이 호출하는 방법
2. Object.prototype.toString() 메서드를 사용하는 방법
3. 문자열 연결 연산자를 이용하는 방법

```jsx
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // -> "true"
String(false); // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString(); // -> "1"
NaN.toString(); // -> "NaN"
Infinity.toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
true.toString(); // -> "true"
false.toString(); // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + ""; // -> "1"
NaN + ""; // -> "NaN"
Infinity + ""; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + ""; // -> "true"
false + ""; // -> "false"
```

### 9.3.2 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

1. Number 생성자 함수를 `new` 연산자 없이 호출하는 방법
2. parseInt, parseFloat 메서드를 이용하는 방법(문자열만 숫자 타입으로 변환 가능합니다.)
3. `+` 단항 산술 연산자를 이용하는 방법
4. `*` 산술 연산자를 이용하는 방법

```jsx
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true); // -> 1
Number(false); // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt("0"); // -> 0
parseInt("-1"); // -> -1
parseFloat("10.53"); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+"0"; // -> 0
+"-1"; // -> -1
+"10.53"; // -> 10.53
// 불리언 타입 => 숫자 타입
+true; // -> 1
+false; // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
"0" * 1; // -> 0
"-1" * 1; // -> -1
"10.53" * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1; // -> 1
false * 1; // -> 0
```

### 9.3.3 boolean 타입으로 변환

boolean 타입이 아닌 값을 boolean 타입으로 변환하는 방법은 다음과 같다.

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. !부정 논리 연산자를 두 번 사용하는 방법

```jsx
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean("x"); // -> true
Boolean(""); // -> false
Boolean("false"); // -> true
// 숫자 타입 => 불리언 타입
Boolean(0); // -> false
Boolean(1); // -> true
Boolean(NaN); // -> false
Boolean(Infinity); // -> true
// null 타입 => 불리언 타입
Boolean(null); // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({}); // -> true
Boolean([]); // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!"x"; // -> true
!!""; // -> false
!!"false"; // -> true
// 숫자 타입 => 불리언 타입
!!0; // -> false
!!1; // -> true
!!NaN; // -> false
!!Infinity; // -> true
// null 타입 => 불리언 타입
!!null; // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{}; // -> true
!![]; // -> true
```

### 9.4 단축평가

---

단축평가는 아래와 같은 상황에서 유용하게 사용된다.

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 객체는 키와 값으로 구성된 프로퍼티의 집합이다. 만약 객체를 가리키기를 기대하는 변수의 값이 객체가 아니라 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입에러가 발생하고 프로그램은 강제 종료된다.

```jsx
var elem = null;
var value = elem.value; //TypeError : Cannot read property 'value' of null
```

이때 단축평가를 이용해서 에러를 발생시키지 않을 수 있는데,

```jsx
var elem = null;
//elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
//elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```

- 함수 매개변수에 기본 값을 설정할 때, 함수를 호출하면서 인수를 전달하지 않으면 매개변수에는 자동으로 `undefined` 가 할당된다. 이때 단축 평가를 사용해서 매개변수의 기본값을 설정해서 `undefined` 로 인해 발생할 수 있는 에러를 방지할 수 있다.

```jsx
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = "") {
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2
```

### 9.4.1 논리 연산자를 사용한 단축평가

```jsx
"Cat" && "Dog"; // "Dog"
```

1. 논리곱 연산자는 두 개의 피연산자가 모두 true 로 평가될 때 true 를 반환한다.
2. 논리곱 연산자는 좌항에서 우항으로 평가가 진행된다.

평가가 진행되는 과정은 다음과 같다.

1. Cat 과 Dog 는 false를 반환하는 값이 아니기 때문에 Truthy한 값이다.
2. 첫 번쨰 피연산자 Cat 은 true 로 평가된다. 하지만 이 시점에서는 아직 전체 표현식이 true 라고는 할 수 없다.
3. 따라서 두 번째 피연산자가 평가 결과를 결정한다.
4. 이때 논리곱 연산자는 논리연산의 결과를 결정하는 두 번째 피연산자 Dog를 그대로 반환한다.

```jsx
"Cat" || "Dog";
```

논리합 연산자는 두 개의 피연산자 중 하나만 true 로 평가되어도 true를 반환한다. 논리합 연산자도 좌항에서 우항으로 평가가 진행된다.

1. Cat과 Dog는 false를 반환하는 값이 아니기 때문에 Trusty한 값이다.
2. 첫 번째 피연산자 Cat은 true로 평가된다. 이 시점에서 두 번째 피연산자까지 평가하지 않더라도 위 표현식을 true로 평가 가능하다.
3. 이때 논리곱 연산자는 논리연산의 결과를 결정하는 첫 번째 피연산자 Cat을 그대로 반환한다.

논리곱 연산자와(&&) 논리합(||) 연산자는 이 처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다. 이를 단축평가라고 한다. 단축평가는 표현식을 평ㄷ가하는 도중에 평가 결과가 확정된 경우ㅡ 나머지 평가 과정을 생략하는 것을 말한다.

- 단축 평가를 사용하면 if 문을 대체할 수 있다. 어떤 조건이 Truthy 값일 때 무언가를 해야 한다면 논리곱(&&) 연산자 표현식으로 if 문을 대체할 수 있다.

```jsx
var done = true;
var message = "";

//주어진 조건이 true 일 때
if (done) message = "완료";

message = done && "완료";
console.log(message); //완료
```

- 조건이 Falsy 값일 때 무언가를 해야 한다면 논리합(||) 연산자 표현식으로 if문을 대체할 수 있다.

```jsx
var done = false;
var message = "";

//주어진 조건이 false일 때
if (!done) message = "미완료";

//if문은 단축평가로 대체 가능하다.
//done이 false라면 message에 '미완료'를 할당.
message = done || "미완료";
console.log(message);
```

### 9.4.2 옵셔널 체이닝 연산자

ES11에서 도입된 옵셔널 체이닝 연산자 `?.` 은 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.

```jsx
var elem = null;
// elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```

### 9.4.3 null 병합 연산자

null 병합 연산자 `??` 은 좌항의 피연산자가 null 또는 undefined 인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다. null 병합 연산자 `??` 는 변수에 기본 값을 설정할 때 유용하다.

```jsx
const foo = null ?? "default string";
console.log(foo);
// Expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// Expected output: 0
```
