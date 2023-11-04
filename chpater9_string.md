## 9 문자열

문자열은 사람과 컴퓨터간에 존재하는 틈을 이어주는 다리같은 역할이다.

### String에서의 기초 메서드

문자열은 0에서 65535 사이의 크기를 가지는 부호가 없는 16비트 정수로 이루어진 불변배열이다.

`String.fromCharCode` 함수로 만들 수 있는데, `charCodeat`으로 접근가능하다.(아스키코드로 주로 사용햇음)

```jsx
String.fromCharCode(122) // z
'z'.charCodeAt() // 122

const example_array = [99,111,114,110]
const example_array_string = String.fromCharCode(...example_array) // corn

example_array_string.charCodeAt(0) === 99 // true
example_array_string.length // 4
typeof example_array_string // "string"

example_array_string[0] === example_array[0] // false
example_array_string[0] === String.fromCharCode(99) // true
```

→ `fromCharCode`에 배열이 들어갈 수 있는지 몰랐는데 배열로 넣어 string으로 사용이 가능하다는 사실!

- 배열에서 사용되는 `concat`, `slice`는 배열메서드와 마찬가지로 작동한다.
    - 하지만 indexOf 는 다르다. indexOf의 인자는 문자열이지 숫자가 아니다. 이 메서드는 문자열의 모든 요소를 뒤져서 인자로 전달된 문자열과 일치하는 첫번째 요소 순서를 찾아서 그 색인을 반환한다.

    ```jsx
    example_array_string.indexOf(String.fromCharCode(111,114)) // 1
    String.fromCharCode(111,114) // "or"
    
    // corn 에서 or 이 1번째에 있으
    
    example_array_string.indexOf(String.fromCharCode(111,110)) // -1
    String.fromCharCode(111,110) // "on"
    ```


- `startsWith`, `endsWith`, `contains` 는 `indexOf`와 `lastIndexOf` 를 사용한 래퍼함수이다.
    - startsWith(searchString) | startsWith(searchString, position)
        - 문자열의 시작지점에서 탐색할 문자열(정규표현식이 올수 없다.)
        - position optional한 값으로 탐색할위치. default: 0
    - endsWith(searchString[,length])
        - searchString의 경우 위와 동일
        - length optional한 값으로 찾고자하는 문자열의 길이값이며, 기본값은 문자열 전체의 길이이다. 문자열의 길이값은 문자열 전체길이 안에서만 존재해야한다.

```jsx
const str1 = "Saturday nights plan";
str1.startsWith("Sat") // true
str1.startsWith("Sat", 3) // false

var str = "To be, or not to be, that is the question.";

console.log(str.endsWith("question.")); // true
console.log(str.endsWith("to be")); // false
console.log(str.endsWith("to be", 19)); // true
// endsWith의 경우 해당 문자열이 끝난 지점 + 1 로 생각하면 간편
```

- 문자열은 그 값이 같을 경우 === 연산자에 의해 동일하다고 판단하지만 배열은 값이 같아도 배열이 서로 다르면 다른거임.

```jsx
example_array_string === example_array_string // true
example_array === [99,111,114,110] // false
example_array_string === String.fromCharCode(999,111,114,110) // true
```

두번째가 거짓인이유는 메모리 할당한곳이 다르다고 생각하면 될듯!

문자열의 동등성검사는 아주 강력한 기능으로 값이 같으면 같은 객체라고 취급할 수 있기에 심벌(Symbol)형이 필요가 없다. ('심볼(symbol)'은 유일한 식별자(unique identifier)를 만들고 싶을 때 사용)

```jsx
let id = Symbol();
// id는 새로운 심볼이 된다.

let tt = Symbol('tt');
let tt2 = Symbol('tt');
//심볼을 만들 때 심볼 이름이라 불리는 설명을 붙일 수도 있습니다. 심볼 이름은 디버깅 시 아주 유용
tt !== tt2 // true tt, tt2는 같지 않다.
```

### 유니코드

자바스크립트는 아주 많은 체계적이고 문법적인 방법으로 유니코드를 지원한다. 유니코드 표준은 절대 사용해서는 안되는 문자가 아닌 몇가지 코드에 대해 규정하지만 자바스크립트는 그런거 신경안쓴다. 16비트코드는 다 허용한다. 문장려 리터럴은 0개 이상의 유니코드 캐릭터를 큰따옴표 (”) 로 묶어서 만들어진다. 작은따옴표(')도 사용가능하지만 그럴 필요가 없어서 추천하지는 않는다고 한다.

더하기(+) 연산자는 문자열 연결에 사용된다. 숫자더하기에도 사용한다. 만약 더하기가 아닌 연결을 하고싶다면 피연산자들중 최소한 하나가 문자열인지 확인해야한다. 한가지방법은 피연산자중 하나를 문자열 리터럴로 사용하는것이다.

```jsx
example_array_string === "corn" // true
"uni" + example_array_string // "unicorn"
3 + 4 // 7
String(3) + 4 // 34 (string)
3 + String(4) // 34 (string)
```

문자열 리터럴은 반드시 한줄로 끝나야한다.

`\` 의 경우 이스케이프문자로서 문자열 리터럴내부에 큰따옴표, 백슬래시(\), 개행문자를 포함 할 수 있게 해준다.

문자열은 만들어지면 동결된다. 한번 만들어지면 변경할 수 없다.
하지만 문자열의 일부분을 복사해 새로운 문자열을 만들 수 있다. 또한 문자열을 연결해 새로운 문자열을 만들 수 있다.

```jsx
const mess = "monster";
const the_four = "uni" + example_array_string + "rainbow butterfly" + mess;
// "unicorn rainbow butterfly monster"

const first = the_four.slice(0,7); // unicorn
const last = the_four.slice(-7); // monster
const parts = the_four.split(" "); // ["unicorn", "rainbow", "butterfly", "monster"]
parts[2][0] === "b" // true
"*".repeats(5); // "*****"
parts[1].padStart(10, "/"); // "///rainbow"
```

- [slice vs](https://www.notion.so/substr-VS-substring-VS-slice-b2488b3f6e9b4616856e4874a965ae2f?pvs=21) substring vs substr (splice 는 배열에 사용됨 - splice 는 원본이 훼손됨, slice는 복제, substr 이 splice같은 원리, substring이 slice 같은 원리)
- repeats는 해당문자열이 숫자만큼 반복된 문자열 return
- 기존문자열.padStart(길이, 추가문자열) ⇒ 길이 - 기존문자열길이 만큼 추가문자열이 기존문자열 앞에 붙음
    - `"123".padStart(5,"456") // “45123”`
- `trim` 의 경우 문자열 양끝 공백을 제거하고 원본문자열을 수정하지 않고 새로운 문자열을 반환한다.
    - 한쪽끝의 공백만 제거한 새로운 문자열을 반환하려면 `trimStart()` || `trimEnd()` 를 사용하면 된다.

(필요한지 모르겟어요… 유니코드를 써야하는 프로그램을 만들때 유의해야하는 부분이라고 합니다 109페이지 참고)

유니코드의 원래목적은 세상에 존재하는 모든언어를 16비트로 표현하는것이였다. 나중에는 전 세계의 언어를 21비트로 표현하는것으로 바뀌었다. 아쉽게도 자바스크립트는 유니코드가 16비트로 표현하려던 시절에 설계되었다.

유니코드는 자바스크립트의 문자를 받아서 코드유닛 , 코드포인트 둘로 나눈다.
코드유닛은 16비트 문자중 하나를 의미하고, 코드포인트는 문자에 해당하는 숫자를 표시한다.
코드포인트는 하나나 그 이상의 코드 유닛들로 구성된다.

### 템플릿 문자열 리터럴

템플릿 문자열 리터럴은 여러줄에 걸쳐 쓸 수 있는 문자열 리터럴이다. 백틱으로 더 많이 알려진 `가 구분자로 사용된다.

```jsx
const old_form = (
	"Can you"
	+"\n believe how"
	+"\n incredible"
);

const new_form = `Can you
believe how
incredible`;

old_form === new_form // true;
```

new_form 이 더 나아보이지만, 감수해야하는 것들이 있다. 바로 언어의 가장 큰 문법적 구조가 공백 문자에 의해 구분된다는 것이다. 이로인해 발생할 수 있는 잠재적인 오류는 아주 크다. 딱보기에 헷갈린다. incredible이라는 단어 뒤 공백이 있어보이냐고 물으면 old에서는 분명히 없다이지만, new에서는 모를 수 잇다.

일반적으로 아주 긴 형태의 텍스트는 프로그램에 저장하지 않다. 그 대신 텍스트 에디터나 json에디터, 데이터베이스등에 저장하고 프로그램은 자원의 일부로서 텍스트에 접근하고 사용하는 편이다. 이런 새로운 문법은 좋지않은 습관을 유발한다.

템플릿문자열리터럴의 이점중 하나는 문자보간을 제공한다는것이다.
템플릿문자열리터럴안에 `${}` 를 써서 올바른 표현식을 넣을 수 있다. 이 표현식은 계산되어 문자열로 변경된 다음 템플릿 문자열리터럴에 삽입된다.

여기서 위험한점은 삽입되는 내용이 `<script src=”http://…”>` 과 같이 심술궂은 내용일수 있다. 웹환경에서는 의심스러운 코드를 활성화 할 수 잇는 많은 방법이 있다. 템플릿을 사용하는것역시 보안 위협성을 증가시키는것으로 본다. 대부분의 템플릿 도구들이 이러한 위험을 줄여주는 장치를 제공하지만 여전히 보안취약점이 발생한다. 어쨋든 문자보간을 제공하는 템플릿문자열리터럴처럼 템플릿은 기본적으로 안전하진 않다. 보안위혐성을 완화시킬수 있는 방법은 태그(tag)함수라고 불리는 특별한 함수를 쓰는것이다. 템플릿문자열리터럴 바로 앞에 함수표현식을 쓰면 해당 함수가 호출되는데 해당 함수에 대한 인자로 템플릿문자열과 표현식의 값을이 전달된다. 태그는 이렇게 전달된 값들에 대해 필터링하고, 인코딩하고, 조립한 다음 반환한다. dump함수는 입력된 값들을 단순히 사람이 읽을 수 있는 형태로 반환하는 태그함수이다.

```jsx
function dump(strings, ...values) {
	return JSON.stringify({
		strings,
		values
	}, undefined, 4);
}

const what = "ram";
const where = "rama lama ding dong"

`who put the ${what} in the ${where}?`

const result = dump`Who put the ${what} in the ${where}?`;
// The result is `{
//   "string": [
//          "who put the",
//          " in the ",
//          "?"
//           ],
//   "values": [
//          "ram",
//          "rama lama ding dong"
//           ]
// }
```

문자열은 배열형태로 전달되는데 반해, 값들은 개별인자들로 전달되는것이 좀 이상하다? 태그함수는 제대로 만들고 제대로 쓴다면 XSS(cross site scripting - 게시판이나 웹메일등에 자바스크립트와 같은 스크립트 코드를 삼입해 개발자가 고려하지 않은 기능이 작동하게 하는 치명적일 수 잇는 공격)나 그외에 보안취약점을 막을 수 있다. 하지만 기본적으로 템플릿은 위험하다는 사실을 명심해야한다.

템플릿 문자열은 언어에 아주많은 새로운 문법과 기법, 그리고 복잡성을 추가 했다. 좋지않은 습관을 조장하지만 큰 문자열 리터럴을 제공하는데 효과가 있다는건 사실이다.

### 정규표현식

정규표현식 객체는 문자열에 대해서 일치하는 패턴이 있는지 찾는다. 예컨대 정규표현식 객체로는 JSON텍스트를 분석할 수 없다. JSON의 중첩구조를 분석하려면 푸시다운(pushdown - 가장 나중에 저장된 정보가 가장먼저 검색되는 기억시스템을 일컫는말이며 스택 자료구조라고 볼 수 있다.) 과 같은 능력이 필요하다. 정규표현식 객체는 푸시다운저장능력은 없다. 그대신 json 텍스트를 토큰으로 분해해 json 파서를 좀 더 쉽게 만들수는 있다.

### 토큰화

컴파일러는 컴파일 과정에서 소스코드를 토큰화 한다. 에디터나 코드데코레이터, 정적 분석기, 매크로 처리기, 최소화 툴(minifiers)등 에서도 토큰화를 사용한다. 에디터와 같이 상호작용이 많이 일어나는 프로그램에서는 이런 토큰화가 아주 빨라야한다. 안타깝게도 자바스크립트는 토큰화하기에 아주 어려운 언어이다. 이는 정규표현식 리터럴과 자동세미클론삽입간의 상호작용때문이다. 이상호작용이 프로그램이 코드를 분석할때 헷갈리게 만든다.

```jsx
return /a/i; // return a regular expression object
return b.return /a/i; // return ((b.return)/a) / i
```

이코드만 봐도 자바스크립트가 코드 전체를 분석하지 않고서는 제대로 토큰화를 하기 힘들다는 사실이 있다.

템플릿문자열 리터럴때문에 더 힘든부분도 잇다. 리터럴안에 표현식이 내장 될 수도 있다. 그리고 그 표현식안에 또다른 템플릿문자열 리터럴이 중첩되어있을수도 있다. 이런중첩은 얼마든지 깊어질 수 있다. 표현식이외에도 백틱이있는 문자열이나 정규표현식리터럴이 있을수도있다. 백틱만 해도 이것이 현재 리터럴을 끝낸다는 의미인지 아니면 중첩된 다른 리터럴을 시작한다는 의미인지 알 수 없다. 그래서 전체적인 구조가 도대체 어떻게 된다는것인지 파악하기 힘들다. ``${`${”\”}`”}`}`` 이보다 더 심한경우도 있다. 표현식에는 정규표현식 리터럴과 빠진 세미클론, 그리고 더 많은 템플릿문자열리터럴을 가지는 함수들이 포함되어있을수 있다.
에디터와 같은 툴들은 프로그램이 제대로 잘 동작한다고 가정할것이다. 자바스크립트에는 전체분석을 하지 않고도 토큰화 할 수있는 부분이 있다 하지만, 이런부분에만 집중하면, 여러분이 만든 툴은 제대로 작동하지 않을것이다...?(걍 작가는 템플릿문자열 안쓰는걸 추천함)

### fullfill

템플릿문자열의 문자보간 대신 쓸만한건 fulfill 함수이다. 심벌릭변수가 포함된 문자열 그리고 [심벌릭](https://mentor75.tistory.com/entry/%EC%8B%AC%EB%B3%BC%EB%A6%AD-%EB%AC%B8%EC%9E%90-Entity%ED%91%9C) 변수를 대체할 수 잇는값이 포함된 객체나 배열 인자로 받는다. 또한 값을 대체할때 인코딩 할 수 있는 함수나 함수객체를 추가로 전달이 가능하다. 해당부분 내용 인터넷 서칭에도 많이 언급되는 내용이 아닌지라 패스 했습니다~~