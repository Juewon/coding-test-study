# 올바른 괄호
https://school.programmers.co.kr/learn/courses/30/lessons/12909

## 문제
**문제설명**   
괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어

- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.   

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.

**제한사항**
- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

**입출력 예**
|s|answer|
|-|-|
|"()()"|true|
|"(())()"|true|
|")()("|false|
|"(()("|false|

## Solution. 0
```python
def solution(s):
    # 초기값 False로 설정
    answer = False
    
    # s[0]이 ')'로 시작할 경우 True가 성립할수 없기 때문에 answer = False로 return
    if s[0] == ')':
        answer = False
        return answer
    
    # 열린 괄호('(')의 개수를 count 하기위해 start = 0으로 설정
    start = 0
    for i in range(len(s)):
        # 문자열 s를 순회하는동안 start가 0 이하일 경우 answer = False로 할당 후 for문 break
        if start < 0:
            answer = False
            break
        
        # s[i]가 열린 괄호('(')일 경우, start는 1이 증가하고
        # s[i]가 닫힌 괄호(')')일 경우, start는 1이 감소하게 설정
        elif s[i] == '(':
            start += 1
            
        elif s[i] == ')':
            start -= 1
    
    # for문이 끝난 후 start가 0이면(올바른 괄호인 경우) True를 return
    if start == 0:
        answer = True
        
    return answer
```

### Sol. 0 - 결과
> **100/100 점**