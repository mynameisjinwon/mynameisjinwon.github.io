---
title:  "열혈 C 프로그래밍 문제6"
excerpt: "오늘은 구조체에 대해서 공부를 해 보았다. "

categories:
  - C
tags:
  - C
 
date: 2021-06-03
last_modified_at: 2021-06-04
---

###### 구조체 변수는 다음과 같이 선언 할 수 있다.

```c
struct point {
	int xpos;
	int ypos;
} pos1, pos2; 
```

이렇게 선언된 구조체 변수 pos1, pos2는 전역변수처럼 사용할 수 있다.

```c
#include<stdio.h>

struct point {
	int xpos;
	int ypos;
} pos1, pos2; //이거 전역변수임?

void simple(void)  {
	printf("%d %d\n",pos1.xpos, pos1.ypos);
}
void func(void) {
	printf("%d %d\n", pos2.xpos, pos2.ypos);
}

int main(void) {
	pos1.xpos = 2;
	pos1.ypos = 3;
	pos2.xpos = 4;
	pos2.ypos = 5;
	
	simple();
	func();
	
	return 0;
}
```

구조체 변수는 초기화리스트를 사용해 배열처럼 초기화 할 수 있다. 구조체를 정의할 때 함께 선언한 구조체 변수는 이미 선언되어있기 때문에 초기화 리스트를 사용해서 변수의 값을 수정할 수 없다. 

###### 1.  문자열 형태의 종업원 이름과 문자열 형태의 주민등록번호 그리고 정수형태의 급여정보를 저장할 수 있는 employee 구조체를 정의해보자.

```c
#include<stdio.h>

typedef struct employee {
	char name[20];
	char pID[20];
	long salary;
} Employee;
void putComma(long money) { //급여정보에 1000단위마다 ,를 찍는 함수 
	int count=0, i;
	while(money % 1000 == 0) {
		money /=  1000;
		count++;
	}
	printf("%d", money);
	for(i=0;i<count;i++)
		printf(",000");
	printf("\\\n");
	
}
void showEmpInf(Employee emp) {
	printf("이름 : %s\n", emp.name);
	printf("주민 번호 : %s\n", emp.pID);
	putComma(emp.salary);
}
int main(void) {
	Employee emp;
	int i; 
	
	fputs("종업원의 이름을 입력하세요 : ", stdout);
	scanf("%s", emp.name);
	fputs("주민번호를 입력하세요 : ",stdout);
	scanf("%s", emp.pID);
	fputs("급여를 입력하세요 : ", stdout);
	scanf("%d", &emp.salary);
	
	showEmpInf(emp);
	
	return 0;
}
```

###### 2. employee 구조체를 기반으로 길이가 3인 배열을 선언하자 그리고 세명의 정보를 입력받아보자

```c
#include<stdio.h>

typedef struct employee {
	char name[20];
	char pID[20];
	long salary;
} Employee;
void putComma(long money) { //급여정보에 1000단위마다 ,를 찍는 함수 
	int count=0, i;
	while(money % 1000 == 0) {
		money /=  1000;
		count++;
	}
	printf("%d", money);
	for(i=0;i<count;i++)
		printf(",000");
	printf("\\\n");
}
void insertEmpInf(Employee emp[]) {
	int i;
	for(i=0;i<3;i++) {
	fputs("종업원의 이름을 입력하세요 : ", stdout);
	scanf("%s", emp[i].name);
	fputs("주민번호를 입력하세요 : ",stdout);
	scanf("%s", emp[i].pID);
	fputs("급여를 입력하세요 : ", stdout);
	scanf("%d", &emp[i].salary);
	}
}
void showEmpInf(Employee emp[]) {
	int i;
	for(i=0;i<3;i++) {
		printf("이름 : %s\n", emp[i].name);
		printf("주민 번호 : %s\n", emp[i].pID);
		printf("급여 : "); 
		putComma(emp[i].salary);
	}
}
int main(void) {
	Employee emp[3];
	int i; 
	
	insertEmpInf(emp);
	showEmpInf(emp);
	
	return 0;
}
```

배열은 주소값을 가지고 있으니 배열을 인자로 전달하는 것은 call by reference인가? 라는 궁금증이 생겨 검색을 해보았다. 결과는 C언어는 call by reference는 지원하지 않는다고 한다. 주소값을 넘겨주는 것도 주소값의 복사본을 넘겨주는 것이라서 call by value 에 속한다. 주소값을 복사해 넘겨주는 것을 call by address라고 한다. 

**C언어의 함수 인자 전달은 모두 Call by Value 방식이지만 포인터를 사용해 주소값을 복사해 넘겨주는 방식으로 Call by Reference를 구현한다.**

###### 구조체 포인터 변수 

```c
#include<stdio.h>
typedef struct point {
	int xpos;
	int ypos;
} Point;
int main(void) {
	Point pos1 = {1,2};
	Point pos2 = {100, 200};
	Point * pptr = &pos1;
	
	printf("%d %d\n", (*pptr).xpos, (*pptr).ypos);
	pptr->xpos *= 420;
	pptr->ypos *= 420;
	printf("%d %d\n", (*pptr).xpos, (*pptr).ypos);
	
	return 0;
}
```

(*ptr).xpos = ptr->xpos : 포인터 변수 ptr이 가리키는 구조체 변수의 멤버 xpos의 값을 의미한다.

구조체를 정의할 때 구조체 타입의 포인터 변수를 추가할 수 있다.

```c
#include<stdio.h>
typedef struct point {
	int xpos;
	int ypos;
} Point;
typedef struct circle {
	double radius;
	Point * center;
} Circle;
int main(void) {
	Point cen = {2,7};
	double rad = 5.5;
	
	Circle ring = {rad, &cen};
	printf("원의 반지름 : %g\n", ring.radius);
	printf("원의 중심 (%d, %d)", ring.center->xpos, ring.center->ypos);
    // ring.center->xpos = (*ring.center).xpos;
	
	return 0;
}
```

구조체를 정의할 때 같은 type의 구조체 포인터 변수를 추가할 수 있다.

```c
#include<stdio.h>

typedef struct point {
	int xpos;
	int ypos;
	struct point * ptr;
} Point;
int main(void) {
	int i;
	
	Point pos[3] = { 
		{1,1},
		{2,2},
		{3,3}
	};
	//Point pos1 = {1,1};
	//Point pos2 = {2,2};
	//Point pos3 = {3,3};
	pos[0].ptr = &pos[1];
	pos[1].ptr = &pos[2];
	pos[2].ptr = &pos[0];
	
	for(i=0;i<3;i++) 
		printf("점%d 좌표 (%d, %d) 연결된 점 : (%d, %d)\n", i+1, pos[i].xpos, pos[i].ypos, pos[i].ptr->xpos, pos[i].ptr->ypos);
    return 0;	
}
```

연결리스트를 이렇게 구현했던것 같다. 기억이 날락말락 한다. 

pos[i].ptr->ypos = (*pos[i].ptr).ypos;


###### 3. 구조체 변수를 대상으로 저장된 값을 서로 바꿔주는 함수를 작성해보자

```c
#include<stdio.h>
typedef struct point {
	int xpos;
	int ypos;
} Point;
Point pos3, pos4;
void SwapPoint1(Point * pos1, Point * pos2) {
	Point temp;
	temp = *pos1;
	*pos1 = *pos2;
	*pos2 = temp;
}
void SwapPoint2(Point pos1, Point pos2) {
	Point temp;
	temp = pos1;
	pos1 = pos2;
	pos2 = temp;
}
int main(void) {
	Point pos1 = {2, 4};
	Point pos2 = {5, 7};
	pos3.xpos = 2, pos3.ypos =4;
	pos4.xpos = 5, pos4.ypos = 7;
	
	printf("before swap\n");
	printf("pos1 : (%d,%d)\npos2 : (%d,%d)\n", pos1.xpos, pos1.ypos, pos2.xpos, pos2.ypos);
	
	SwapPoint1(&pos1, &pos2);
	printf("\nAfter swap\n");
	printf("pos1 : (%d,%d)\npos2 : (%d,%d)\n", pos1.xpos, pos1.ypos, pos2.xpos, pos2.ypos);
	
	SwapPoint2(pos3, pos4);
	printf("\nAfter swap2\n");
	printf("pos3 : (%d,%d)\npos4 : (%d,%d)\n", pos3.xpos, pos3.ypos, pos4.xpos, pos4.ypos);
	
	return 0;
}
```

문제에는 변수를 대상으로 혹은 구조체 변수의 주소값을 대상으로 Swap을 해보라고 했는데, 변수를 인자로 보내서 어떻게 바꿀 수 있을까? 전역변수를 써서 하긴 했는데 다른 방법을 한번 찾아 보자. 



###### 4.  좌상단의 좌표가 (0,0) 우하단의 좌표가 (100, 100)인 좌표평면상의 직사각형 정보를 표현하기 위한 구조체 Rectangle을 정의하되, 다음 구조체를 기반으로 정의해 보자. (이전 문제에 사용한 point 구조체) 그리고 Rectangle 구조체 변수를 인자로 전달받아서 해당 직사각형의 넓이를 계산해서 출력하는 함수와 직사각형을 이루는 네 점의 좌표정보를 출력하는 함수를 각각 정의해보자, 단, 좌표평면상에서 직사각형을 표현하기 위해서 필요한 점의 갯수는 4개가 아닌 2개이니, 직사각형을 의미하는  Rectangle 변수 내에는 두 점의 정보만 존재해야한다.

```c
#include<stdio.h>
typedef struct pooint {
	int xpos;
	int ypos;
} Point;
typedef struct rectangle {
	Point ltop;
	Point rbot;
} Rect;
void CalArea(Rect rect) {
	int width, height;
	
	width = rect.rbot.xpos - rect.ltop.xpos;
	height = rect.rbot.ypos - rect.ltop.ypos;
	
	printf("사각형의 넓이는 : %d\n", width * height);
}
void ShowPoint(Rect rect) {
	printf("사각형의 좌표는 : \n");
	printf("(%3d, %3d)\t(%3d, %3d)\n\n", rect.ltop.xpos, rect.rbot.ypos, rect.rbot.xpos, rect.rbot.ypos);
	printf("(%3d, %3d)\t(%3d, %3d)\n", rect.ltop.xpos, rect.ltop.ypos, rect.rbot.xpos, rect.ltop.ypos);
	
}
int main(void) {
	Rect rect = { {0,0}, {100,100} };
	int area = 0;
	
	CalArea(rect);
	ShowPoint(rect);
	
	return 0;	
}
```

CalArea 함수의 width, height 변수를 지우는 게 나을까 아닐까? 생각해봤는데 지우는게 나을것같다.
불필요한 연산을 줄이는 것이 더 좋은 프로그램을 만드는 방법인것 같다. 지워서 실행결과를 비교해봤는데 
실행시간이 많이 감소되었다.

