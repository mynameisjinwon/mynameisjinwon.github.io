---
title:  "열혈 C 프로그래밍 문제7"
excerpt: "오늘은 파일 입출력에 대해서 공부를 해 보았다. "

categories:
  - C
tags:
  - C
 
date: 2021-06-04
last_modified_at: 2021-06-04
---
### 파일 입출력

##### 파일쓰기

```c
#include<stdio.h>

int main(void) {
	FILE *fp = fopen("data.txt", "wt");
	if(fp == NULL)  {//fopen함수 스트림 형성 성공시 FILE구조체 변수의 주소값, 실패시 NULL포인터 반환 
		printf("파일열기 실패!\n");
		return -1; // 비정상 종료를 알리기위해 0이 아닌 값을 리턴 
	}
	fputc('a', fp);
	fputc('b', fp);
	fputs("hello\n", fp);
	fputs("world\n", fp);
	
	fclose(fp); // 자원낭비를 막기위해 파일사용 후 닫아준다. 
	return 0;
}
```

fopen() : 스트림 형성을 위한 함수,  출력 스트림을 형성하고 FILE구조체 변수의 주소값을 리턴한다.

fclose() : 스트림 소멸을 위한 함수, 파일 사용후 fclose함수를 통해 할당받은 자원을 반환한다.

프로그램 실행결과 data.txt파일이 생성되고 파일의 내용은 아래와 같다.

abhello

world

##### 파일 읽기

```c
#include<stdio.h>

int main(void) {
	char str[20];
	int ch;
	FILE *fp = fopen("data.txt", "rt");
	if(fp == NULL)  {//fopen함수 스트림 형성 성공시 FILE구조체 변수의 주소값, 실패시 NULL포인터 반환 
		printf("파일열기 실패!\n");
		return -1; // 비정상 종료를 알리기위해 0이 아닌 값을 리턴 
	}
	ch = fgetc(fp);
	printf("%c \n", ch);
	ch = fgetc(fp);
	printf("%c \n", ch);
	fgets(str, sizeof(str), fp);
	printf("%s", str);
	fgets(str, sizeof(str), fp);
	printf("%s", str);
	
	fclose(fp); // 자원낭비를 막기위해 파일사용 후 닫아준다. 
	return 0;
}
```

fgetc 함수는 FILE구조체 포인터 변수가 가리키는 파일의 문자를 읽어들인다. 

fgets 함수는 '\n'문자로 문자열을 구분한다.

파일에 저장되는 문자열에는 널문자가 추가되지않는다.  '개행문자로 문자열을 구분한다.

운영체제마다 개행문자를 표현하는 방식이 다르다. 개행문자가 포함된 문자열을 파일에 저장할 때 주의가 필요하다. 자동으로 운영체제에 맞는 개행문자 표현으로 변환된다. 

텍스트 모드로 파일열기 : rt, wt, at, rt+, wt+, at+

w, a, wt, at, wb, ab는 파일이 없으면 생성하지만 읽기모드는 에러가 발생한다.

##### 파일 복사하기

```c
#include<stdio.h>

int main(void) {
	FILE * des = fopen("des.txt", "wt");
	FILE * src = fopen("src.txt", "rt");
	char str[20];
	
	if(des==NULL||src==NULL) {
		printf("파일 열기 실패!");
		return -1;
	}
	while(fgets(str, sizeof(str), src) != NULL) //fgets 파일끝도달 or 함수호출 실패시 NULL반환
		fputs(str, des);
	if(feof(src)!=0) //파일끝에 도달했으면 
		printf("파일 복사 성공!");
	else			//파일끝에 도달하지 못했으면 fgets함수 호출 실패한거임
		printf("파일 복사 실패!");
	fclose(des);
	fclose(src);
	
	return 0;
}
```

feof() : 파일의 끝에 도달하면 0이 아닌 값을 반환한다.

##### 바이너리 파일에대한 입출력

```c
#include<stdio.h>

int main(void) {
	FILE * src = fopen("src.bin", "rb");
	FILE * des = fopen("des.bin", "wb");
	char buf[100];
	int readCnt;
	
	if(src==NULL||des==NULL) {
		printf("파일열기실패!");
		return -1;
	}
	while(1) {
		readCnt = fread((void*)buf, 1, sizeof(buf), src);
		if(readCnt < sizeof(buf)) {// 읽어들인 데이터가 버퍼의 크기보다 작으면 
			if(feof(src) != 0) { //src가 파일 끝에 도달했으면 
				fwrite((void*)buf, 1, readCnt, des); //읽어들인 데이터의 수만큼을 버퍼에서 읽어와 des에 저장한다.
				break;
			} 
			else //src가 파일 끝에 도달하지 못했는데 읽어들인 데이터가 지정한 크기보다 작다면 함수호출에 오류가 발생한것
				puts("파일 복사 실패");
			break;
		}
		//버퍼의 크기만큼 데이터를 읽어들였으면  
		fwrite((void*)buf, 1, sizeof(buf), des); //버퍼의 모든데이터를 des에 저장 
	}
		
	fclose(src);
	fclose(des);
	return 0;
}
```

fwrite, fread 함수로 바이너리 파일에 데이터를 쓰고 읽을수 있다.

포인터 형이 필요한 이유는 포인터 변수에는 포인터가 가리키는 변수의 시작 주소값만 저장되어있어 시작 주소로 부터 몇 바이트만큼 데이터를 읽을지를 결정하기 위해서이다. fwrite, fread함수에서는 인자로 오는 변수의 포인터 형을 한정하지 않으면서 다양한 변수를 인자로 보낼 수 있게하기 위해서 보이드형 포인터를 인자로 받는다고 생각한다.(ㅋ;) 그리고 함수의 인자로 읽어들일 데이터의 크기와 개수를 지정하기 때문에 포인터 형이 필요없다고 생각한다.

void형 포인터 변수를 사용하는 이유를 구글에서 찾아보았다 그 이유는 모든 자료형의 변수의 주소값을 받을 수 있게 하기 위해서이다.

##### src.bin파일을 복사한 des.bin파일의 내용을 출력하는 프로그램

```c
#include<stdio.h>

int main(void) {
	FILE * fp = fopen("des.bin", "rb");
	int i;
	if(fp==NULL) {
		printf("파일 읽기 실패");
		return -1; 
	}
	for(i=0;i<100;i++) 
		printf("%d", fgetc(fp));
		
	fclose(fp);
	return 0;
}
```

printf() 함수가 아니라 putchar함수를 통해서 출력을 시도해보았는데 숫자가 아닌 문자가 출력되었다. 

putchar는 문자를 출력하는 함수여서 그런것같다. 문자열관련 함수라는 것을 잊지말자.



###### 1. 프로그램상에서 mystory.txt라는 이름의 파일을 생성해 본인의 이름, 주민번호, 전화번호를 저장하는 프로그램을 작성하자.

```c
#include<stdio.h>

int main(void) {
	FILE * fp = fopen("mystory.txt", "wt"); // wt모드는 파일이 없으면 생성한다.
	if(fp == NULL) { //fopen 함수 호출 실패시 NULL포인터를 반환한다.
		printf("파일 열기 실패!");
		return -1;
	}
	fputs("#이름 : 한진원\n", fp); //fputs 함수는 자동으로 개행되지않는다.
	fputs("#주민번호 : 251334-7788899\n", fp);
	fputs("#전화번호 : 010-20000-12222\n", fp);
	
	fclose(fp);
	return 0;
}
```

###### 2. 문제1에서 작성한 파일에 데이터를 추가하자. 추가할 데이터는 즐겨 먹는 음식의 정보와 취미이다.

```c
#include<stdio.h>

int main(void) {
	FILE * fp = fopen("mystory.txt", "at"); //내용을 추가하기위해 at모드로 파일을 열었다.
	if(fp == NULL) {
		printf("파일 열기 실패!");
		return -1;
	}
	fputs("#즐겨먹는 음식 : 돈까스\n", fp); //돈까스
	fputs("#취미 : 독서\n", fp);
	
	fclose(fp);
	return 0;
}
```

###### 3.문제 1과 2에서 생성한 파일에 저장된 정보 전체를 출력하는 프로그램을 작성하자.

```c
#include<stdio.h>

int main(void)  {
	FILE * fp = fopen("mystory.txt", "rt");
	char str[20];
	if(fp==NULL) {
		printf("파일열기실패!");
		return -1;
	}
	while(fgets(str, sizeof(str), fp)!=NULL) { //fgets함수는 파일끝에 도달하면 NULL포인터를 반환
		fputs(str, stdout);
	}
	fclose(fp);
	return 0;
}
```

##### 서식에 따른 데이터 입출력 fprintf, fscanf

```c
#include<stdio.h>

int main(void) {
	char name[10];
	char sex;
	int age, i;
	FILE * fp = fopen("friend.txt", "wt");
	if(fp==NULL) {
		printf("파일열기실패!");
		return -1;
	}
	for(i=0;i<3;i++) {
		printf("이름 성별 나이 순서로 입력 : ");
		scanf("%s %c %d", name, &sex, &age);
        //getchar(); 버퍼에 남아있는 '\n'소멸
		fprintf(fp, "%s %c %d", name, sex, age);
	}
	fclose(fp);
	return 0;
}
```

fprintf함수를 통해서 파일을 대상으로 텍스트 데이터와 바이너리 데이터를 출력할 수 있다.

15줄에 있는 getchar()함수가  처음에는 필요없다고 생각했다. 어차피 scanf함수는 개행문자를 입력받지 않으니까 문제 없다고 생각했는데 조금 더 생각해보니 scnaf함수는 공백으로 데이터를 구분하기 때문에 개행문자가 남아있는 입력버퍼를 비울 필요가 있다는 생각이 들었다. 

하지만 실행결과는 getchar함수가 있던 없던 똑같았다. 구글링 결과 white space로 해결할 수 있다는 정보를 얻었다. " %c"처럼 %c 문자앞에 공백을 추가하면 white space는 무시하고 문자만 입력받는다. 

14줄의 코드를 scanf("%s%c%d", name, &sex, &age);로 수정하고 실행해보았더니 두번째 입력을 건너뛰고 세번째 입력을 받는 오류가 나타났다. 그런데 여기서 밑에 getchar함수를 추가했더니 두번째 입력은 받는데 세번째 입력을 건너 뛰게 된다. 그리고 파일에는 의도하지 않은 문자가 저장이 된다. fflush(stdin)을 사용했더니 입력은 받을 수 있지만 파일에는 의도하지 않은 데이터가 저장되었다. 

파일에 출력하는 코드를 수정해보았다. fprintf(fp, "%s \n%c \n%d\n\n", name, sex, age); 그랬더니

```
한진원 

4203769

한진원 

4203769

한진원 

4203769
```

이런 결과를 얻을 수 있었다. 변수 sex에는 데이터가 저장되지 않았다. 입력값은 "한진원 m 22인데"  m은 어디로 사라진걸까 'm'의 아스키코드값은 109이다. 이제 알았다 입력을 받을때 서식문자를 공백 없이 붙여놓으면 안되나보다. 4203769는 초기화하지않아서 변수 age에 저장된 쓰레기값이다.  파일에 저장하기 전에 표준출력으로 입력받은 값을 출력하는 코드를 추가해서 확인했다. 

입력을 받는 순서를 수정했더니 scanf("%c %s %d",  &sex,  name, &age); 입력버퍼를 비우는 코드없이는 의도하지않은 값이 저장되는 문제가 발생했다.  왜 처음 코드에서는 이런 문제가 일어나지 않았을까?

##### 구조체 변수의 입출력

```c
#include<stdio.h>

typedef struct fren {
	char name[10];
	char sex;
	int age;
}Friend;
int main(void) {
	FILE * fp;
	Friend myfren1;
	Friend myfren2;
	
	fp=fopen("Friend.bin", "wb");
	printf("이름, 성별, 나이 순 입력 : ");
	scanf("%s %c %d", myfren1.name, &myfren1.sex, &myfren1.age);
	fwrite((void*)&myfren1, sizeof(myfren1), 1, fp);
	fclose(fp);
	
	fp=fopen("Friend.bin", "rb");
	fread((void*)&myfren2, sizeof(myfren2), 1, fp);
	printf("%s %c %d\n", myfren2.name, myfren2.sex, myfren2.age);
	fclose(fp);
	return 0;
}
```

구조체 변수를 하나의 바이너리 데이터로 인식하고 처리한다.  fwrite, fread함수가 void 형 포인터 변수를 인자로 받기 때문에 구조체 Friend 타입의 주소값도 함수의 인자로 전달할 수 있다. 

##### 파일위치지시자의 위치를 이동시키는 fseek, 현재 위치를 나타내는 ftell

FILE 구조체에는 파일의 어느 위치를 읽고있는지에 대한 정보를 담고있는 멤버 변수가 있다. 이를 파일 위치 지시자라고 한다.

fseek 함수를 사용하면 파일 위치 지시자를 이동시킬 수 있어 파일의 임의의 위치에 접근이 가능하다. 바이트 단위로 이동한다.

```c
#include<stdio.h>

int main(void) {
	char num;
	FILE * fp = fopen("text.txt", "wt");
	fputs("123456789", fp);
	fclose(fp);
	
	fp=fopen("text.txt", "rt");
	
	fseek(fp, -2, SEEK_END); //파일의 끝(마지막 바이트X)에서 부터 2바이트만큼 시작방향으로 이동 
	putchar(fgetc(fp));
	printf("\n%d\n", ftell(fp));
	
	fseek(fp, 2, SEEK_SET); //파일의 시작부분부터 2바이트 만큼 파일의 끝 방향으로 이동
	putchar(fgetc(fp));
	printf("\n%d\n", ftell(fp));
	
	fseek(fp, -2, SEEK_CUR); //현재 지시자의 위치부터 2바이트 만큼 파일의 시작 방향으로 이동
	putchar(fgetc(fp));
	printf("\n%d\n", ftell(fp));
	close(fp);
	return 0;
}
```

ftell함수는 파일 위치 지시자의 위치 정보를 반환한다.

현재 파일 위치 지시자가 첫번째 바이트를 가리키는 경우 0을 반환하고 두번째 바이트를 가리키는 경우 1을 반환한다.

n번째 바이트를 가리키는 경우 n-1을 반환한다.



###### 4.  FILE 구조체의 포인터가 인자로 전달되면, 파일의 크기를 바이트 단위로 계산하여 반환하는 함수를 정의하자!

```c
#include<stdio.h>
int fileLen(FILE * fp) {
	fseek(fp, -1, SEEK_END); 
	return ftell(fp) + 1;// 파일지시자가 n번째 바이트를 가리키면 ftell은 n-1을 반환한다.
}
int main(void) {
	FILE * fp = fopen("src.txt", "rt");
	int len, txt;
	if(fp==NULL){
		puts("파일 열기 실패!");
		return -1;
	}
	puts("파일의 내용 : ");
	while((txt=fgetc(fp))!=EOF) 
		putchar(txt);
	puts("");
	len = fileLen(fp);
	printf("파일의 길이 : %d", len);
	fclose(fp);
	
	return 0;
}
```


