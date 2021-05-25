---
title:  "열혈 C 프로그래밍 문제"
excerpt: "열혈 C 프로그래밍 책에 있는 프로그래밍 문제를 풀어보았다. "
 
date: 2021-05-25
last_modified_at: 2021-05-25
---
열혈 C 프로그래밍 문제

7-2 while문

1. 프로그램 사용자로부터 총 5개의 정수를 입력 받아서, 그 수의 합을 출력하는 프로그램을 작성해보자. 단! 한가지 조건이 있다. 정수는 반드시 1이상이어야 한다. 만약에 1미만의 수가 입력되는 경우에는, 이를 입력으로 인정하지 않고 재 입력을 요구해야 한다. 그래서 결국 1이상의 정수 5개를 모두 입력받을 수 있도록 프로그램을 완성해야 한다.

소스 코드 :


	

```c
#include<stdio.h>

int main(void) {
    int num, sum, i=0;
	
    while(i < 5) {
        printf("1 보다 작은 정수를 입력하세요 : %d", i);
		scanf("%d", &num);
		while(num < 1 ) {
            printf("%d는 1보다 작습니다. 다시 입력하세요 : ", num);
            scanf("%d", &num);
		}
		sum += num;
		i++;
	}
	printf("%d", sum);
    return 0;
}
```

2. 아래의 출력을 보이는 프로그램을 작성해보자.

   *

   o*

   oo*

   ooo*

   oooo*

   참고로, 총 5행에 걸쳐서 출력이 이뤄지고, 행이 더해질 때마다 o 문자의 수가 증가한다는 특징을 기반으로 while문의 중첩을 구성하면 된다.

   
   	

   ```c
   #include<stdio.h>
   
   int main(void) {
       int i=0, j;
       while(i < 5) {
           j=0;
           while(j < i) {
               printf("o");
               j++;
           }
           printf("*");
           printf("\n");
           i++;
       }
       
       return 0;
   }
   ```

   

if ~ else 대신 조건 연산자 사용하기

```c
#include<stdio.h>

int main(void) {
	int kor, eng, math;
	double mean;
	char score;
    
    printf("국 영 수 : ");
    scanf("%d %d %d", &kor, &eng, &math);
    mean = (double)(kor + eng + math) / 3;

    score = mean >= 90 ? 'A' : mean >= 80 ? 'B' : mean >= 70 ? 'C' : mean >= 50 ? 'D' : 'F';
    printf("%f %c", mean, score);
    
    return 0;
}
```



if ~ else 대신 switch문 사용하기

```c
#include<stdio.h>

int main(void) {
	int a;

    scanf("%d", &a);

    switch(a/10) { 
        case 0 :
            printf("0 이상 10 미만");
            break;
        case 1 : 
            printf("10 이상 20 미만");
            break;
        case 2 :
            printf("20 이상 30 미만");
            break;
        default :
            printf("30 이상");
            break;a
	}
    return 0;
}
```



최소 , 최대값을 찾는 함수 

```c
#include<stdio.h>

int MaxFinder(int a, int b, int c) {
	int max = 0;
	
    if( a >= b && a >= c) {
        max = a; 
    } else if ( a >= b && a < c ) {
        max = c;
    } else if ( a < b && b >= c) {
        max = b;
    } else
        max = c;
    return max;
}    
int MinFinder(int a, int b, int c) {
	int min = 99999;
	

	if( a <= b && a <= c) {
		min = a; 
	} else if ( a <= b && a > c ) {
		min = c;
	} else if ( a > b && b <= c) {
		min = b;
	} else
		min = c;
	return min;

}

int main(void) {
	int num1, num2, num3;
	

	scanf("%d %d %d", &num1, &num2, &num3);
	
	printf("%d %d %d 중 최대값은 %d \n", num1, num2 ,num3, MaxFinder(num1, num2, num3));
	printf("%d %d %d 중 최소값은 %d \n", num1, num2 ,num3, MinFinder(num1, num2, num3));
	
	return 0;

}
```



화씨는 섭씨로 섭씨는 화씨로 바꾸는 함수

```c
#include<stdio.h>

double CelToFah(int temp) {
	return temp * 1.82 + 32;
}
double FahToCel(int temp) {
	return (temp-32)/1.82;
}

int main(void) {
	char tp; 
	double temp;
	
    while(1) {
        printf("섭씨는 C 화씨는 F 입력: ");
        scanf("%c", &tp);
        if( tp == 'F' || tp == 'f' || tp == 'C' || tp == 'c');
            break;
    }
    printf("온도를 입력해주세요 : ");
    scanf("%lf", &temp);

    if( tp == 'f' || tp == 'F') {
        printf("화씨 %.2f도는 섭씨 %.2f도 입니다.", temp, FahToCel(temp));
    } else 
        printf("섭씨 %.2f도는 화씨 %.2f도 입니다.", temp, CelToFah(temp));

    return 0;
}    
```



피보나치 수열을 출력하는 함수



```c
#include<stdio.h>

int fbSeq(int n) {
	if( n== 1 ) 
		return 0;
	if( n == 2 ) 
		return 1;
	return fbSeq(n-1) + fbSeq(n-2);
}
int main(void) {
	int n=1, i=1;
	
	printf("종료하려면 0 입력 \n");
	
	while(n!=0) {
		scanf("%d", &n);
		for(i=1; i<=n; i++) {
			printf("%d ", fbSeq(i));
		}
		printf("\n");
	}
	
}
```

재귀함수 안쓰고 피보나치 수열 출력하기  잘 모르겠다 내일로 미루자



static 변수 사용

```c
#include<stdio.h>
int AddTotal(int num) {
	static int total= 0; //static 변수는 자동으로 0으로 초기화된다.
	total += num;
	return total; 
}

int main(void) {
	int num, i;
	for(i=0; i<3; i++) {
		printf("입력 %d ", i+1);
		scanf("%d", &num);
		printf("누적 : %d\n", AddTotal(num) );
	}
	return 0;
}
```

static 변수로 선언하면 프로그램 시작과 동시에 메모리 공간에 할당받고 프로그램 종료까지 남아있는다.

전역변수와 차이점 : 선언한 함수내에서만 접근이 가능하다.

공통점 : 초기화 하지 않으면 자동으로 0으로 초기화된다. 프로그램 종료 까지 메모리에 남아있는다.



최대 공약수를 찾는 프로그램

```c
#include<stdio.h>

int findGcd(int num1, int num2)	{
	int r; 
	if(num1 % num2 == 1)
		return 1; 
	else if( num1 % num2 ==0 )
		return num2;
	else {
		r = num1 % num2;
		findGcd( num2, r);
	}
}
int main(void) {
	int num1, num2, temp;
	

	printf("두 정수를 입력하세요 : ");
	scanf("%d %d", &num1, &num2);
	
	if(num1 <= num2 ) { // num1이 num2 보다 작으면 num1과 num2 바꿈 
		temp = num1;
		num1 = num2;
		num2 = temp;
	}
	printf("%d와 %d의 최대 공약수는 %d\n", num1, num2, findGcd(num1, num2));
	
	return 0;

}
```

유클리드 호제법 처음알았다. 신기하다.

유클리드 호제법 : 최대공약수를 찾는 알고리즘.  A와 B의 최대 공약수는 찾을 때 A를 B로 나눈 나머지 r 과 B의 최대공약수와 같다. 이를 통해서 B를 r 로 나눈 나머지 r' 을 찾고 다시 r을 r'으로 나눈 나머지를 구하는 과정을 반복해 나머지가 0일 때 나누는 수가 최대 공약수이다. 

