## K번째 수     
- 풀이    

```javascript    
function solution(array, commands) {
    return commands.map((com) => array.slice(com[0]-1, com[1]).sort(function(a,b){return a-b})[com[2]-1])
}
```     
- 이번 문제는 주어진 배열 `array`를 i부터 j번째까지 자르고, 정렬한 후 k번째 수를 구하는 문제였다.     
- i, j, k는 commands 배열로 주어진다. 즉, `commands = [ [i,j,k], [i,j,k], ...]` 형태로 주어진다.     
- 비교적 간단한 문제였으므로 javascript를 최대한 활용하여 한줄코딩하는데 초점을 맞췄다.     
- 그래서 commands 배열의 각 원소를 꺼내와서, 각 원소의 0번째 수 = i, 1번째 수 = j 이므로     
  `com[0]-1` 인 i번 부터 `com[1]-1` 인 j번까지 slice로 자르고, `sort`로 정렬한다.    
  - 여기서 정렬하는 기준을 `function(a,b){return a-b}` 로 해서 숫자 오름차순으로 정렬하였다.     


- 그리고 `com[2]-1`인 k번째의 수를 꺼내서 return하면 완료    
