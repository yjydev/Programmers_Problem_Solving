## k번째 수   
- 풀이   

```python   
def solution(array, commands):
    answer = [0]*(len(commands))
    for index, com in enumerate(commands):  
        i,j,k = com
        new_array = sorted(array[i-1:j])
        answer[index] = new_array[k-1]
    return answer
```     
- 주어진 배열을 i부터 j번째까지 slice 하여 정렬한 뒤 k번째 수를 구하는 문제였다.    
- 문제 자체가 설명이 간단명료하게 되어있어서 금방 풀 수 있었다.    
- 다른 사람들의 풀이를 보니 lambda를 이용해서 한줄코딩하는 경우도 있던데, 물론 짧다고 다 좋은건 아니겠지만    
  람다가 아직도 약간 아리까리해서 람다에 익숙해지기도 할 겸, 다음부턴 이렇게 간단한 문제들은 람다를 이용하는 방법도 고려해봐야겠다.     
  
  
