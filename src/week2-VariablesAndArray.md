## 프리미티브 타입, 변수 그리고 배열을 사용하는 방법

### 프리미티브 타입 종류와 값의 범위 그리고 기본 값

#### 정수타입

- 소수점 없는 숫자, 음수도 허용

int : 4bytes. –2,147,483,648 ~ 2,147,483,647(약 21억)

long : 8bytes. –9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 (약 900경)

short : 2bytes. –32,768 ~ 32,767

byte : 1byte. –128 ~ 127

long 정수 리터럴 : 접미어 L을 붙여 작성한다.

ex ) long test = 40000000L

#### 부동 소수점타입

float : 4bytes. +-3.40282347E+38F(유효자릿수 6~7)

double : 8bytes. +-1.79769313486231570E+308(유효자릿수 15)

부동소수점 수가 많을 때는 float, 소수점 자리가 많을 때는 double을 사용한다.

#### char타입

2 byte

UTF-16 문자 인코딩의 ‘코드유닛(코드 단위)’, 작은 따옴표로 감싼다.

ex) char a = ‘J’;

#### boolean 타입

1 byte

true, false 두 가지 값을 가진다.(정수 0,1과는 아무 관련이 없다.)

### 프리미티브 타입과 레퍼런스 타입

#### 프리미티브 타입

- 계산을 위한 실제 값이 저장되는 타입의 변수

- 주솟값을 가지지 않는다.

- null 값이 될 수 없다.

#### 레퍼런스 타입

- 객체의 주소를 저장

- null 값이 될 수 있다.

### 리터럴



### 변수 선언 및 초기화하는 방법
### 변수의 스코프와 라이프타임
### 타입 변환, 캐스팅 그리고 타입 프로모션
### 1차 및 2차 배열 선언하기
### 타입 추론, var