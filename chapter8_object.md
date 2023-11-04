자바스크립트의 기본 데이터 구조

- 객체에는 undefined를 저장하지 않는 것이 좋다 (실제로 속성이 '없는' 것과 헷갈릴 수 있기 때문)

```jsx
let bar = "a long rod or rigid piece of wood or metal";
let my_little_object = {
	"0/0": 0,
	foo: bar,
	bar,
	my_little_method() {
		return "so small.";
	}
}
```

```jsx
my_little_object.foo === my_little_object.bar // true
my_little_object["0/0"] === 0                 // true
```

```jsx
my_little_object.rainbow // undefined
my_little_object[0]      // undefined
```

```jsx
my_little_object.age = 39;
my_little_object.foo = "slightly frilly or fancy";
```

```jsx
delete my_little_object["0/0"];
```

```jsx
typeof my_little_object === "object"
```

### 대소문자

키 값은 대소문자를 구별

```jsx
my_little_object.cat !== my_little_object.Cat
my_little_object.cat !== my_little_object.CAT
```

### 복사

```jsx
let my_copy = Object.assign({}, my_little_object);
my_copy.bar;         // "a long rod or rigid piece of wood or metal"
my_copy.age;         // 39
my_copy.age += 1;
my_copy.age;         // 40
delete my_copy.age;
my_copy.age;         // undefined
```

### 상속

- 자바스크립의 상속은 데이터만 연결된다
- 객체에 없는 속성을 참조하려 할 때, undefined를 반환하기 전에 해당 객체의 프로토타입을 확인하고, 프로토타입의 프로토타입을 확인한다

```jsx
const my_clone = Object.create(my_little_object);
my_clone.bar          // "a long rod or rigid piece of wood or metal"
my_clone.age          // 39
my_clone.age += 1;
my_clone.age          // 40
delete my_clone.age;
my_clone.age          // 39
```

- Object.assign(Object.create({}), *prototype*) 보다 Object.create(*prototype*)이 메모리를 덜 쓴다

```jsx
Object.create(null)
```

### 키

상속받은 속성을 제외한 나머지 속성의 이름을 배열로 반환된다 (삽입된 순서로 나열)

```jsx
Object.keys(object) // string[]
```

### 동결

- 객체를 전달받아서 변경할 수 없게 만든다
- 불변성은 시스템을 더 신뢰할 수 있게 만든다

⚠️ 깊은 동결은 아니기 때문에 최상위 객체만 동결 된다

```jsx
Object.freeze(my_copy);
const my_little_constant = my_little_object;

my_little_constant.foo = 7;  // allowd
my_little_constant = 7;      // SYNTAX ERROR!
my_copy.foo = 7;             // EXCEPTION!
my_copy = 7;                 // allowd
```

### 프로토타입과 동결을 같이 쓰지 마세요

- 프로토타입은 객체의 간단한 복사본을 만들 때 사용한다
- 동결된 객체는 속성이 불변이기 때문에 상속받은 객체의 속성을 자신만의 버전으로 가질 수 없다
- 또한 새로운 속성을 추가하는 것도 좀 느리게 동작한다 (프로토타입 체인을 뒤져서 해당 이름의 속성이 불변으로 지정되었는지 검사하기 때문)

### WeakMap

- 내용에 대한 검사를 허용하지 않아, 키가 없으면 값을 볼 수 없다
- 존재하는 키의 사본이 더 이상 없다면, 해당 키의 속성은 자동으로 삭제된다 (메모리 누출 방지)
- 비슷한 기능인 Map은 보안 기능이나 메모리 누출 방지 기능이 없다

| Object | WeakMap |
| --- | --- |
| key: 문자열 | key: 객체 |
| object = Object.create(null); | weakmap = new WeakMap(); |
| object[key] | weakmap.get(key) |
| object[key] = value; | weakmap.set(key, value); |
| delete object[key] | weakmap.delete(key); |

```jsx
const secret_key = new WeakMap();
secret_key.set(object, secret);

secret = secret_key.get(object);
```

```jsx
function sealer_factory() {
	const weakmap = new WeakMap();
	return {
		sealer(object) {
			const box = Object.freeze(Object.create(null));
			weakmap.set(box, object);
			return box;
		},
		unsealer(box) {
			return weakmap.get(box);
		}
	}
}
```