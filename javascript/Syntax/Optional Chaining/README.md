## Optional Chaining(옵셔널 체이닝)

옵셔널 체이닝(`?.`) 을 사용하면 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근할 수 있다.

모던 자바스크립트를 공부하면서 가장 신박하다고 느낀 문법이고, 간결하면서 강력한 기능을 가지고 있다고 생각한다.

<br />

### 옵셔널 체이닝은 언제 사용할까?

```jsx
let user = {}; // 주소 정보가 없는 사용자

const zipcode = user.address.zipcode;
TypeError: Cannot read property 'zipcode' of undefined
```

위 예제를 살펴보자. 사용자들 정보를 관리하고 있는데, 여기서 몇몇은 주소 정보가 없다고 생각해보자. 무작위로 한 사용자의 zipcode를 조회했을 때, 위 오류를 겪을 수 있다.

```jsx
let user = {}; // 주소 정보가 없는 사용자

const zipcode = user && user.address && user.address.zipcode;
// undefined, 에러는 발생하지 않는다.
```

데이터를 안전하게 접근하기 위해서 위 코드처럼 작성해야 한다. 이에 많은 개발자들은 데이터를 접근함에 있어서 피로감을 느끼게 되었고, 이를 개선하고자 옵셔널 체이닝이라는 새로운 문법을 발표하게 되었다. 

<br />

### 옵셔널 체이닝에 대해 예제를 만들어 보자!

`?.`은 "**평가 대상이 undefined나 null이면 평가를 멈추고 undefined를 반환**"한다. 

예제를 통해 머리 속에 넣어두자!

```jsx
let user = {}; // 주소 정보가 없는 사용자

alert( user?.address?.zipcode );
// undefined, 에러는 발생하지 않는다.
```

엄청 간결해진 것을 볼 수 있다. 위 예제는 여러 사용자들에 대해 반복하여 zipcode를 탐색하는 예제로 봐야 옵셔널 체이닝의 강력한 힘에 놀라지 않을 수 없다 ㅎㅎ.. 

<br />

### 옵셔널 체이닝은 만능일까?

하지만, 클린한 코드를 만들기 위해서 옵셔널 체이닝 사용에 대한 몇 가지 약속이 있다.

1. `?.` **남용하지 말자.**

    ?. 문법은 존재하지 않아도 되는 대상에만 사용해야 한다. 자동차를 예로 들어보자.자동차는 여러가지 부품이 존재한다. `car.handle?.size` 이렇게 사용할 수 있을 것이다. 하지만, "**자동차에 핸들이 없다?"** 있을 수 없는 일이다. handle이 없다면 바로 알아차릴 수 있어야 한다. 즉, **필수값이 아닌 대상에 사용해야 한다.**

2. `?.` **맨 앞이 평가 대상이라면 그 대상은 꼭 선언되어 있어야 한다.**

    ```jsx
    car?.handle
    ReferenceError: user is not defined
    ```

    car 변수가 선언되어있지 않다면, 위와 같은 에러가 발생한다.

3. **구식 브라우저는 폴리필이 필요하다**

    비교적 최신 문법이기 때문에 구식 브라우저는 지원하지 않는다.
    특히, IE는 악이다...


<br />

### 옵셔널 체이닝 응용해 보자!

옵셔널 체이닝은 연산자가 아니다. `?.`은 **함수나 대괄호와 함께 동작하는 특별한 문법 구조체(syntax construct)**로 함수 관련 예시와 함께 존재 여부가 확실치 않은 함수를 호출할 때도 사용할 수 있다. `?.()`와 `?.[]`에 대한 사용 예제를 만들어보자.

```jsx
let car1 = {
	name: 'moring',
  accelerator() {
    console.log("달린다");
  },
	brake() {
    console.log("멈춘다");
	},
}

let car2 = {};

car1.accelerator?.(); // 달린다
car2.accelerator?.(); // undefined

alert( car1?.['name'] ); // moring
alert( car2?.['name'] ); // undefined

alert( car1?.['name']?.something?.existing); // undefined
```

`?.`은 delete와 조합해 사용할 수도 있습니다.

```jsx
delete car1?.brake; // car1가 존재하면, car1.accelerator 삭제
console.log(car1); // {accelerator: ƒ}, 멈출 추 없는 차가 된다.
```