# 타겟넘버
https://school.programmers.co.kr/learn/courses/30/lessons/43165

## 문제
**문제 설명**
n개의 음이 아닌 정수들이 있습니다. 이 정수들을 순서를 바꾸지 않고 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

**제한 사항**
- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

**입출력 예**
|numbers|target|return|
|-|-|-|
|[1, 1, 1, 1, 1]|3|5|
|[4, 1, 2, 1]|4|2|

**입출력 예 설명**

**입출력 예 #1**

문제 예시와 같습니다.

**입출력 예 #2**
```
+4+1-2+1 = 4
+4-1+2-1 = 4
```

총 2가지 방법이 있으므로, 2를 return 합니다.

## Solution. 0
`DFS` 알고리즘을 활용해 리스트안의 모든 수의 조합을 탐색하는 방식을 통해 target넘버가 되는 횟수를 return하는 방법 사용

```python
def solution(numbers, target):
    # index와 현재 조합된 수를 0으로 통일
    idx = curr = 0
    
    def dfs(nums, target, curr, idx):
        # idx가 numbers의 길이와 같아질때, curr이 target과 같으면 1을 return
        # 그렇지않으면 0을 return함
        if idx == len(nums):
            return 1 if curr == target else 0
        
        # 재귀함수를 통해 idx는 계속 증가하면서 curr은 numbers[idx]에 맞게 더하고 뺄수 있는
        # 모든 조합을 탐색하면서 target과 같아질시 1이 return되는 모든 경우를 더해서 answer에 대입
        return dfs(nums, target, curr+nums[idx], idx+1) \
        + dfs(nums, target, curr-nums[idx], idx+1)
    
    answer = dfs(numbers, target, curr, idx)

    return answer
```

### Sol. 0 - 결과
> **100/100 점**