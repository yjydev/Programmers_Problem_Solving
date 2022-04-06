## x만큼 간격이 있는 n개의 숫자    
- 풀이   

```python  
def solution(x, n):
    return list(x*i for i in range(1,n+1))
```    
- 이번 문제는 x 와 n을 입력받아서, x 부터 시작해 x만큼씩 증가하는 n 개의 수의 리스트를 리턴하는 문제였다.     
- 처음엔 자연스럽게 `range` 함수를 떠올려서 `list(range(x,(x*n)+(x//abs(x)),x))` 를 생각했었는데,     
  테케 중 하나가 런타임 에러로 실패하였다 ㅠㅠ     
- 그래서 `for문` 돌리는 걸로 방법을 바꾸니까 바로 성공!     

