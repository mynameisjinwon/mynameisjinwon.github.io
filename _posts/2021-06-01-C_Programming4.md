---
title:  "열혈 C 프로그래밍 문제4"
excerpt: "열혈 C 프로그래밍 책에 있는 프로그래밍 문제를 풀어보았다. 4  "

categories:
  - C
tags:
  - [C, 벌써6월, omg]
 
date: 2021-06-01
last_modified_at: 2021-06-01
---
### 2차원 배열 

```c
#include<stdio.h>

int main(void) {
    int arr1[3][3] = {0};
    int arr2[3] = {0};
    
    printf("%d\n%d\n%d\n%d\n%d\n", arr1, *arr1, &arr1, arr1[0], &arr[0][0]);
    printf("%d\n%d\n%d\n%d\n", arr2, *arr2, &arr2, &arr[0]);
}
```

2차원 배열의 경우 arr1, *arr1, &arr1, arr1[0] 모두 arr1의 시작 주소값을 의미한다.

1차원 배열의 경우 arr2, &arr2, &arr2[0] 모두 arr2의 시작 주소값을 의미한다. *arr2는 arr2[0]값을 의미한다.



### argc, argv

argc(Argument count)  : 프로그램 호출시 전달받은 인자의 수를 저장하는 변수

argv(Argument vector) : 프로그램 호출시 전달받은 문자열을 저장하는 배열

```c
#include<stdio.h>

int main(int argc, char *argv[]) {
	int i = 0;
	
	printf("전달된 문자열의 수 : %d\n", argc);
	
	for(i=0;i<argc;i++) 
		printf("%d 번째 문자열 : %s \n", i+1, argv[i]);
	
	return 0;
}
```

명령 프롬프트상에서 argcArgv.exe와 함께 문자열을 입력하면 

```
전달된 문자열의 수 : 3
1 번째 문자열 : argcArgv.exe
2 번째 문자열 : Hello
3 번째 문자열 : World!
```

이런 결과를 얻을 수 있다.



char *argv[]  = char ** argv, argv 는 char형 더블 포인터 변수

char형 포인터 변수를 저장하는 1차원 배열의 이름을 전달받을 수 있다. ( 문자열이 저장된 배열을 전달 받을 수 있다.)



### 도전! 프로그래밍 3

###### 1.  2차원배열 돌려가며 출력하기

```c
#include<stdio.h>
void printArr(int (*arr)[4], int nrow, int time) {
	int i, j, k;
	int temp[4][4]; //임시 배열 선언
	
	for( k=0; k<time; k++) {
		for(i=0;i<nrow;i++)
			for(j=0; j<4; j++) 
				temp[j][3-i] = arr[i][j]; //배열을 시계방향으로 90도 회전시킨다.
        
		for(i=0;i<nrow;i++) 
			for(j=0; j<4; j++) 
				arr[i][j] = temp[i][j]; // 임시배열에 있는 값을 배열 포인터에 저장
	}
}
int main(void) {
	int arr[4][4] = {
		{1, 2, 3, 4},
		{5, 6, 7, 8},
		{9, 10, 11, 12},
		{13, 14, 15, 16}
	};
	int nrow, i, j, time;
	
	printf("배열을 몇번 회전 시키시겠습니까? : ");
	scanf("%d", &time);
	
	nrow = sizeof(arr)/sizeof(arr[0]); 
	printArr(arr, nrow, time);
    
	for(i=0; i<nrow; i++) { //배열 출력
		for(j=0;j<4;j++)
			printf("%2d ", arr[i][j]);
		printf("\n");
	}
	return 0;
}
```



###### 2. 달팽이 배열 만들기 

```c
#include<stdio.h>

void snail(int (*arr)[50], int *num, int n, int *start) {
	int count = 0, i, j;
	
	if( n < 0 ) {
		return;
	}
	// 오른쪽으로 채우기 
	i = *start;
	for(j=*start;j<n;j++)
		arr[i][j] = (*num)++;	
	// 아래쪽으로 채우기 
	i = n-1;
	for(j=*start+1; j<n;j++)
		arr[j][i] = (*num)++;
	// 왼쪽으로 채우기 
	i = n-1;
	for(j=n-2;j>=*start;j--)
		arr[i][j] = (*num)++;
	// 위쪽으로 채우기 
	i= *start;
	for(j=n-2;j>*start;j--)
		arr[j][i] = (*num)++;
	(*start)++;
	
	snail(arr, num, n-1, start);
}
int main(void) {
	int arr[50][50] = {{0}};
	int i, j, n, start=0, num=1;
	
	printf("정수 입력 : ");
	scanf("%d", &n);
	
	snail(arr, &num, n, &start);
	for(i=0;i<n;i++) {
		for(j=0;j<n;j++)
			printf("%3d ", arr[i][j]);
		printf("\n");
	}
	return 0 ;
}
```



###### 3. rand()함수를 이용해서 0~ 99 사이의 난수 5개를 출력하는 프로그램

```c
#include<stdio.h>
#include<stdlib.h>

int main(void) {
	int i;
	
	printf("0~%d 사이의 난수\n", RAND_MAX);
	for(i=0;i<5;i++)
		printf("%d \n", rand()%100); // rand() % 100 <= 99
	return ;
}
```

rand() 함수는 진짜 난수가 아닌 의사 난수(pseudo_radom number)를 발생시킨다. rand()함수로 발생한 난수들은 프로그램을 몇번을 실행해도 똑같은 값을 출력한다. 

이러한 문제를 해결하기위해 srand()함수를 사용한다. srand()함수는 seed값을 인자로 받는다. rand()함수는 이 seed값에 따라 다른 난수를 발생시킨다. 그러나 seed값이 같으면 항상 같은 난수를 발생시킨다는 문제점은 해결되지 않는다.

이 문제를 해결하기위해 시스템시간을 이용해 seed값을 다르게 전달해 프로그램을 실행할 때마다 다른 난수를 발생시키도록 할 수 있다.

srand((int)time(NULL)) 

time_t time( time_t *timeptr)

time() 함수는 1970년 1월 1일 0시 부터 인자값으로 전달 받은 시간까지 흐른 시간을 초단위로 반환한다. 현재시간까지 흐른 시간을 구한다면 time(NULL), time()함수를 사용하기 위해서는 time.h헤더 파일을 포함해야한다.

###### 4. 주사위 두개를 던졌을 때 그 결과를 예측하는 프로그램 

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

int main(void) {
	int i;
	srand((int)time(NULL)); // time(NULL) 1970.01.01 ~ 현재시간
	for(i=0;i<10;i++) 
		printf("주사위 %d의 예측값 : %d\n", i+1, rand()%6+1); //1<= 6으로 나눈 나머지 + 1 <= 6
	
	return;
}
```



###### 5.가위바위보 게임을 만들어보자!

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

int main(void) {
	int rsp, c_rsp, c_win=0, c_draw=0, lose=0;
	srand((int)time(0));
	
	while(lose != 1) {
		printf("바위는 1, 가위는 2, 보는 3 : ");
		scanf("%u", &rsp);	
		c_rsp = rand()%3+1;
		switch(c_rsp) {
			case 1 :
				if(rsp==1) {
					printf("당신은 바위, 컴퓨터는 바위, 비겼습니다!\n");
					c_draw++;
				}
				else if (rsp==2) {
					printf("당신은 가위, 컴퓨터는 바위, 당신이 졌습니다!\n");
					lose = 1;
				}
				else {
					printf("당신은 보, 컴퓨터는 바위, 당신이 이겼습니다!\n");
					c_win++;
				}
				break;
			case 2 :
				if(rsp==1){
					printf("당신은 바위, 컴퓨터는 가위, 당신이 이겼습니다!\n");	
					c_win++;
				}
				else if (rsp==2){
					printf("당신은 가위, 컴퓨터는 가위, 비겼습니다!\n");
					c_draw;
				}
				else {
					printf("당신은 보, 컴퓨터는 가위, 당신이 졌습니다!\n");
					lose = 1;
					break;
				}
				break;			
			case 3 : 
				if(rsp==1) {
					printf("당신은 바위, 컴퓨터는 보, 당신이 졌습니다!\n");
					lose = 1;
					break;
				}
				else if (rsp==2) {
					printf("당신은 가위, 컴퓨터는 보, 당신이 이겼습니다!\n");
					c_win++;
				}
				else {
					printf("당신은 보, 컴퓨터는 보, 비겼습니다!\n");
					c_draw++;
				}
				break;			
		}	
	}
	printf("\n게임 결과 : %d승 %d무 %d패\n", c_win, c_draw, lose);
	return;
}
```



###### 6. 컴퓨터랑 하는 야구게임 

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

int main(void) {
	int cnum[3] = {0}; //computer number
	int pnum[3] = {0}; //player number
	int i, j, count, strike, ball;
	
	srand((int)time(0));
	for(i=0; i<3; i++) 
		cnum[i] = rand()%10; 

	printf("\n");
	printf("Start Game!\n");
	while(1) {
		strike = ball = 0;
		count++;
		printf("3개의 숫자 선택 : ");
		for(i=0;i<3;i++)
			scanf("%d", &pnum[i]); // scanf("%d %d %d ", &pnum[0], &pnum[1], &pnum[2]); 이거 가능? 
		for(i=0;i<3;i++) {
			for(j=0;j<3;j++) {
				if(cnum[i]==pnum[j] && i == j)
					strike++;
				else if(cnum[i]==pnum[j] && i != j)
					ball++;
			}
		}
		printf("\n%d번째 도전 결과 : %d strike, %d ball\n", count, strike, ball);
		if(strike == 3) {
			printf("\nGame Over\n");
			break;
		}
	}
	return 0;	
}
```

6 6 2 일 때 6 6 6을 입력하면 2strike 4ball이 출력 되는 문제가 있다. 어떻게 해결해야 할까 

그게 아니다 문제를 잘못읽었다. 세 숫자는 서로 달라야한다. 나는 바보다 한글도 잘 못읽는다. 

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

int main(void) {
	int cnum[3] = {0}; //computer number
	int pnum[3] = {0}; //player number
	int i, j, count, strike, ball;
	
	srand((int)time(0));
	for(i=0; i<3; i++) {
		cnum[i] = rand()%10; 
		if(cnum[i-1] == cnum[i] && i > 0) // 직전의 수와 같은 수가 뽑힌다면 
			cnum[i] = rand()%10;	      
	}
	printf("\n");

	printf("Start Game!\n");
	while(1) {
		strike = ball = 0;
		count++;
		printf("3개의 숫자 선택 : ");
		for(i=0;i<3;i++)
			scanf("%d", &pnum[i]); 
		for(i=0;i<3;i++) {
			for(j=0;j<3;j++) {
				if(cnum[i]==pnum[j] && i == j) { 
					strike++;
				}
				else if(cnum[i]==pnum[j] && i != j) {
					ball++;
				}
			}
		}
		printf("\n%d번째 도전 결과 : %d strike, %d ball\n", count, strike, ball);
		if(strike == 3) {
			printf("\nGame Over\n");
			break;
		}
	}
	return 0;	
}
```

하지만 이것도 문제가 있다.  직전의 수와 같은 수가 뽑히면 13행의 조건문에 의해 새로운 난수를 뽑는데 새롭게 뽑은 숫자가 이전의 숫자와 같은 경우에는 직전의 수와 같은 수가 다시 배열에 저장된다. 또 첫번째 숫자와 마지막 숫자가 같을 경우에는 수정을 하지 않는다.

```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

int main(void) {
	int cnum[3] = {0}; //computer number
	int pnum[3] = {0}; //player number
	int i, j, count, strike, ball;
	
	srand((int)time(0));
	for(i=0; i<3; i++) {
		cnum[i] = rand()%10; 
		printf("%d", cnum[i]);
		while( (cnum[i-1] == cnum[i] && i >0) || (i==2 && cnum[0]==cnum[i])) 	
			cnum[i] = rand()%10;
	}
	printf("\n");
	for(i=0;i<3;i++)
		printf("%d", cnum[i]);
	printf("\n");

	printf("Start Game!\n");
	while(1) {
		strike = ball = 0;
		count++;
		printf("3개의 숫자 선택 : ");
		for(i=0;i<3;i++)
			scanf("%d", &pnum[i]); 
		for(i=0;i<3;i++) {
			for(j=0;j<3;j++) {
				if(cnum[i]==pnum[j] && i == j) {
					strike++;
				}
				else if(cnum[i]==pnum[j] && i != j) {
					ball++;
				}
			}
		}
		printf("\n%d번째 도전 결과 : %d strike, %d ball\n", count, strike, ball);
		if(strike == 3) {
			printf("\nGame Over\n");
			break;
		}
	}
	return 0;	
}
```

14행의 if문을 while반복문으로 바꾸고 조건식을 추가했다. 
