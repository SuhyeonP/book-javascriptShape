<aside>
💡 컴퓨터는 숫자를 다루는 기계이고, 우리는 여러종류이 정보를 숫자와 매핑함.

</aside>

자바스크립트의 수는 실수에서 영감받았지만, **진짜 실수는 아니다.**



자바스크립트는 **`number`** 라고 하는 하나의 숫자형을 가지고있다. 자바스크립트는 숫자형이 하나뿐이란느 이유로 자주 비판받지만, 이것이 큰 장점이다. 비슷한 여러가지 타입중에서 혹시 잘못된 타입을 사용할까봐 고민하며 시간을 낭비하지 않아도 되니 개발자의 생산성이 증가하고, 타입변환으로 오류도 없다. int형을 사용해 발생하는 [오버플로](https://honey-hyoni-sub.notion.site/3cd673c3a1ab4ebb825103c2bd771355?v=7c8587fe2d1f47e18e1c460af5ce788e&pvs=4) 문제도 발생하지 않는다. 오버플로가 발생하지 않기 때문에 자바스크립트의 정수는 자바의 정수보다 훨씬 안정적이다.

자바스크립트의 number는 자바의 double과 밀접하다.

### 0

자바스크립트에서 0으로 표시되지만, 0이 아닌값이 있다. 제대로된 시스템이라면 오직 하나의 0 만 있겠지만,
0 과 -0 두개의 0이 있다. 자바스크립트는 이 이상한 현상을 숨기기 위해 노력햇고 거의 성공햇지만

- `(1 / 0) === (1 / -0)` // false
- `Object.is(0,-0)`  // false

이와 같은 경우를 제외하면 -0이 존재하는 사실은 몰라도 된다.

### 숫자리터럴

Infinity는 표현하기에는 너무 큰 모든 숫자를 나타낸다. Infinity와 ∞ 를 헷갈리면 안된다. 수학에서 ∞ 는 값이아니라 은유니까.

NaN 은 숫자가 아닌 숫자를 나타내는 특별한 값인데, NaN 은 Not a number 를 뜻하지만,
typeof 연산자는 NaN 을 number로 표시하기에 아주 헷갈리긴한다.

NaN은 문자열을 숫자로 변환하려고 했으나, 실패했을때 결과값으로 반환될 수 있다. 변환에 실패한 경우 오류가 발생하거나 프로그램이 멈추는 대신 NaN이 반환된다. 산술연산자 역시 입력중 NaN 이 있다면 그 결과로 NaN을 생성한다. 하지만 NaN 과 NaN을 동등연산자로 비교하면 다르다 (`NaN !== NaN`)
테스트 기댓값이 NaN 인지 확인하려면 `Number.isNaN(value)` 를 사용하라!
추가로!!!  `Number.isFinite(value)` 를 사용하면 값이 `NaN`, `Infinity`, `-Infinitiy`인 경우 `false`를 반환한다.

### Number

이는 number와 다르다. 대문자 N!

이는 숫자를 만드는 함수이다. 자바스크립트의 수는 불변의 객체이다. 수에대한 typeof 연산자는 “number” 를 반환한다. Number에는 new 를 사용하면 안된다.

```jsx
const goodExample = Number("432"); // 432
const badExample = new Number("432"); // Number{432}

typeof goodExample; // "number"
typeof badExample; // "object"
goodExample === badExample // false
```

`NUMBER.EPSILON` 은 2.220446049… 인데, 1에 더했을때 1보다 큰수를 만들어낼 수 있는 가장 작은 양수이고, `NUMBER.EPSILON`보다 작은수를 1에 더해도 그수는 1과 같다.

`NUMBER.MAX_SAFE_INTEGER`는 정확히 9007199254740991 , 약 9천조이다. 자바스크립트의 숫자형은

NUMBER.MAX_SAFE_INTEGER 까지의 모든 정수형을 표현할 수 있으므로 다른 정수형 타입이 필요없다. 자바스크립트의 숫자형은 부호를 포함한 54비트를 사용한다.

NUMBER.MAX_SAFE_INTEGER 보다 큰수에 1을 더하는것은 0을 더하는것이나 마찬가지이다.

자바스크립트는 값이나 연산 결과, 그리고 중간 연산 값들이 전부 -NUMBER.MAX_SAFE_INTEGER 와 NUMBER.MAX_SAFE_INTEGER 사이의 정수값인 경우에만 올바른 정수연산을 할 수 있다.

Number.isSafeInteger(number)는 숫자가 안전한 범위내에 있을경우 true를 반환한다.

Number.MAX_VALUE 에 안전한 범위안에 있는 어떤 양의 정수를 더해도 그값은 여전히 Number.MAX_VALUE이다.

Number.MIN_VALUE 는 0보다 큰 수 중에서 가장 작은수이다. Number.MIN_VALUE 보다 작은 양수는 0과 구별이 불가능하다. Number.MIN_VALUE 의 유효숫자는 최하위 비트 단 한개만 포함하고 있으며 이 비트로 인해 수없이 많은 환상에 불과한 유효숫자가 만들어진다.

### Math 객체

> Math 객체는 Number에 내장 되어있어야할 중요한 여러함수를 포함하고 있다.
>

Math.floor와 Math.trunc 는 수에서 정수를 만들어내는데

- Math.floor() 는 더 작은 정수
- Math.trunc()는 0에 가까운 정수를 만들어낸다

```jsx
Math.floor(-2.5); // -3
Math.trunc(-2.5); // -2
```

Math.min 과 Math.max 는 인자중 가장 작은값과 큰값을 반환한다.

Math.random 은 0과 1 사이 임의의 수를 반환하는데, 게임에는 적합하지만 암호학적 프로그램이나, 카지노게임에는 적합하지 않다(보안 이슈)

자바스크립트의 10진 소수값은 능력이 좋지않다.

0.1또는 그 외 10진소수값을 프로그램에 입력하면, 자바스크립트는 그 값을 제대로 처리할 수 없다. 그래서 그 대신 값을 정확히 표현할 수 있는 별칭(alias)를 사용.