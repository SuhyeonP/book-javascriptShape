## 10 빈값
자바스크립트에서의 빈 값은 null과 undefined뿐

```jsx
function stringify_bottom(my_little_bottom) {
	if (my_little_bottom === undefined) {
		return "undefined";
	}
	if (my_little_bottom === null) {
		return "null";
	}
	if (Number.isNaN(my_little_bottom)) {
		return "NaN";
	}
}
```

```jsx
let foo;
foo // undefined

const obj = {};
obj.prop; // undefined

typeof null // 'object'
typeof undefined // 'undefined'

// 값이 없는 경우 할당하지 않는다.
// 잘못된 방법
{
	name: 'foo',
	address: null, // 권장되지않음.
	age: undefined // 권장되지않음.
}
// 올바른 방법
{
	name: 'foo',
}
```

```jsx
my_little_first_name = my_little_person.name.first;

// 해결
my_little_first_name = 
(my_little_person && my_little_person.name && my_little_person.name.first)

// 해결
my_little_first_name = my_little_person?.name?.first;

```

---

### 타입검사 typeof 처리 순서

1. undefined 검사
2. object 검사 (typeof null 이 여기에 해당됨)
3. number, string, boolean 검사