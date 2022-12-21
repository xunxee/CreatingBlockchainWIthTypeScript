# **객체를 받아서, 객체를 반환(RORO)**

**RORO**는 Receive an object, return an object를 나타낸다. RORO 패턴으로 얻을 수 있는 **이점**은 아래와 같다.

- 네임드 파라미터
- 더 명확한 디폴트 파라미터
- 더 풍부한 리턴 값
- 쉬운 함수 컴포지션

## **네임드 파라미터(Named Parameters)**

주어진 역할(Role)에 있는 사용자(Users) 목록을 리턴하는 함수가 있다. 각 사용자의 연락처 정보와 비활성 사용자를 포함하는 또 다른 옵션을 제공해야한다.

```jsx
function findUsersByRole (
  role,
  withContactInfo,
  includeInactive
) {...}
```

그리고 이 함수의 호출은 아래와 같이 이루어진다.

```jsx
findUsersByRole('admin', true, true);
```

그런데 **마지막 두 매개변수**`(“true”, “true”)`**는 굉장히 모호**하다. 우리가 만드는 앱이 연락처(Contant Info)는 필요 없지만 비활성 사용자(Inactive Users)는 대부분 필요하다면, **필요가 없는 경우에도 우리는 두 번째 파라미터를 항상 넣어 주어야** 한다. 이것은 코드를 이해하기 어렵고 작성에 있어 까다롭게 만든다.

그 대신 아래의 코드와 같이 하나의 객체로 이를 받을 수 있다.

```jsx
function findUsersByRole ({
  role,
  withContactInfo,
  includeInactive
}) {...}
```

위의 코드는 기존의 함수 코드와 비슷해보이지만 세 개의 파라미터를 받는 대신  `role`,  `withContantInfo`, `includeInactive`라는 프로퍼티를 가진 **단일 객체**를 받게 된다. 아래의 코드에서도 단일 객체를 받아 구조 분해 할당을 시행한다.

```jsx
// 좌항은 우황인 단일 객체를 받아 "구조 분해 할당" 한다.
const { name, address, hobbit } = { 
	name: 'gunhee', address: 'Seoul', hobbit: 'coding' 
};
```

이제는 ES2015에 소개된 자바스크립트의 **구조분해할당**(Destructuring) 기능으로 아래의 코드와 같이 함수를 호출할 수 있다.

```jsx
// 모던 자바스크립트 Deep Dive 참고
const user = { firstName: 'Ungmo', lastName: 'Lee'};

//ES6 객체 디스트럭처링 할당  
const { lastName, firstName } = user;

console.log(firstName, lastName); //ungmo Lee
```

```jsx
function findUsersByRole ({
  role,
  withContactInfo,
  includeInactive
}) {...}

findUsersByRole({
  role: 'admin',
  withContactInfo: true,
  includeInactive: true,
});
```

이제 아래의 코드와 같이 **매개변수를 생략하거나 재정렬하는 것은 더 이상 문제가 되지 않는다**. 이것은 **읽고 이해하기에 훨씬 쉬워**진다.

```jsx
findUsersByRole({
  withContactInfo: true,
  role: 'admin',
  includeInactive: true,
});
```

```jsx
findUsersByRole({
  role: 'admin',
  includeInactive: true,
});
```

한 가지 중요한 사항은 모든 **매개변수를 선택적으로 지정하려면** 즉, 아래의 호출이 유효하다면

```jsx
findUsersByRole();
```

…**매개변수 객체의 기본값을 아래의 코드와 같이 설정**해야한다.

```jsx
function findUsersByRole ({
  role,
  withContactInfo,
  includeInactive
} = {}) {...}
```

**매개변수 객체에 대한 destructuring 사용**의 추가 이점은 **불변성을 장려한다**는 것이다. 우리가 함수로 가는 도중에 객체에 destructuring 할 때 우리는 객체의 프로퍼티를 새로운 변수에 할당한다. 이러한 변수의 값을 변경해도 **원래 객체는 변경되지 않는다**.

```jsx
const options = {
  role: 'Admin',
  includeInactive: true
};

findUsersByRole(options)

function findUsersByRole ({
  role,
  withContactInfo,
  includeInactive
} = {}) {
  role = role.toLowerCase()
  console.log(role) // 'admin'
  ...
}

console.log(options.role) // 'Admin'
```

단, **매개변수 객체의 속성 중 하나가 “배열” 또는 “객체”**인 경우 **얕은 복사가 진행되므로 원본이 변경될 수** 있다.

## **더 명확한 디폴트 파라미터**

ES2015 자바스크립트 함수를 사용하면 **디폴트 파라미터**(default parameters)를 정의할 수 있다. 우리는 위의 `findUsersByRole` 함수의 매개변수 객체에 `={}`를 추가하여 디폴트 파라미터를 사용했다.

전통적인 디폴트 파라미터를 사용하면 `findUsersByRole` 함수는 아래의 코드와 같다.

```jsx
function findUsersByRole (
  role,
  withContactInfo = true,
  includeInactive
) {...}
```

```jsx
function findUsersByRole (
  role,
  withContactInfo = true,
  includeInactive
) {
  console.log(role, withContactInfo, includeInactive);
}

findUsersByRole('Admin', undefined, true); // 'Admin' true true
```

그런데 위의 코드는 `includeInactive`를 `true`로 설정하려면 `withContactInfo`를 **명시적으로 undefined로 전달해야 디폴트 값을 유지**할 수 있다. 이것은 코드의 **가독성을 굉장히 해치는, 좋지 못한 방법**이다.

그래서 아래와 같이 **파라미터 객체**를 이용할 수 있다.

```jsx
function findUsersByRole ({
  role,
  withContactInfo = true,
  includeInactive
} = {}) {console.log(role, withContactInfo, includeInactive);}

findUsersByRole({
  role: 'Admin',
  includeInactive: true
}) // 'Admin' true true
```