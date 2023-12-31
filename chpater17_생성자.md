# 17장 생성자


상속은 아주 강력한 코드 재사용 기법이다

상속은 ‘거의 같지만, 예외가 있다’ 라는 생각에 기초를 두고 있다

하지만 프로그램이 복잡해지면서, 상속은 많은 문제를 일으킨다.

- 상속은 클래스 사이의 강한 결합도를 유발한다. 한 클래스에 가해진 변경은 의존적인 다른 클래스에 문제를 일으킨다.
- 타입에 대한 지나친 의존도 문제도 있다
- 각각의 속성에 대한 getter, setter 메서드가 지나치게 강조되며, 최악의 설계에서는 속성들이 공개되어서 객체가 알아차리지 못하는 사이 변경될 수도 있다.
- 좋은 설계에서는 속성들이 숨겨져 있고 메서드들이 속성 값을 단순히 변경하는 것이 아닌 트랙잭션을 처리한다.

## 생성자

- 객체는 단단하게 보호되어야 하고, 이것이 가장 좋은 캡슐화 방법이다
- 데이터에 직접 접근할 수 있는 방법이 없어야 하고, 이것이 좋은 모듈화 설계이다
- 자바스크립트의 아주 좋은 것 중 하나가 바로 객체 리터럴이다
- 객체의 종류는 두 가지
  - 메서드만 가지고 있는 단단한 객체 (hard object)
  - 데이터만 가지고 있는 부드러운 데이터 객체(soft data object)
- 단단한 객체를 문자열화 해야 한다면 해당 객체에 toJSON 메서드를 정의해줘야 한다
  (JSON.stringfy 사용하면 그냥 빈 객체로 보고 메서드를 무시하고 데이터를 숨겨버린다) - 자세한 내용은 22장에서

## 생성자 매개변수

- 열 개의 인자를 전달받는 것보다 매개변수로 하나의 객체만 전달받는 생성자를 만드는게 영리하다
  - key 문자열 값이 코드 자체를 문서화해 줍니다. 호출할 때 어떤 인자가 어떤 일을 하는지 코드 자체가 알려 주기 때문에 가독성이 증가한다
  - 인자의 순서가 상관없어진다
  - 기존 코드 수정 없이 새로운 인자의 추가가 가능하다
  - 쓸모없는 매개변수는 무시된다

    ```jsx
    // X
    function my_little_constructor(name, mana_cost, color, type...) {
    }
    // O
    function my_little_constructor(spec) {
    	let {
    		name, mana_cost, color, type, supertypes, types, subtypes, text, 
    		flavor, power, toughness, loyalty, timeshifted, hard, life
    	} = spec;
    }
    ```

  ## 컴포지션

  - 자바스크립트는 클래스가 없는 언어이지만 전형적인 클래스 형태의 프로그램을 만들 수 있다
  - ‘거의 다 같지만 약간 다른’ 것을 만드는 대신, 여기저기서 조금씩 가져와서 만들 수 있다 (함수 컴포지션)
  - 생성자는 원하는 상태 관리나 동작을 쓰기 위해서 다른 생성자를 호출해서 쓸 수 있다.
  - 이렇게 코드를 재사용하는 방식은 상속과 비슷하지만 강한 결합이 없다는 점에서 더 좋다.

## 크기

- 객체를 생성하는 방법은 프로토타입을 쓰는 방법보다 메모리를 더 많이 사용한다
  - 프로토타입 사용 시 프로토타입 객체가 메서드를 가진 프로토타입에 대한 참조만 가진다
    위 방법은 모든 단단한 객체가 모든 객체의 메서드를 다 가진다
  - 최근 메모리 용량의 증가와 비교해 보았을 때 큰 차이는 없다
  - 이 정도의 메모리 사용량 차이는 티끌에 불과하다 (얼마 전까지만 해도 메모리를 킬로바이트 단위로 계산했지만, 이제는 기가바이트 단위로 계산한다)
- 메모리 사용량은 증가하지만 모듈화에서 더 큰 이득을 얻을 수 있다
  - 속성 값 처리를 트랙잭션으로 만들면서 메서드 수는 줄이고 응집도를 더 높인다
- 프로토타입을 사용하는 경우에는 프로토타입 객체가 메서드를 가진 프로토타입에 대한 참조만 가지고 있는 반면, 여기서 사용하는