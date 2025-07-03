

2025-06-15

----
#BP #anim #Unreal 

## 개요
내 Hyperion 클라이언트 프로젝트에 적용하고 싶은 것은 속도, 가속도에 따라 부드러운 애니메이션 전환이 가능한 것이다. 

## 내용
### 필요한 게임 로직상의  정보들
캐릭터 객체의 움직임을 받아와서 애니메이션에 반영해야 하는데, `Event Graph`에서 정보를 가져오는 부분은 다음과 같다. (`Update Logic` -> `Update Essential Values`)
![[Pasted image 20250615234920.png]]

`Pre-Event Graph`에 해당하는 노드들은, 다른 BP `Event Graph`에선 볼 수 없었던 것으로, 해당 노드가 속한 이벤트(붉은 노드)가 호출되기 전에 값을 가져오는 행위를 한다. 
![[Pasted image 20250615234554.png]]

### Update_Trajectory
![[Pasted image 20250620151525.png]]
![[Pasted image 20250620151542.png]]
![[Pasted image 20250620151554.png]]