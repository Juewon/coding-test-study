# 안전지대
https://school.programmers.co.kr/learn/courses/30/lessons/120866

## 문제 설명
지뢰는 2차원 배열 board에 1로 표시되어 있고 board에는 지뢰가 매설 된 지역 1과, 지뢰가 없는 지역 0만 존재합니다.   
지뢰가 매설된 지역의 지도 board가 매개변수로 주어질 때, 안전한 지역의 칸 수를 return하도록 solution 함수를 완성해주세요   
board = [[1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1]]

- 제한 사항
  - board는 n * n 배열입니다.
  - 1 ≤ n ≤ 100
  - 지뢰는 1로 표시되어 있습니다.
  - board에는 지뢰가 있는 지역 1과 지뢰가 없는 지역 0만 존재합니다.

## 문제 풀이
```python
def solution(board):
    # 위험지대 탐색할 좌표값 입력
    x = [-1, 0, +1]
    y = [-1, 0, +1]

    # 지뢰가 매설된 지역(1) 찾기
    for i in range(len(board)):
        for j in range(len(board[i])):
            if board[i][j] == 1:
                for a in x:
                    for b in y:
                        # 주어진 x,y 좌표값으로 순회 이동 후 index out of range를 대비해 예외처리 구문 작성
                        # 매설된 지역과 위험지역을 2로 변경해 구분
                        try:
                            if board[i+a][j+b] == 1:
                                continue    
                            else:
                                board[i+a][j+b] = 2
                        except:
                            continue
    # 안전지대(0)인 개수를 찾아 return             
    answer = 0                
    for x in board:
        for y in x:
            if y == 0:
                answer += 1
                       
            
    return answer
```
## 결과
- **90/100 점** (테스트 케이스 1 실패)


