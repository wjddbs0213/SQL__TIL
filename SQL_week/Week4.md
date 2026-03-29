# SQL_BASIC 4주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_4th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**4주차 과제부터는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_4th

### 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-4. 오류를 잘 디버깅하는 방법



## 섹션 5. 데이터 탐색 - 변환

### 4-1. INTRO

### 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

### 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

### 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 3-4. 오류를 디버깅하는 방법

~~~
✅ 학습 목표 :
* 오류의 정의에 대해 설명할 수 있다. 
* 오류 메시지를 보고 디버깅이라는 과정을 수행할 수 있다. 
~~~

### [ Syntax 에러: 문법에러 ]

* 오류 메세지의 역할
  - 현재 작성한 방식으로하면 답을 얻을 수 없음: 길잡이역할
  - 문제진단 

### [대표적인 오류: BigQuert Error]

- 문법을 지키지 않아서 생기는 오류

   -> 오류 메세지 확인하기 
   -> 구글, 지피티 등 질문하면 좋음

#### 1. SELECT list must not be empty at [10:1]
- 밑줄쳐진 줄의 **앞/뒤**에 오류가 있을 확률이 큼

  -> 데이터 예시, 쿼리 및 오류메세지와 함께 오류 디버깅

#### 2. Number of arguments does not match for aggregate function COUNT

~~~
SELECT
 COUNT(id, kor)
  FROM ~~

 : 집계함수 COUNT의 인자수가 일치하지 않는 것 (인자는 하나만)
~~~

#### 4. SELECT list expression references column type1 a which is neither grouped nor aggregated

~~~
SELECT type1, COUNT(id) AS CNT
  FROM ~~
  **GROUP BY** type1 

  -> count 라는 집계 함수가 있고, 앞에 type1 이 있기에 "그룹화" 필요
~~~

- SELECT 목록 식은 다음에서 그룹화되거나 집계되지 않은 열을 참조합니다
  
  -> GROUP BY에 적절한 컬럼을 명시해야함

#### 5. Syntax error: Expected end of input but got keyword SELECT

~~~
SELECT
type1,COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY type1

  -> 💡 [8:1]:[줄:칸]
~~~ 

- 입력이 끝날 것으로 예상되었지만, SELECT 키워드가 입력되었습니다
   -> ; 으로 앞의 쿼리를 끝내줘야함

#### 6. Syntax error: Expected end of input but got keyword WHERE at [5:1]

~~~
SELECT *
FROM basic.trainer -- LIMIT10 --
WHERE id=3
~~~
- 입력이 끝날 것으로 예상되었지만, [5:1]에서 키워드 WHERE을 얻었습니다

  -> WHERE 절에 문제 O 
  
  -> LIMIT 은 맨 마지막에 입력해야함

#### 7. Syntax error: Expected")" but got end of script at [8:11]

- ")" 가 예상되지만, [8:11]에서 끝났습니다

  -> 괄호는 꼭 끝내줘야함


## 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

~~~
✅ 학습 목표 :
* 데이터 타입의 종류를 설명할 수 있다. 
* 데이터 타입을 변환하는 방법을 설명할 수 있다. 
~~~

*데이터를 활용하는 과정 중 **변환** part*

~~~
* SELECT 문에서 데이터를 변환 ⭕️
* WHERE의 조건문에도 사용 ⭕️

  -> 데이터 타입에 따라 다양한 함수가 존재
~~~

### [데이터 타입]
- 숫자 : 1, 2, 3.14 ...
- 문자 : "나", "데이터" ...
- 시간, 날짜: 2024-01-01, 2024-01-01 23:59:59 ... 
- 부울(Bool) : 참 / 거짓 

   -> 이 조건이 TRUE 일 때만 가져오겠다

   -> 혹은 TRUE(FALSE)인데 문자로 저장되어있을 수도 있음
- (ARRAY, JSON...)

**❓왜 데이터 타입을 알아야할까**
- 엑셀에서의 빈 값 => ""? NULL?
- 1 이라고 저장 => 숫자 1? 문자 1?
- 2023-12-31 => DATE? 문자?
- Bool에서 "TRUE" => 문자일수도 있음

  *데이터의 타입을 서로 변경해야함*

### [자료타입 변경하기]
- 자료 타입을 변경하는 함수: CAST

~~~ 
1️⃣ SELECT 
  CAST (1 AS STRING )

= 숫자 1 ➡️ 문자 1(STRING)

2️⃣ SELECT 
  CAST ("카일스쿨" AS INT64)
= 문자 ➡️ 숫자: 변환실패

SAFE_CAST("카일스쿨" AS INT64)
 = 변환 실패의 경우, "NULL" 반환
~~~

### [수학함수]

- 수학함수는 수학연산(평균, 표준편차, 코사인 등)

  -> 필요하면 검색해서 사용하기

❓ 나누기를 할 경우
- x/y -> **SAFE_DIVIDE** 함수 사용

  => 둘 중 하나라도 0이면, ZERO ERROR 발생


## 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

~~~
✅ 학습 목표 :
* 문자열 함수들의 종류를 이해하고 어떠한 상황에서 사용하는지 설명할 수 있다. 
~~~

### 문자열(STRING) 함수란?
- "안녕하세요" 

| 함수 | 실행 | 
|:---|:---:|
| SPLIT | 문자열 분리 | 
| CONCAT | 문자열 붙이기 | 
| REPLACE | 특정단어 수정하기 |
| TRIM | 문자열 버리기 |
| UPPER | 소문자 -> 대문자 |

### [문자열 붙이기]
💡 CONCAT ("안녕", "하세요")
  
  -> FROM 없어도 ⭕️

### [문자열 분리하기]
💡 SPLIT("가,나,다,라", ",")

  -> ','를 기준으로 문자를 나눔
  
  -> 띄어쓰기 까지 포함해서 **나눌기준** 표시

### [특정단어 수정하기]
💡 REPLACE ("안녕하세요", "안녕", "실천") 

  -> REPLACE (문자열 원본, 찾을단어, 바꿀단어)

### [문자열 자르기]
💡 TRIM ("안녕하세요", "하세요")

  -> TRIM (문자열 원본, 자를단어)


### [영어 소문자 -> 대문자]

💡 UPPER ("abc")

  -> UPPER (문자열 원본)

  -> 모두 대문자로 변경해서 같은지 체크 (Ash)

~~~
SELECT

CONCAT / SPLIT / REPLACE / TRIM / UPPER ()

AS RESULT
~~~

## 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터 타입과 UTC의 개념을 설명할 수 있다. 
* DATE, DATETIME, TIMESTAMP 에 대해서 설명할 수 있다.
* 시간함수들의 종류와 시간의 차이를 추출하는 방법을 설명할 수 있다. 
~~~

~~~
1) 날짜, 시간 데이터 타입 파악: ⭐️ DATE, DATETIME, TIMESTAMP ⭐️
2) 날짜, 시간 데이터 관련 알면 좋은 것: UTC, Millisecond
3) 날짜, 시간 데이터 타입 변환하기 (DATE ↔️ DATETIME ↔️ TIMESTAMP)
4) 시간함수 (두 시간의 차이, 특정 부분 추출하기)
~~~

### [시간 데이터 다루기]

#### DATE / DATETIME / TIME
- DATE: DATE 만 표시 => **2023-12-31**
- DATETIME : DATE + TIME (Time Zone 정보 ❌) => **2023-12-31 14:00:00**
- TIME: TIME 만 표시 => **23:59:59.00**

#### ⏲ TimeZone

* GMT
* UTC
  - 한국시간: UTC+9 = 한국 지역의 표준 시간대
  - 국제적인 표준시간 / 협정 세계시
  - 타임존이 존재 = 특정 지역의 표준 시간대

* TIMESTAMP
  - UTC부터 경과한 시간을 나타내는 값
  - TimeZone 정보 ⭕️

#### millisecond, microsecond

* millisecond
  - 1000ms = 1초
  - 빠른 반응이 필요한 분야에서 사용
     -> 초보다 더 중요할 때
     -> 누가 더 빠르게 주문?
  - **millisecond -> TIMESTAMP => DATETIME**

* microsecond
  - 1/1,000ms 

~~~
💡 잘못된 예시

1704176819711ms
- 2024-01-02 15:26:59 (DATETIME)

SELECT
TIMESTAMP_MILLIS(1704176819711) AS milli_to_timestamp_value,
TIMESTAMP_MICROS(1704176819711000) AS micro_to_timestamp_value,
DATETIME(TIMESTAMP_MICROS(1704176819711000)) AS datetime_value;

💡 맞는 예시 

SELECT
TIMESTAMP_MILLIS(1704176819711) AS milli_to_timestamp_value,
TIMESTAMP_MICROS(1704176819711000) AS micro_to_timestamp_value, 
DATETIME (TIMESTAMP_MICROS (1704176819711000)) AS datetime_value,
DATETIME (TIMESTAMP_MICROS(1704176819711000), 👉 'Asia/Seoul') AS datetime_value_asia;

⭐️ DATETIME 쓸떄는 꼭!! TimeZone ⭐️
DATETIME, (timestamp 정보, zone 정보)
~~~

### [TIMESTAMP, DATETIME 비교]

|| TIMESTAMP | DATETIME | 
|:---|:---|:---:|
| 타임존 | UTC | T (time) |
| 시간차이 | 한국시간 -9 | 한국 Zone 사용시, 한국시간과 동일 | 

<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

> 조건에 맞는 도서 리스트 출력하기
>
> **먼저 문제를 풀고 난 이후에 확인 문제를 확인해주세요**
>
> 문제 링크 
>
> :  https://school.programmers.co.kr/learn/courses/30/lessons/144853

<!-- 문제를 풀기 위하여 로그인이  필요합니다. -->

![image](../image/42.png)


## 문제 1

> **🧚Q. 프로그래머스 문제를 풀던 규서는 여러 번의 시행착오 끝에 결국 혼자 해결하기 어려워 오류 메시지를 공유하며 도움을 요청했습니다. 여러분들이 오류 메시지를 확인하고, 해당 SQL 쿼리에서 어떤 부분이 잘못되었는지 오류 메시지를 해석하고 찾아 설명해주세요.**

~~~sql
# 조건에 맞는 도서 리스트 출력하기
# 규서의 SQL 첫 번째 풀이
SELECT BOOK_ID, PUBLISHED_DATE
FROM BOOK
WHERE CATEGORY = '인문'
  AND YEAR(PUBLISHED_DATE, 2021);
  
오류 메시지 : Error: Number of arguments does not match for function YEAR
~~~



~~~
- YEAR 함수는 년도를 뽑아내는 역할만 하기 때문에 
  WHERE YEAR(PUBLISHED_DATE) = 2021 처럼 작성해야함

- 문제에서 DATE 데이터는 날짜, 시, 분, 초로 되어있었는데
  시간은 제외하고, 날짜만 추출하기 위해 
  DATE_FORMAT(PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE 을 시행함

SELECT BOOK_ID, 
       DATE_FORMAT(PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
FROM BOOK
WHERE CATEGORY = '인문'
  AND YEAR(PUBLISHED_DATE) = 2021
ORDER BY PUBLISHED_DATE ASC;
~~~



### 🎉 수고하셨습니다.