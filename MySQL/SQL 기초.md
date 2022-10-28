## SELECT     

- 조건에 맞는 레코드를 원하는 순서대로 출력        

  ```mysql
  SELECT (칼럼명) FROM (테이블명) [WHERE (조건)] [ORDER BY (정렬 기준 칼럼) {DESC - 내림차순 / ASC - 오름차순}]      
  ```      
  - `WHERE` 절에서 `AND`, `OR` 로 여러 조건을 같이 사용가능하다.       
  
  - `ORDER BY` 절에서 기본 정렬은 오름차순이며 `ASC` 는 생략가능하다.       

    - 만약, `조건1` 에 대해 오름차순 정렬하고, `조건1` 이 같을 경우 `조건2` 에 대해서 내림차순 정렬을 한다고 하면,    

      ```mysql      
      SELECT (칼럼명) FROM (테이블명) [WHERE (조건)] ORDER BY (조건1) , (조건2) DESC     
      ```     
      로 작성한다.      

  - `칼럼명` 대신 `COUNT()`, `MAX()`, `MIN()` 등으로 활용할 수도 있다.     

    - `COUNT(*)` 인 경우 모든 칼럼의 수를 카운트한다는 의미이다.    



<br>      

## GROUP BY    

- 칼럼을 그룹화하여 조회      

  ```mysql     
  SELECT (칼럼명) FROM (테이블명) [WHERE (조건)] GROUP BY (그룹화할 칼럼명) [HAVING (조건)] [ORDER BY (정렬 기준 칼럼)] 
  ```      
  
  - 여기서 `WHERE` 절은 그룹화 하기 전에 적용되는 조건이고, `HAVING` 절은 그룹화 하고 난 이후에 적용되는 조건이다.      

    - 예를 들어, 상품 판매 정보를 담은 테이블에서 동일한 회원이 동일한 상품을 재구매한 데이터를 조회하고자 한다면,    
      
      동일한 회원 ID 가 동일한 상품 ID를 구매한 이력이 2개 이상이어야 한다는 뜻이므로    
      
      ```mysql    
      SELECT USER_ID, PRODUCT_ID FROM ONLINE_SALE GROUP BY USER_ID, PRODUCT_ID HAVING COUNT(*) > 1     
      ```     
      
      로 작성하면 된다.     
      
      - 참고 : [프로그래머스 '재구매가 일어난 상품과 회원 리스트 구하기' ](https://school.programmers.co.kr/learn/courses/30/lessons/131536) 중 일부        
  
  - `ORDER BY` 절은 항상 맨 마지막에 작성되어야 한다.    

  
<br>     

## JOIN    

- 여러 테이블을 특정 칼럼을 기준으로 병합            

### (INNER) JOIN    

- 기본적으로 `JOIN = INNER JOIN` 으로 `INNER` 은 생략 가능하며, 조인하는 테이블 **모두에 값이 있는 행**을 반환한다.      

  ```mysql    
  SELECT (칼럼명) FROM (테이블1 이름) (테이블1 약칭)
  JOIN (테이블2 이름) (테이블2 약칭) ON (조인 조건) [WHERE (조건)] [GROUP BY (그룹화할 칼럼명)] [...] [ORDER BY (정렬 기준 칼럼)]
  ```     

  - 만약, `TABLE_1` 테이블을 `T1` 약칭으로, `TABLE_2` 테이블을 `T2` 약칭으로 하여 `동일 ID` 값으로 `JOIN` 한다고 하면,     
    
    ```mysql    
    SELECT * FROM TABLE_1 T1 JOIN TABLE_2 T2 ON T1.ID = T2.ID
    ```          
    
    로 작성한다.      
    
  - `JOIN ... ON ...` 이후로 동일하게 `WHERE` 조건절이나 `GROUP BY` 그룹화 등등 다양하게 조합할 수 있다.     
    
  - 2개뿐만 아니라 그 이상의 테이블을 조인할 수도 있다.    

  - 동일한 테이블끼리 조인하는 경우는 `SELF JOIN` 이라고 한다. 방법은 `INNER JOIN`과 동일하다.        


<br>    

### LEFT/RIGHT (OUTER) JOIN      

- 반대쪽의 데이터 여부와 무관하게 선택된 방향에 데이터가 있으면 행 반환        

  ```mysql    
  SELECT T1.NAME, T2.NAME FROM TABLE_1 T1 (LEFT/RIGHT) JOIN TABLE_2 T2 ON T1.ID = T2.ID     
  ```     

  - `(INNER) JOIN` 과 마찬가지로 `LEFT/RIGHT (OUTER) JOIN` 에서 `OUTER` 생략가능     
  
  - `IFNULL` 함수로 데이터가 없을 때 대신 출력할 문구 설정도 가능     

    ```mysql     
    SELECT IFNULL(T1.NAME, 'T1_NONE'), IFNULL(T2.NAME, 'T2_NONE') FROM TABLE_1 T1 LEFT JOIN TABLE_2 T2 ON T1.ID = T2.ID    
    ```      

  - LEFT JOIN      

    - 반대쪽에 데이터가 없어도 **왼쪽(LEFT)에 데이터가 있으면 행 반환**       
    
    - `T1.ID = T2.ID` 조건을 만족하는 `T2.NAME` 데이터가 없더라도 `T1.NAME` 이 있으면     
      
      `T2.NAME`은 NULL 값을 가지고 `T1.NAME` 값을 출력하면서 행 반환       

  - RIGHT JOIN      

    - 반대쪽에 데이터가 없어도 **오른쪽(RIGHT)에 데이터가 있으면 행 반환**         
    
    - `T1.ID = T2.ID` 조건을 만족하는 `T1.NAME` 데이터가 없더라도 `T2.NAME` 이 있으면   
      
      `T1.NAME` 은 NULL 값을 가지고 `T2.NAME` 값을 출력하면서 행 반환    

<br>    

### CROSS JOIN    

- 조건 없이 모든 행 반환     

  ```mysql     
  SELECT T1.NAME, T2.NAME FROM TABLE_1 T1 CROSS JOIN TABLE_2 T2  
  ```    
  
  - T1 이 총 9개 행이고 T2 가 총 10개 행이라면 `CROSS JOIN` 했을 때, 총 90개 행이 반환된다.    
  

<br>       

## 집합      

- `JOIN` 과 달리 위 아래로 모든 행을 나열     

  - `JOIN` 은 `A X B` 형태이고, `UNION` `UNION ALL` 등의 합집합은 그냥 위 아래로 모든 항을 쭉 열거하는 형태   

- 서브 쿼리를 활용할 수도 있다.   

  ```mysql     
  SELECT * FROM ( SELECT NAME, ID FROM T1 WHERE NAME = '%I%' 
                  UNION ALL 
                  SELECT NAME, ID FROM T2 WHERE NAME = '%I%' ) A 
            ORDER BY NAME
  ```     

### UNION     

- 중복을 제거한 합집합 결과 반환    

  ```mysql    
  SELECT (칼럼명) FROM (테이블명) [WHERE 조건절] UNION SELECT (칼럼명) FROM (테이블명) [...]   
  ```       
  
### UNION ALL    

- 중복을 제거하지 않은 합집합 결과 반환    

  ```mysql    
  SELECT (칼럼명) FROM (테이블명) [WHERE 조건절] UNION ALL SELECT (칼럼명) FROM (테이블명) [...]   
  ```      

<br>     
  
## NULL값 처리    

- 만약, `NULL` 값은 따로 지정한 값으로 (`ex. 'NONE'`) 출력하고자 한다면,     
  ```mysql    
  SELECT IFNULL(칼럼명, 'NONE') FROM (테이블명)    
  ```    
  으로 작성하고,      
  ```mysql    
  SELECT IFNULL(칼럼명, 'NONE') AS (칼럼명) FROM (테이블명) 
  ```     
  으로 `AS` 문을 추가하면 더 깔끔하게 출력가능하다.    
  
  - 간혹가다 출력 칼럼명도 지정해주는 경우에 유용하게 활용 가능하다.      

<br>      

- `NULL` 값은 제외하고 출력하고자 한다면,     

    ```mysql    
    WHERE NOT (칼럼명) is NULL
    ```      
    로 사용 가능하다.      
    
<br>     

## 날짜 형식      

- 기본적으로 MySQL 에선 `DATE_FORMAT(칼럼명, 지정 형식)` 으로 날짜 형식을 변경할 수 있다.    

  - 지정 형식은 아래와 같다. 더 자세한 사항은 [공식 홈페이지 참고](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format)              

    형식 | 의미 | 형식 | 의미 
    ----- | ----- | ---- | ---- 
     %Y | 4글자 년 (`ex. 1999`) | %y | 2글자 년 (`ex. 99`)     
     %M | 영문 전체 월 (`ex. March`) | %b | 영문 축약 월 (`ex. Mar`)   
     %m | 2글자 월 (`ex. 01`)  | %c | 월 (`ex. 1, 10`)     
     %d | 2글자 일 (`ex. 02`)  | %e | 일 (`ex. 2, 30`)   
     %W | 영문 전체 요일 (`ex. Monday`) | %a | 영문 축약 요일 (`ex. Mon`)   
     %H | 24시간 표기 (00 to 23) | %k | 24시간 표기 (0 to 23)  
     %h | 12시간 표기 (00 to 12) | %I | 12시간 표기 (1 to 12)
     %i | 분 (00 to 59) | %S | 초 (00 to 59)    
     %p | PM, AM 표시 | %r | 12시간 표기 + PM,AM 표시 (`ex. hh:mm:ss AM/PM`)   
     %T | 24시간 표기 (`ex. hh:mm:ss`) 
     
   - 기타     

      형식 | 의미 | 형식 | 의미   
      ----| ---- | ---- | ----    
      %D | 서수(1st, 2nd)로 날짜 표현 | %j | 일 (001 to 366) 
      %f | Micro Seconds (000000 to 999999) | %% | % 문자 표시 
      %V | 올해의 몇번째 주 (01 to 53), **일요일**이 주의 첫 번째 날. with `%X`| %X | **일요일**이 첫 요일인 주 표시를 위한 4자리 년도 
      %v | 올해의 몇번째 주 (01 to 53), **월요일**이 주의 첫 번째 날. with `%x`| %x | **월요일**이 첫 요일인 주 표시를 위한 4자리 년도  
      %w | 요일 순서 숫자 (`ex. 일요일=0, 토요일=6`) |    
      
<br>     
    
- 그러므로 날짜 형식을 지정하여 출력하고자 한다면 위의 포맷을 조합하여 작성하면 된다.        

  ```mysql    
  DATE_FORMAT(칼럼명, '%Y-%m-%d') AS (칼럼명)     
  ```     
  처럼 작성하면 `2022-03-01` 와 같은 형태로 출력할 수 있다.    
     
    
