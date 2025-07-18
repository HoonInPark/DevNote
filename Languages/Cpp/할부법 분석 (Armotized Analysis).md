

2025-03-14

----


## 개요
배열에서 일반적인 push 연산 동작에 소요되는 자원을 계산하는 방법을 설명한 부분이 있다. 
![[Pasted image 20250314152705.png||600]]

## 내용
ChatGPT에 물어보니 다음과 같이 일목요연하게 알려준다.
![[Pasted image 20250319151221.png]]

한마디로, 새 배열을 할당하고 원소들을 복사하는 등의 작업에 소모되는 자원을 각 push 연산으로 분배해서 미리 ***퉁치는*** 것.

좀 더 자세히 설명하면...
맨 뒤에 원소 하나를 빈 메모리 공간에 추가하는 push를 수행할 때...
1. 단순 삽입 연산에 `1` 소요.
2. 아래와 같은 상황일 때 새로운 메모리 공간으로 아래 원소들을 복사한다고 하면 코인을 이미 다 쓴 [0], [1], [2], [3] 원소의 복사를 위해 각각 1씩, 코인을 가지고 있는 [4], [5], [6], [7] 원소들 자신들의 복사를 위해 각각 1씩 소요. 
![[Pasted image 20250319155142.png||400]]

따라서 원소 하나의 push 연산을 위해 필요한 코인은 3.
(좀 재귀적인 사고가 필요하다.) 

그리고 복잡도 계산은 기본적으로 최악일 때를 기준으로 하므로 가장 최악일 때 `3n`의 비용을 가진다고 해도 ***상관 없다***.