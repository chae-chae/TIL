## Tilde ~

피연산자의 각 자리의 비트를 뒤집는다. 비트연산자 `NOT`의 기능과 유사하다.

예:

```js
let a = 12;
console.log(a);    // 12
console.log(~a);   // -13
```

12의 2진수(32비트): 00000000000000000000000000001100,
~12                         : 11111111111111111111111111110011

### 2의 보수 연산

양수를 음수로, 음수를 양수로 변환하는 연산.

비트를 NOT 해준 뒤, 1을 더함. 음수라면 결과물에 -를 붙인다.

따라서 ~12는 -13이 된다.

식으로 표현하면 `-(n+1)`.

## Double tilde ~~

Tilde 연산자를 2번 반복하는 연산자.

예:

```js
let a = 12;
console.log(~a);   // -13
console.log(~~a);  // 12
```

이 때, 실수를 Double tilde하면 무슨 일이 생길까?

```js
let a = 12.25;   // 00000000000000000000000000001100.01 = 12.25
console.log(~a);   // 11111111111111111111111111110011 = -13
console.log(~~a);  // 00000000000000000000000000001100 = 12
```

소수점이 버려지는 것을 알 수 있다. 이는 `Math.floor`와 유사하다. 그렇다면 `Math.floor` 대신 Double tilde만 사용해도 될까?

## Math.floor와 Double tilde

이 둘에는 큰 차이점이 존재한다.

```js
let a = 12.25;
console.log(~~a);            // -12
console.log(Math.floor(a));  // -13
```

Double tilde는 소수점을 버리지만, `Math.floor`는 내림을 하기 때문에 음수를 연산할 때 결과에서 차이가 나게 된다. 그렇다면 double tilde는 항상 옳은 결과만을 산출할까? 그렇지 않다.

```js
// −2,147,483,648 ~ 2,147,483,647

let a = 10000000000.25;
console.log(Math.floor(a));  // 10000000000
console.log(~~a);            // 1410065408
```

Double tilde는 비트 연산이기 때문에, 2의 31승(2,147,483,648)을 넘는 값에 대해서 의도치 않은 결과를 얻을 수 있다.

단순히 소수점을 버리는 작업이라면 비트 연산자 `OR`을 사용하는 것이 예기치 못한 버그로부터 자유로울 것이다.

```js
console.log(12.4 | 0);        // 12
console.log(12.8 | 0);        // 12
console.log(-15.2 | 0);       // -15
console.log(-15.7 | 0);       // -15
```

## Double tilde의 다른 쓰임새

### undefined나 null을 0으로 변환환

Double tilde는 `undefined`나 `null`을 0으로 변환할 때 사용할 수 있다.

`undefined + 숫자`를 하면 `NaN`의 결과를 얻게 되는데, 이를 방지하는 용도다.

예:

```js
const arr = [1,1,1,2,2,3,3,3,3];
const obj = {};

arr.forEach(v => {
  if (obj[v]) obj1[v] += 1
  else obj[v] = 1
});
//obj {1: 3, 2: 2, 3: 4}
```

위 예시는 배열에서 1, 2, 3의 개수를 객체에 저장하는 예시이다. if절을 통해 obj에 key값이 있는 지 확인하는 코드가 들어있다. 이 코드가 없으면 NaN이 나올 수 있다.

Double tilde를 활용해 코드를 다시 작성해보자.

```js
const arr = [1,1,1,2,2,3,3,3,3];
const obj = {};

arr.forEach(v => obj[v] = ~~obj[v]+1)
```

### 메서드 반환값에 활용

`indexOf` 메서드에서 특정 문자를 찾지 못하면 -1을 반환한다. `-1에 tilde를 취하면 0`이 된다는 사실과 `0은 false`라는 사실을 이용해보자.

```js
let string = "abcd";

// 보편적인 사용법

if (string.indexOf('a') >= 0) console.log("존재함")
else console.log("존재하지 않음")

// tilde를 활용

if(~string.indexOf('a')) console.log("존재함")
else console.log("존재하지 않음")
```