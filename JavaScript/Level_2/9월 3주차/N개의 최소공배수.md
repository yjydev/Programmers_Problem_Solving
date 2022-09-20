## N개의 최소공배수   
- 풀이   

```javascript    

```    

- 이번 문제는 n개의 숫자를 담은 배열 `arr`이 주어질 때, 이 배열의 수들의 최소공배수를 리턴하는 문제였다.   

- 기본적으로 최소공배수는 `최대공약수 * (arr 원소들 / 최대공약수)` 한 값이다.    

  - 즉, `[2,6,8,14]` 라면, 최대공약수가 2이므로 `2 * (2/2) * (6/2) * (8/2) * (14/2)` 로 `168` 이 된다.    

- 그래서 위와 같은 최소공배수와 최대공약수의 개념을 활용하여 아래와 같이 접근해서 풀어보았다.    
  
  - 최대공약수를 `gcd` 변수에 1로 초기화하고, `(arr 원소들 / 최대공약수)` 값들을 담을 배열 `remind` 를 만들어    
  `arr.slice()` 메서드로 깊은 복사를 하였다.     
  
  - 최대공약수는 arr 배열의 가장 작은 수보단 같거나 작을테니까 `i` 범위를 `Math.min(...arr)` 보다 작거나 같게하여 반복문을 돌린다.    
  - `flag` 변수를 `true`로 초기화하고, `arr` 배열을 복사한 `remind` 배열의 각 원소들이 `i`로 나누어떨어지면 통과하고     
    아니면 `flag` 변수를 `false`로 바꾸고 `break`를 건다.    
  - 그 후, `flag` 변수가 `true` 이면 `remind` 배열의 모든 원소가 `i`로 나누어떨어진다는 소리이므로 공약수가 되어서 `gcd` 에 곱하고,    
    `arr` 배열의 길이만큼 반복문을 돌면서 `remind[k]` 원소를 `i`로 나눈 값으로 갱신한다.    
  - 마지막으로 `answer` 변수에 최대공약수 `gcd` 변수를 곱하고, `remind` 배열의 각 원소를 `answer` 에 곱해서 마무리한다.     

  ```javascript      
  function solution(arr) {
  var answer = 1;
  var gcd = 1;
  var remind = arr.slice();
  for (let i = 1; i <= Math.min(...arr); i++) {
      var flag = true;
      for (let j = 0; j < arr.length; j++) {
          if (remind[j] % i) {
              flag = false;
              break;
          };
      };
      if (flag === true) {
          gcd *= i;
          for (let k = 0; k < arr.length; k++) {
              remind[k] /= i;
          }
      }
    };
    answer *= gcd;
    remind.forEach((r) => answer *= r);
    return answer;
  }
  ```        
  
  - 하지만 이렇게 작성하면 모두 틀리거나 케이스 1개만 정답처리가 되어서 접근방법이 틀린건지 코드 작성에서 틀린건지    
    좀 더 고민해볼 필요가 있을 것 같다.       
    
    
  
