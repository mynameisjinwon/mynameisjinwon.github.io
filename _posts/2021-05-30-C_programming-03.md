---
title:  "열혈 C 프로그래밍 문제3"
excerpt: "열혈 C 프로그래밍 책에 있는 프로그래밍 문제를 풀어보았다. 3  "

categories:
  - C
tags:
  - C
 
date: 2021-05-30
last_modified_at: 2021-05-30
---
### 도전 프로그래밍! 을 풀어보았다.

1. 길이가 10인 배열을 선언하고 총 10개의 정수를 입력받아서 홀수와 짝수를 구분지어 출력하는 프로그램

```c
#include<stdio.h>

void printOdd(int * arr, int len) {
	int i;
	printf("홀수 출력 : ");
	for(i=0; i<len; i++) {
		if( arr[i] % 2 != 0) 	
			printf("%d ", arr[i]);
	}
	printf("\n");
}
void printEven(int * arr, int len) {
	int i;
	printf("짝수 출력 : ");
	for(i=0; i<len; i++) {
		if( arr[i] % 2 == 0) 	
			printf("%d ", arr[i]);
	}
	printf("\n");
}
int main(void) {
	int arr[10];
	int i, len;
	

	len = sizeof(arr) / sizeof(int);
	printf("정수 10개 입력 : ");
	for(i=0; i<10; i++)
		scanf("%d", &arr[i]);
		
	printOdd(arr, len);
	printEven(arr, len);
	
	return 0;

}
```

2.  10진수 정수를 입력받아서 2진수로 바꾸는 프로그램

```c
#include<stdio.h>

int main(void) {
	int arr[50];
	int num, i=0, count=0, temp;
	

	printf("10진수 정수 하나 입력 : ");
	scanf("%d", &num);
	
	while(1) { 
		arr[i++] = num % 2; 
		if(num==1)
			break;
		num /= 2;
		count++;
	}
	for(i=0; i<=count/2; i++) {
		temp = arr[i];
		arr[i] = arr[count-i];
		arr[count-i] = temp;
	}
	for(i=0; i<=count; i++)
		printf("%d", arr[i]);

    return 0;
}
```

3.  길이가 10인 배열을 선언하고 총 10개의 정수를 입력받아서 짝수면 배열의 뒤에서부터, 홀수면 배열의 앞에서부터 채우는 프로그램

```c
#include<stdio.h>

int main(void) {
	int arr[10] = {0};
	int i, num, countEven = 9, countOdd = 0;
	

	for(i=0; i<10; i++) {
		scanf("%d", &num);
		if(num % 2 == 0)  //짝수면 뒤에서부터
			arr[countEven--] = num;
		else  //홀수면 앞에서 부터 
			arr[countOdd++] = num;
	}
	for(i=0; i<10; i++)
		printf("%d ", arr[i]);
    
    return 0;
}
```

4. 입력된 단어가 회문(Palindrome, 뒤집어도 똑같은 단어인거)인지 아닌지 확인하고 결과를 출력하는 프로그램

```c
#include<stdio.h>

int main(void) {
	char str[50];
	int i, len=0, isP=1;
	

	printf("단어를 입력하세요 : ");
	scanf("%s", str);
	
	len = strlen(str);
	for(i=0; i<=len/2; i++) {
		if(str[i] != str[(len-1)-i]) {
			isP = 0;
			break;
		}
	}
	if( isP == 1 ) 
		printf("%s는 회문입니다. ", str);
	else
		printf("%s는 회문이아닙니다.", str);
    return 0;
}
```

5. 버블정렬을 실행하는 함수를 호출하는 프로그램 (내림차순 정렬)

```c
#include<stdio.h>

void bubbleSort(int arr[], int len) {		
	int i, j, tmp;
	
	for(i=0; i<len-1 ; i++) {
		for(j=0; j<(len-1)-i; j++) {
			if(arr[j] < arr[j+1]) {
				tmp = arr[j];
				arr[j] = arr[j+1];
				arr[j+1] = tmp;
			}
		}
	}
}
int main(void) {
	int arr[7] = {0};
	int i=0, len;
	
	printf("정수 7개 입력 : \n", i+1);
	for(i=0; i<7; i++)
		scanf("%d", &arr[i]);
	
	len = sizeof(arr)/sizeof(int);
	bubbleSort(arr, len);
	
	for(i=0; i<len ; i++)
		printf("%d ", arr[i]);
		
	return 0;

}
```

### 2차원 배열

1. 가로 길이가 9 , 세로 길이가 3인 int 형 2차원 배열을 선언해 구구단 중 2단 3단 4단을 저장해라

```c
#include<stdio.h>

int main(void) {
	int arr[3][9] = {0};
	int i, j;
	

	for(i=0; i<3; i++) {
		for(j=0; j<9;j++)
			arr[i][j] = (i+2) * (j+1);
	}
	for(i=0; i<3; i++) {
		for(j=0; j<9;j++) 
			printf("%d ", arr[i][j]);
		printf("\n");
	}
	
	return 0;

}
	
```

2. 배열 a {1, 2, 3, 4},{ 5, 6, 7, 8} 배열 b {1, 5}, {2, 6}, {3, 7}, {4, 8} 를 초기화하는 프로그램

```c
#include<stdio.h>

int main(void) {
	int arr1[2][4] = {1,2,3,4,5,6,7,8};
	int arr2[4][2] = {0};
	int i, j;
	
	for(i=0; i<2; i++) {
		for(j=0; j<4; j++) {
			arr2[j][i] = arr1[i][j];
		}
	}
	printf("배열 1 :\n");
	for(i=0; i<2; i++) { //배열 1 출력  
		for(j=0; j<4; j++) {
			printf("%d ", arr1[i][j]);
		}
		printf("\n");
	}
	printf("\n");
	printf("배열 2 :\n");
	for(i=0; i<4; i++) { // 배열 2 출력 
		for(j=0; j<2; j++) {
			printf("%d ", arr2[i][j]);
		}
		printf("\n");
	}

	return 0;
}
```

3.  철희, 철수, 영희, 영수의 국영수 +국사 성적을 저장하는 2차원 배열을 만들고 과목별 총점, 개인 총점을 추가해라 

```c
#include<stdio.h>

int main(void) {
	int score[5][5] = {0};
	char * name[5] = {"철희", "철수", "영희", "영수", "과목별 총점"};
	char * sub[5] = {"국어", "영어", "수학", "국사", "총점"};
	int i, j, total;
	
	printf("점수를 입력해 주세요 : \n"); //점수 입력 
	for(i=0; i<4; i++) {
		for(j=0; j<4; j++) {
			printf("%s의 %s 점수 : ", name[i], sub[j]);
			scanf("%d", &score[i][j]);
		} 
		printf("\n");
	}
	//개인 총점 저장
	for(i=0; i<4; i++)  {
		total = 0;
		for(j=0; j<4; j++) 
			total += score[i][j];
		score[i][4] = total ;
	}
	// 과목별 총점 저장
	for(i=0; i<5; i++)  {
		total = 0;
		for(j=0; j<4; j++) 
			total += score[j][i];
		score[4][i] = total;
	}
	//출력 
	printf("\t");
	for(i=0; i<5; i++) 
		printf("%5s", sub[i]);
	printf("\n");
	for(i=0; i<5; i++) {
		printf("%13s", name[i]);
		for(j=0; j<5 ; j++) {
			printf("%3d ", score[i][j]);
		}
		printf("\n") ;
	}
    return 0;
}
```

4. 정수를 입력받아 배열에 저장하고 배열의 최대 최소값을 찾는 함수를 만들어라. maxPtr, minPtr에는 가장 큰값과 가장 작은 값이 저장된 배열요소의 주소값이 저장되어야한다.

```c
#include<stdio.h>
void maxAndMin(int * arr, int len, int ** maxp, int ** minp) {
	int * max, * min;
	int i;
	max = min = &arr[0]; // 포인터 max , min에 arr[0]의 주소를 저장 
	for(i=0; i<len; i++) {
		if(*max < arr[i])
			max = &arr[i]; // 포인터 max에 최대값이 있는 배열요소의 주소값을 저장 
		if(*min > arr[i])
			min = &arr[i];
	}
	*maxp = max; //maxp에저장된값은포인터변수maxPtr,maxPtr에최대값이있는배열요소의주소값을 저장 
	*minp = min;  
}
int main(void) {
	int arr[5] = {0};
	int * maxPtr;
	int * minPtr;
	int i, len;

	printf("정수 5개를 입력해주세요 : ");
	for( i=0; i<5 ; i++) 
		scanf("%d", &arr[i]);
		
	len = sizeof(arr)/sizeof(int);
	maxAndMin(arr, len, &maxPtr, &minPtr);
	
	printf("입력된 정수 \n");
	for(i=0; i<len; i++) 
		printf("%d ", arr[i]);
	printf("\n");
	printf("최소값 : %d 최대값 : %d \n", *minPtr, *maxPtr);
	
	return 0;
}
```

