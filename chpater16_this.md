**프로토타입**: 객체

**객체**: 속성만 저장

### 책의 예시

```jsx
const new_object = Object.create(old_object);

old_object_bud = function bud() {
	const that = this;

	function lou() {
			do_it_to(that);
	}
	lou();
};

// 성공
new_object.bud();

// 실패
const funcky = new_object.bud;
funcky();
```

```jsx
function pubsub() {
  const subscribers = [];
  return {
    subscribe: function (subscriber) {
      subscribers.push(subscriber)
    },
    publish: function (publication) {
      const length = subscribers.length;
      for (let i=0; i<length; i++) {
        subscribers[i](publication);
      }
    },
		getSubscribers: {
			console.log(subscribers)
		}
  }
}

// 문제가 있는 코드: 모든 구독자에 대한 데이터 삭제가 가능하다.
my_pubsub.subscribe(function (publication) {
  this.length = 0;
})

```

### 함수 객체가 가진 프로토타입 속성

- Function.prototype 델리게이션 링크 (객체에 연결된 prototype 을 Function.prototype 델리게이션 링크라고 부른다.)
- new 접두사를 통해 객체를 생성하는 경우, 생성된 객체의 프로토타입으로 사용된 객체에 대한 참조를 prototype의 속성으로 가진다.

  ex) new Array()

    ```jsx
    const a = new Array();
    a.splice() // splice사용 가능
    ```


### new 접두사가 하는 일들

- Object.create(function.prototype)에 대한 this 값을 만든다.
- 새로운 객체에 바인딩된 this값으로 함수를 호출한다.
- 함수가 객체를 반환하지 않으면 this를 강제로 반환한다.

### 🧐 결론은 this를 사용하지 말자.

---

### this 예시

```jsx
apple = '독이 든 사과';
var banana = '바나나';
var home = {
  apple: '맛있는 사과',
  // eatApple: eatAppleFn,
	// 내부 함수의 this는 전역 객체를 바인딩한다.
	eatApple: function() {
	  eatAppleFn();
	}
}

// this === window

function eatAppleFn() {
  console.log(`백설공주가 ${this.apple}를 먹습니다.`);
}

function eatBananaFn() {
	console.log(`백설공주가 ${this.banana}를 먹습니다.`);
}

// (1) 객체 method 호출
home.eatApple(); // 백설공주가 맛있는 사과를 먹습니다.

// (2) 함수 직접 호출
eatAppleFn(); // 백설공주가 독이 든 사과를 먹습니다.

eatBananaFn(); // 백설공주가 undefined를 먹습니다.
//////////////////////////////////////////

var apple = '독이 든 사과';
function Home() {
  this.apple = '맛있는 사과'
}

Home.prototype.eatApple = function () {
  console.log(`백설공주가 ${this.apple}를 먹습니다.`);
}

// 프로토타입의 this는 호출한 객체를 가리킨다.
const home = new Home();
home.eatApple(); // 백설공주가 맛있는 사과를 먹습니다.
```

### this binding

```jsx
var apple = '독이 든 사과';
var home = {
  apple: '맛있는 사과',
  eatApple: function() {
    eatAppleFn();       
  },
	eatAppleApply: function() {
    eatAppleFn.apply(this);
  },
  eatAppleCall: function() {
    // 여기서의 this는 home
    eatAppleFn.call(this); 
  },  
  eatAppleBind: function() {
    // 여기서의 this는 home
    (eatAppleFn.bind(this))(); 
  }
}

function eatAppleFn() {	
  console.log(`백설공주가 ${this.apple}를 먹습니다.`);    
}

home.eatApple();     // 백설공주가 undefined를 먹습니다.
home.eatAppleApply(); // 백설공주가 맛있는 사과를 먹습니다.
home.eatAppleCall(); // 백설공주가 맛있는 사과를 먹습니다.
home.eatAppleBind(); // 백설공주가 맛있는 사과를 먹습니다.
```

```jsx
function menu(item) {
	console.log(`this is ${item} ${this.name}`);
}

function testApply(item1, item2) {
	[item1, item2].forEach(function (el) {
		console.log(el, this.name)
	}, this)
}

const first = {
	name: 'testName'
}

menu.call(first, 'test-log'); // this is test-log testName
testApply.apply(first, ['element1', 'element2'])

const testForBind = menu.bind(first);

testForBind('bind!!!'); // this is bind!!! testName
```