# 7장 배열

### **배열의 length**: 배열이 담고있는 요소의 개수를 의미하지는 않는다.

ex) [1, undefined, undefined, undefined, 5]인 배열의 경우

length: 5, 요소의 개수는 2

### isArray

typeof 연산자는 `object`를 반환하므로 배열을 확인할 때는 `isArray`를 사용해야한다.

```jsx
const what_is_it = new Array(1000);
typeof what_is_it; // object
Array.isArray(what_is_it); // true
```

### 배열의 원점

배열은 `0`번째부터 시작한다.

### 초기화 및 배열 생성

```jsx
// 배열 리터럴
let same_thing = [0,0,0,0,0,0,0,0,0,0];

// new Array(정수)
let my_little_array = new Array(10).fill(0);

// from
Array.from({length: 3}, (e, i) => i)

// 객체와 마찬가지로 실제 같은 배열인 경우에만 동일 배열로 본다.
same_thing === my_little_array; // false
```

### 스택과 큐

배열을 스택처럼 사용할 수 있는 메소드를 갖고 있다.

- pop: 마지막 요소를 배열에서 제거하고 반환
- push: 배열의 끝에 요소를 추가
- shift: 배열의 0번째 요소를 제거하고 반환 (pop과 유사함)
- unshift: 배열의 가장 앞(0번째)에 새로운 요소를 추가 (push와 유사함)

### 검색

- indexOf
    - 앞부터 검사
    - return type number
- lastIndexOf
    - 뒤부터 검사
    - return type number
- includes:
    - return type boolean

```jsx
const array = [1,2,3];

array.indexOf(3); // 2
array.indexOf(6);// -1

array.lastIndexOf(3); // 2
array.lastIndexOf(6);// -1

array.includes(3); // true
array.includes(6);// false

```

### 축약

- reduce
    - 배열의 앞부터 시작한다.
- **reduceRight**
    - 배열의 끝에서 시작한다.

```jsx
const array1 = [
  [0, 1],
  [2, 3],
  [4, 5],
];

const result1 = array1.reduce((accumulator, currentValue) => accumulator.concat(currentValue));// [0, 1, 2, 3, 4, 5]
const result2 = array1.reduceRight((accumulator, currentValue) => accumulator.concat(currentValue));// [4, 5, 2, 3, 0, 1]
```

### 반복

- every
- some
- find
- findIndex
- filter
- map

```jsx
const isBelowThreshold = (currentValue) => currentValue < 40;
const array1 = [1, 30, 39, 29, 10, 13];
console.log(array1.every(isBelowThreshold));// true

const array = [1, 2, 3, 4, 5];
const even = (element) => element % 2 === 0;
console.log(array.some(even)); // true

const array1 = [5, 12, 8, 130, 44];
const found = array1.find((element) => element > 10); // 12

const array1 = [5, 12, 8, 130, 44];
const found = array1.find((element) => element > 10); // 1

const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];
const result = words.filter((word) => word.length > 6);
console.log(result); // ["exuberant", "destruction", "present"]

const array1 = [1, 4, 9, 16];
const map1 = array1.map((x) => x * 2); // [2, 8, 18, 32]
```

**배열의 끝에 도달하기 전에 종료할 수 있는 메소드**

`forEach`(every, some이 더 빨리 종료시킬 수 있다.), `find`

**배열의 끝에 도달하기 전에 종료불가한 메소드**

`map`, `reduce`, `filter`

### 정렬

sort는 추가 메모리를 사용하지 않고 배열 자체를 수정한다.

### 그 외 메소드

- concat
- join
- reverse
- slice

```jsx
const array1 = [1,2];
const array2 = [3,4];

array1.concat(array2);// [1,2,3,4]
array1.join(); // 1,2
array1.reverse(); // [2,1];
const result = array1.slice(1); // [2]
```

### 순수함수

* `concat`

* `every`

* `filter`

* `find`

* `findIndex`

* `forEach`

* `indexOf`

* `join`

* `lastIndexOf`

* `map`

* `reduce`

* `reduceRight`

* `slide`

* `some`

### 비순수 함수 (배열자체를 바꾸는 함수)

* `fill`

* `pop`

* `push`

* `shift`

* `splice`

* `unshift`

* `reverse`

* `sort`