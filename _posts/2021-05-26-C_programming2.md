빵 500원 새우깡 700원 콜라 400원 일때 모두 하나 이상 구매하면서 3500원을 남김없이 쓰는 경우의 수

```c
#include<stdio.h>

int main(void) {
	int a, b, c;
	int bread=0, shrimp=0, coke=0;
	

	for(a=1; a<8; a++) {
		for(b=1; b<6; b++) {
			for(c=1; c<9; c++) {
				if( 5*a + 7*b + 4*c == 35) {
					bread = a; 
					shrimp = b;
					coke = c;
					printf("빵 %d 새우깡 %d 콜라 %d \n", bread, shrimp, coke);
				}
			}
		}
	}
	return 0;
}
```



소수 10개 출력하기

```c
#include<stdio.h>

int main(void) {
	int i, j, count=0; //소수 10개 출력하기 
	

	for(i=2; i <100 ; i++) {
		for(j=i; j > 1; j--) { 
			if( j == 2) {// 나누어 떨어지는 수 가 없으면 
				count++;
				printf("%d ", i);
			} else if( i % j == 0 && i != j) //1과 자기자신을 제외한 약수가 있으면 반복문 탈출 
				break;
		}
		if(count >10) 
			break;
	} 
	return 0;
} 
```

처음 짠 코드

```c
#include<stdio.h>

int main(void) {
	int i, j, count=0; //소수 10개 출력하기 
	

	while(count <= 10) {
		for(i=2; i <100 ; i++) {
			for(j=i; j > 1; j--) { 
				if( j == 2) {// 나누어 떨어지는 수 가 없으면 
					count++;
					printf("%d ", i);
				} else if( i % j == 0 && i != j)//1과 자기자신을 제외한 약수가 있으면 반복문 탈출 
					break;
			}
		}
	} 

} 
```

문제점 : count 변수가 for 문 안에 있어서 i가 100이 될 때 까지 while문을 탈출하지 못한다. count 값을 따로 출력해보니 24가 되었다.  if문을 사용해서 count 값을 검사할 수 있는데 불필요하게 반복문을 사용했다. 



초를 입력받아 시, 분, 초 단위로 출력하는 프로그램

```c
#include<stdio.h>

void printTime(int time){
	int hr, min, sec;
	

	hr = time / 3600;
	min = (time % 3600) / 60;
	sec = (time % 3600) % 60;
	
	printf("%d 초는 %d 시간 %d 분 %d 초 입니다.\n", time, hr, min, sec);

}
int main(void) {
	int time;
	

	printf("시간을 초 단위로 입력하세요 : ");
	scanf("%d", &time);
	
	printTime(time);
	
	return 0;

}
```

2^k <= n이 성립하는 k의 최대값을 출력하는 프로그램

```c
#include<stdio.h> 

int main(void) {
	int n, k, pow;
	

	printf("정수를 입력하세요 : ");
	scanf("%d", &n);
	
	pow=1;
	for(k=0; k<n ; k++) { 
		if( pow < n && pow*2 <= n) 
			pow *=2;
		else
			break;
	}
	printf("2^k <= %d를 만족하는 k의 최대값은 %d\n", n, k);

    return 0;
}
```



2의 n승을 출력하는 프로그램을 재귀함수를 사용해 만들어보자

```c
#include<stdio.h>

int pow(int n) {
	if(n==0)
		return 1;
	else if( n==1 )
		return 2;
	else
		return pow(n-1) * 2;
}
int main(void) {
	int n;
	

	printf("정수를 입력하세요 : ");
	scanf("%d", &n);
	
	printf("2^%d = %d", n, pow(n));
	
	return 0;

}
```



배열을 사용해서 다섯개의 정수를 받고 최대값, 최소값, 총합을 출력해보자.

```c
#include<stdio.h>

int findMax(int * arr, int size) {
	int max = -99999, i;

	max = arr[0];
	for(i=1; i<size; i++) {
		if( max < arr[i] )
			max = arr[i];
		else
			continue;
	}
	return max;

}
int findMin(int * arr, int size) {
	int min = 99999, i;
	
	min = arr[0];
	for(i=1; i<size; i++) {
		if( min > arr[i] )
			min = arr[i];
		else
			continue;
	}
	return min;

}
int sum(int * arr, int size)  {
	int i, total;
	for(i=0; i<size; i++)
		total += arr[i];
	return total;
}
int main(void) {
	int arr[5];
	int i, len;
	
	for(i=0; i<5; i++) {
		printf("정수 입력 %d : ", i+1);
		scanf("%d", &arr[i]); // &arr[i] = arr+i
	}
	len = sizeof(arr)/sizeof(int); //배열의 요소의 개수
	for(i=0; i<5; i++) 
		printf("%d ", arr[i]);
	printf("중 최대값은 %d, 최소값은 %d , 총 합은 %d 입니다.", findMax(arr, len), findMin(arr, len), sum(arr, len));
	 
	return 0;

}
```

최대값, 최소값, 총합 한번에 for 문으로 구할 수 있다. 나는 바보다.

문자열 출력

```c
#include<stdio.h>

int main(void) {
	char str[] = "Good morning";
	int i =0;

	while(str[i] != '\0') { //문자열의 끝에 null문자가 자동으로 삽입된다.
		printf("%c", str[i]);
		i++;
	}
	printf("\n%s", str); // 배열의 이름은 포인터 상수다. 배열의 첫번째 요소의 주소값을 가지고있다.

    return 0;
}
```

단어를 입력받고 길이를 출력하는 프로그램

```c
#include<stdio.h>int main(void) {	char str[50];	int i;		printf("영어단어를 입력하세요 : ");	scanf("%s", str);		for(i=0; i<50; i++){		if( str[i] == '\0')			break;	}	printf("%s 의 길이는 %d", str, i);		return 0;}
```

단어를 입력받고 단어를 뒤집어보자

```c
#include<stdio.h>int main(void) {	char str[50], temp;	int i, j;		printf("영어단어를 입력하세요 : ");	scanf("%s", str);		for(i=0; i<50; i++){  // while( str[i] != 0) i++; 로 대체할 수 있다.		if( str[i] == '\0')			break;	}	i--; // for문을 탈출한 후 i는 null문자의 인덱스 i--는 마지막 문자의 인덱스		for(j=0; j<i/2;j++) {		temp = str[j];		str[j] = str[i-j];		str[i-j]= temp;	} 	printf("%s ", str);		return 0;}
```

단어를 입력받고 아스키 코드값이 가장 큰 문자를 출력하는 프로그램

```c
#include<stdio.h>int main(void) {	char str[50], temp;	int i=0, j, max=0;		printf("영어단어를 입력하세요 : ");	scanf("%s", str);		while( str[i] != '\0') 		i++;		for(j=0; j<i; j++) {		if(max < str[j])			max = str[j];	}	 	printf("%d", i);	printf("%s 중 아스키 코드값이 가장 큰 단어는 %c\n", str, max);		return 0;}
```

정수형 변수 두개 선언하고 포인터로 가리키고 값을 수정하고 포인터가 가리키는 값을 서로 바꾸고 출력하기

```c
#include<stdio.h>int main(void) {	int num1 = 10, num2 = 20, tmp =0 ;	int *ptr1 = &num1, *ptr2 = &num2;		*ptr1 += 10; // num1 += 10;	*ptr2 -= 10; // num2 -= 10;	    // ptr1 = &num2, ptr2 = &num1;	tmp = ptr1; 	ptr1 = ptr2;	ptr2 = tmp;		printf("ptr1 : %d num1 : %d\nptr2 : %d num2 : %d", *ptr1, num1, *ptr2, num2);		return 0;}
```

다음 코드의 출력 결과 예상

```c
#include<stdio.h>int main(void) {	int num = 10;	int * ptr1 = &num;	int * ptr2 = ptr1;		(*ptr1)++;	(*ptr2)++;	printf("%d\n", num);	return 0;}
```

내 생각 : ptr2 가 ptr1을 가리킨다고 생각했다. (*ptr2)++; 의 실행결과로 ptr1이 가리키는 주소가 변경될거라고 생각했다.  최종적으로 num의 값은 11이 될 줄 알았다.

결과 :  ptr2 = ptr1 의 실행결과로 ptr2 에 ptr1의 주소가 저장된다.  그래서 *ptr1, *ptr2 둘은 모두 num의 값과 동일하다. 최종적으로 num의 값은 12가 되었다.

포인터가 무언가를 가리킨다고 생각하지말고 주소값을 저장한다고 생각하는게 나을 것 같다. 포인터 변수에는 주소값이 저장된다는것을 기억해야겠다.



길이가 5인 배열를 1, 2, 3, 4, 5로 초기화하고 배열의 첫번째 요소를 가리키는 포인터 변수 ptr을 선언한다. 그 다음 포인터 변수 ptr에 저장된 값을 증가시키는 방식으로 배열 요소에 접근하면서 모든 요소에 2를 더한 뒤 정상적으로 증가가 이루어졌는지 확인해라.

```c
#include<stdio.h>int main(void) {	int arr[5] = {1, 2, 3, 4, 5};	int * ptr = arr; // ptr = &arr[0];	int i=0;		for(i=0; i < 5 ; i++) {		*(ptr+i) += 2; // *(ptr++) += 2;	}	for(i=0; i < 5 ; i++) {		printf("%d ", arr[i]); //arr[i] = *(ptr+i)	}		return 0;}
```

길이가 6인 int형 배열을 선언하고 1, 2, 3, 4, 5, 6으로 초기화한 다음 배열에 저장된 값이 6, 5, 4, 3, 2, 1이 되도록 변경하는 프로그램을 만들어보자. 배열의 앞 뒤를 가리키는 포인터 변수 두개를 선언하고 이를 활용해 저장된 값의 순서를 바꿔야한다.

```c
#include<stdio.h>int main(void) {	int arr[6] = {1, 2, 3, 4, 5, 6};	int * ptr1 = &arr[5], *ptr2 = &arr[0];	int i, tmp;		for(i=0; i<3; i++) {		tmp = *(ptr1-i);		*(ptr1-i) = *(ptr2+i);		*(ptr2+i) = tmp;	}	for(i=0;i<6;i++)		printf("%d ", arr[i]);		return 0;}
```

call by value , call by reference

```c
#include<stdio.h>int squareByValue(int num) {	return num * num;}void squareByReference(int * ptr) {	*ptr *= *ptr;}int main(void) {	int num = 2, rbv;	rbv = squareByValue(num);	squareByReference(&num);		printf("num : %d cbv : %d cbr : %d\n", num, rbv);		return 0;}
```

