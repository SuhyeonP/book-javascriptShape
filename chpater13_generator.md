## 13 제너레이터

### 제너레이터는 순수 함수와 비순수 함수 사이의 경계에 있다.

constant 팩토리로 만들어진 제너레이터는 **순수 함수** 이지만, 대부분의 제너레이터는 비순수함수이다.

제너레이터는 상태를 가지고 있을 수 있지만 팩토리 클로저에 숨겨져 있으며 상태는 제너레이터를 호출하는 경우에만 변경됩니다.

```jsx
function* count() {
	let count = 0;
	while(true) {
		count += 1;
		yiled count;
	}
}

const gen = counter();

gen.next().value; // 1
gen.next().value; // 2
gen.next().value; // 3

// count: factory / counter_generator: generator 
function count() {
	let count = 0;
	return function counter_generator() {
		count +=1;
		return count;
	}
}

const gen = count();
gen() //1
gen() //2
gen() //3
```

```jsx

function element(array, gen = integer(0, array.length)) {
	return function element_generator(...args) {
		const element_nr = gen(...args);
		if (element_nr !== undefined) {
			return array[element_nr];
		}
	}	
}

function property(object, gen = element(Object.keys(object))) {
	return function property_generator(...args) {
		const key=gen(...args);
		if (key !== undefined) {
			return [key, object[key]);
		}
	}
}

//////////////////////////////////////////////////////////////////////////////// 
function integer(from =0, to=Number.MAX_SAFE_INTEGER, step = 1) {
	return function () {
		if (from < to) {
			const result = from;
			from += step;
			return result;	
		}
	}
}

function collect(generator, array) {
	return function collect_generator(...args) {
		const value = generator(...args);
		if (value !== undefined) {
			array.push(value);
		}
		return value;
	}
}

function repeat(generator) {
	if (generator() !== undefined) {
		return repeat(generator)
	}
}

// example
const my_array = [];
repeat(collect(integer(0,7),my_array)) // [0, 1, ...6]

function harvest(generator) {
	const array = [];
	repeat(collect(generator, array));
	return array;
}

// example
harvest(integer(0,7)); // [0, 1, ...6]

////////////////////////////////////////////////////////////////////////////////
function limit(generator, count = 1) {
	return function (...args) {
		if (count >= 1) {
			count -= 1;
			return geneator(...args);
		}
	}
}

function filter(generator, predicate) {
	return function filter_generator(...args) {
		const value = generator(...args);
		if (value !== undefined && !predicate(value)) {
			return filter_generator(...args);	
		}
		return value;
	}
}

const my_third_array = harvest(filter(
	integer(0, 42),
	function divisible_by_three(value) {
		return (value % 3) === 0;
	}
});
// [0, 3, 6 .. 39]

// 두개 이상의 제너레이터 함수를 인자로 받아 인자들을 조합하여 순차적으로 실행하는 새로운 제너레이터를 만든다.
// ?? 첫번째 제너레이터에서 undefined가 나올 때까지 값을 받고 다음 제너레이터로 옮긴다.
function concat(...generators) {
  const next = element(generators);
  let generator = next();
  return function concat_generator(...args) {
	
    if (generator !== undefined) {
      const value = generator(...args);
      if (value === undefined) {
        generator = next();
        return concat_generator(...args);
      }
      return value;
    }
  }
}

// 새로운 제너레이터가 호출될 때마다 인자로 받은 제너레이터를 호출하고 그 값을 함수로 전달한다. 
function join(func, ...gens) {
  return function join_generator() {
      return func(...gens.map(function (gen) {
        return gen();
      })) 
  }
}

// 함수와 배열을 인자로 전달받아 각 요소에 대해 함수를 호출한 결과값을 담은 새로운 배열을 반환한다.
function map(array, func) {
	return harvest(join, func, element(array));
}

function objectify(...names) {
	return function objectify_constructor(...values) {
		const object = Object.create(null);
		names.forEach(function (name, name_nr){
			object[name] = values[name_nr];
		})
		return object;
	}
}

let date_marry_kill = objectify("date", "marry", "kill");
let my_little_object = date_marry_kill("butterfly", "unicorn", "monster");
// {date: "butterfly", marry: "unicorn", kill: "monster"}
```

---

```jsx
const array = [1,2,3];
console.dir(arr[Symbol.iterator]());
console.dir(arr[Symbol.iterator]().next());
```

---

### 제너레이터는 Iterable 인터페이스와 Iterator 인터페이스를 동시에 구현한다.

```jsx
function* generateSequence()

function *generateSequence()
```

위 두가지 문법 모두 사용가능하지만 첫번째 문법이 선호된다.

### function

Generator 함수 선언 키워드

### yeild

쉽게 value가 반환된다고 보면 된다.

next 메소드 호출 시 반환할 값에 사용하는 키워드

다음 next 호출까지 해당 키워드에서 함수 실행이 멈춘다.

### yield*

또 다른 iterable 객체를 위임해서 사용하기 위한 키워드

```jsx
function* generatorFunction() {
	yield 1;
	yield 2;
	yield 3;
}
const generator = generatorFunction();

for (const element of generator) {
	console.log(element);
}
// 1
// 2
// 3

function* generatorFunction() {
	yield* [1,2,3];
	/*	
	for (const element of generator) {
		console.log(element);
	}
	*/
} 

for (const element of generator) {
	console.log(element);
}
// 1
// 2
// 3
```

### Iterator 객체

### function

Generator 함수 선언 키워드

### yeild

쉽게 value가 반환된다고 보면 된다.

next 메소드 호출 시 반환할 값에 사용하는 키워드

다음 next 호출까지 해당 키워드에서 함수 실행이 멈춘다.

### yield*

또 다른 iterable 객체를 위임해서 사용하기 위한 키워드

```jsx
function* generatorFunction() {
	yield 1;
	yield 2;
	yield 3;
}
const generator = generatorFunction();

for (const element of generator) {
	console.log(element);
}
// 1
// 2
// 3

function* generatorFunction() {
	yield* [1,2,3];
	/*	
	for (const element of generator) {
		console.log(element);
	}
	*/
} 

for (const element of generator) {
	console.log(element);
}
// 1
// 2
// 3
```

### Iterator 객체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad6f0e8e-e46a-45c2-aefe-44cd9755de7a/71ba114b-24a8-4f97-9495-ae1ce660e9ed/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad6f0e8e-e46a-45c2-aefe-44cd9755de7a/951ed9c6-6aad-4ca5-8225-53cd94919d18/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad6f0e8e-e46a-45c2-aefe-44cd9755de7a/587400a0-4743-4052-8210-2b4f575227c8/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad6f0e8e-e46a-45c2-aefe-44cd9755de7a/1bd565e3-8616-4b15-888a-f1cc555eda05/Untitled.png)