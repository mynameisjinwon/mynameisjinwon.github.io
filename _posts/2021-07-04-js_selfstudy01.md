---
title:  "js 배우기"
excerpt: "노마드 코더 무료강의를 통해 js를 배워보자"

categories:
  - JavaScript
tags:
  - JavaScript
 
date: 2021-07-04
last_modified_at: 2021-07-04

---

# js를 배워보자

### 노마드 코더 무료강의를 듣고있습니다.

### 변수 선언

코드를 적게 쓰는것이 에러도 적고 시간도 덜 걸린다.

```js
const a = 10; 
const myName = "Jinwon"

console.log(a);
console.log("hello" + myName);
```

변수이름에는 공백을 사용할 수 없다.  카멜 표기법을(Camel case) 사용한다.

myName, veryLongVarableName



### const and let

const : constant 값을 바꿀수가 없다. 바꾸지 않을, 바꿔서는 안되는 변수를 선언할 때 사용한다.

let : const 변수가 아닌 변수를 선언할 때 사용하는 키워드

```js
const a = 10;
let myName = "jinwon";
console.log(a + myName);
myName = "Hanjinwon";
console.log(a + myName);
a = 7; // 오류발생
```

기본적으로 대부분 변수는 const를 사용하고 

필요할 때만 let을 사용한다. 

const 선언을 통해서 실수로 변수를 바꾸는 것을 막을 수 있다.



### Boolean 

true or false 

```js
const amIUgly = false;
let something;
console.log(amIUgly);
console.log(something); // undefined 출력
```

true, false, null, undefined 는 데이터 타입이다.

null 은 자연적으로 발생하지않는다. 변수의 값이 비어있는지를 알려기 위해 사용한다.

undefined 변수가 메모리에 할당 되었지만 값이 없는 상태를 나타낸다. 초기화 되지않은 변수



### 배열 

동일한 타입의 데이터를 하나로 묶는 데이터구조

변수안에 데이터의 리스트를 가지는 것

```js
const mon = "mon";
const tue = "tue";
const wed = "wed";
const thu = "thu";
const fri = "fri";
const sat = "sat";

const daysOfWeek = [mon, tue, wed, thu, fri, sat];
console.log(daysOfWeek);
//배열 요소에 접근하기
console.log(daysOfWeek[0]); // mon 출력 내일월요일ㅋ
//요소 추가하기
daysOfWeek.push("sun"); // push는 함수 
```



### Object 

여러가지 속성을 가진 변수

```js
const player = {
    name : "jinwon",
    points : 10,
    fat : true
};
console.log(player);
console.log(player.name);
console.log(player["name"]);
player.fat = false;
player.height = 273;
```

object를 선언할때 const로 선언했지만

object의 속성값을 변경하는 것은 가능하다.

새로운 속성값을 추가하는 것도 가능하다.
