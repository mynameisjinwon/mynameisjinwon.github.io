---
title:  "열혈 C 프로그래밍 문제5"
excerpt: "오늘은 문자열 관련 함수들에 대해 공부를 해 보았다. "

categories:
  - C
tags:
  - C
 
date: 2021-06-02
last_modified_at: 2021-06-02
---

###### 1. 사용자로부터 알파벳 문자를 입력받아 대문자면 소문자로 소문자면 대문자로 변환하는 프로그램

```c
#include<stdio.h>

int main(void) {
	unsigned char ch1; //A = 65 Z = 90, a= 97, z =122

	puts("알파벳을 입력해주세요: ");
	ch1 = getchar();
	
	if( 65 <= ch1 && ch1 <= 90 ) {
		ch1 += 32;
	} else if (97 <= ch1 && ch1 <= 122) {
		ch1 -= 32;
	} else {
		printf("잘못 입력했습니다.");
		return -1;
	}
	putchar(ch1) ;
	return 0;
}
```

##### 문자 단위 입출력함수 

putchar() : 인자로 전달받은 문자를 표준 출력 스트림으로 전송한다.

fputc() : 인자로 전달받은 문자를  지정한 스트림으로 전송한다.

putchar(), fputc()함수는 인자로 전달받은 문자의 ASCII코드값을 반환한다. 파일끝에 도달하거나 함수 호출 실패시 EOF를 반환한다. 

getchar() : 표준 입력 스트림으로 부터 문자를 입력받는다.

fgetc() : 지정한 스트림으로 부터 문자를 입력받는다. 

getchar(), fgetc() 는 모두 입력된 문자의 ASCII코드 값을 반환한다. 함수 호출 실패시 EOF를 반환한다.

위 네 함수는 char형이 아닌 int형을 반환값으로 가지는데 그이유는 char형은 일부 컴파일러에서 unsigned char형으로 간주하는데 이럴 경우에 -1로 정의된 상수 EOF값을 정확하게 반환할 수 없다. 

##### 문자열 입출력 함수 

puts() : 인자로 전달받은 문자열을 표준 출력 스트림으로 전송한다. 문자열을 출력하고 자동으로 개행이 이뤄진다.

fputs() : 인자로 전달받은 문자열을 지정한 스트림으로 전송한다. 문자열을 출력하고 개행이 이뤄지지않는다.

함수 호출에 성공하면 0을 반환하고 그렇지 않으면  EOF를 반환한다.

gets() : 표준 입력 스트림으로 부터 문자열을 입력받는다. 입력하는 문자열이 배열의 길이보다 길경우에 할당받지 않은 메모리 공간을 침범해 오류가 발생할 수 있기 때문에 fgets()함수를 쓰는것이 좋다.

fgets() : 지정한 스트림으로 부터 지정된 길이만큼 문자열을 입력받는다.

문자열을 입력받으면 자동으로 NULL 문자가 문자열 끝에 삽입된다.

```c
#include<stdio.h>

int main(void) {
    char str[7];
    int i;
    
    for(i=0;i<3;i++){
        fgets(str, sizeof(str), stdin);
        printf("Read %d : %s \n", i+1, str);
    }
    return 0;
}
```

위 프로그램을 실행하고 12345678901234567890을 입력하면 입력한 문자열은 입력버퍼에 저장되고 fgets함수에 의해 sizeof(str) -1(6)의 길이 만큼 str에 저장된다. 문자열 끝에 자동으로 NULL문자가 삽입되기 때문에 (배열의 길이 -1)개의 문자만 저장된다. 

fgets함수는 '\n'을 만날때까지 문자열을 입력받는데 '\n'을 제외하지 않고 문자열에 저장한다.

gets 함수와 fgets함수는 scanf함수와 달리 공백이 있는 문자열도 입력 받을 수 있다.



**표준 입출력 함수를 통해 데이터를 입출력 하는 경우 운영체제가 제공하는 메모리 버퍼를 중간에 통과한다.**

메모리 버퍼를 사용하는 이유 : 키보드나 모니터와 같은 외부장치와의 데이터 입출력은 생각보다 시간이 걸리는 작업이다.  버퍼링 없이 키보드가 눌릴 때마다 문자 정보를 목적지로 바로 이동시키는 것보다 중간에 메모리 버퍼를 두고 여러 데이터를 한번에 이동시키는 것이 더 빠르고 효율적이기 때문에 입출력 버퍼를 사용한다. 

출력버퍼를 비우는 것 : 버퍼에 저장된 데이터를 목적지로 이동시키는 것

입력버퍼를 비우는 것 : 버퍼에 저장된 데이터가 소멸되는 것 // gets 등의 함수를 사용해 문자열을 읽어들이면 읽어 들인 데이터만큼 입력버퍼의 내용이 사라진다. 

###### 2. 적당한 길이의 문자열을 입력받아 그안에 존재하는 숫자의 합을 계산해서 출력하는 프로그램 (ex 입력 A15#46 이면 1 + 5 +  4 + 6 의 연산결과가 출력되야한다.)

```c
#include<stdio.h>
#include<string.h>

int pow(int n) { //10의 N제곱을 구하는 함수
	if(n==0) 
		return 1;
	return 10 * pow(n-1);
}
void sepNum(int num[], int * sum, int end) { //배열에 저장된 정수들을 더하는 함수
	int count = 0, temp,i,j, n=0;
	
	for(i=0;i<end;i++) {
		temp = num[i];
		while( temp  != 0 ) { // temp가 한자리수 정수면 count = 1, 두자리수면 2, 세자리수면 3
            temp /= 10;
			count++;
		}
		for(j=1; j<=count; j++) {
			n = pow(j);
			*sum += (num[i] % n - num[i] % (n/10)) / (n/10); 
		}
	}
}
int main(void) {
	char str[20];
	int num[20];
	int i, index=0, sum=0;
	printf("문자열 입력 : ");
	scanf("%s", str);
	
	for(i=0;i<strlen(str);i++) {
		if( (atoi(&str[i]) != 0 && atoi(&str[i-1]) != 0) || atoi(&str[i]) == 0)
			continue;
		num[index++] = atoi(&str[i]);
	}
	printf("문자열에 포함된 정수는 : ");
	for(i=0;i<index;i++) 
		printf("%3d ", num[i]);
	printf("\n");
	sepNum(num, &sum, index);
	printf("정수들의 합은 : %d\n", sum);
				
	return 0;
}
```
답지를 보았다. 너무 충격적이다. 답지는 2초 내가 짠 코드는 4초나 걸렸다. 시간을 안재봐도 내가 짠 코드가 더 느리다는 것을 알 수 있다.
너무 허무하다 이렇게 간단한 방법이 있는데 나는 왜 이 생각을 못했을까. 그래도 화는 안난다. 
```c
#include<stdio.h>
#include<string.h>

int ConvToInt(char c) {
	static int diff = 1 - '1';
	return c + diff;
}
int main(void) {
	char str[50];
	int len, i;
	int sum=0;
	
	printf("문자열 입력 : ");
	fgets(str, sizeof(str), stdin);
	len=strlen(str);
	for(i=0;i<len;i++) {
		if('1'<=str[i] && str[i]<='9')
			sum += ConvToInt(str[i]);
	}
	printf("숫자의 총합 :%d\n", sum);
	
	return 0;
}
```

###### 3. str1과 str2에는 프로그램을 통해 입력받은 문자열을 저장하고 str3에는 str1을 복사한뒤 str2의 내용을 뒤에 덧붙이는 프로그램

```c
#include<stdio.h>
#include<string.h>

int main(void) {
	char str1[20];
	char str2[20];
	char str3[40];
	int i;
	
	fputs("문자열을 입력해주세요 1 : ", stdout);
	fgets(str1, sizeof(str1), stdin);
	fputs("문자열을 입력해주세요 2 : ", stdout);
	fgets(str2, sizeof(str2), stdin);
	
	strncpy(str3, str1, strlen(str1)-1);
	strncat(str3, str2, strlen(str2)-1);
	
	puts(str3);
		
	return 0;	
}
```

###### 4. 프로그램 사용자로부터 이름과 나이를 다음의 형식에 맞춰 하나의 문자열로 입력받는다. 

"이정선 29"

"한수정 7"

"오선주 17 "

```c
#include<stdio.h>
#include<string.h>
int findName(char str[]){ //공백의 위치를 반환하는 함수  
	int i =0;
	while(str[i] != ' ') 
		i++;
	return i;
}

int main(void){
	char str[50];
	int len, i, nameIndex1, nameIndex2;
	
	fputs("이름과 나이를 예시와 같이 입력해주세요 : (홍길동 25) ", stdout);
	fgets(str, sizeof(str), stdin);
	len = strlen(str)-1; 
	fputs("이름과 나이를 예시와 같이 입력해주세요 : (홍길동 25) ", stdout);
	fgets(&str[len], sizeof(str), stdin);
	
	//이름만 출력
	nameIndex1 = findName(str); // 문자열에 있는  공백의 위치 
	for(i=0; i<nameIndex1;i++)
		putchar(str[i]);
	printf(" ");
	
	nameIndex2 = findName(&str[len]) + len; // 문자열에 있는  공백의 위치 
	for(i=len; i<nameIndex2;i++)
		putchar(str[i]);
	printf(" 두 사람은 ");
	 
	//이름 비교 
	if(strncmp(str, &str[len], nameIndex1) != 0) {
		
		fputs("이름이 다릅니다. ", stdout);
		
	} else
		fputs("이름이 같습니다. ", stdout);
	fputs("그리고 ", stdout);
	//나이 비교
	if(strncmp(&str[nameIndex1+1], &str[nameIndex2+1], 2 )!= 0) 
		puts(" 나이가 다릅니다. ");
	else 
		puts(" 나이가 같습니다.");
	
	puts(str);
	return 0;
}
```

