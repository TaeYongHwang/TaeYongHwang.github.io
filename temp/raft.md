[naver D2 raft 설명](https://d2.naver.com/helloworld/5663184)   
[참고 사이트](https://suckzoo.github.io/tech/2018/01/17/raft-3.html)   
[raft 공식 웹사이트](https://raft.github.io/)   

>leader는 변경 내역이 담긴 AppendEntry를 follower에게 전송하되, 변경 내역이 없더라도 일정 시간마다 하트비
>트로 더미 AppendEntry를 보내야 한다. 일정 시간이 지나도 leader로부터 AppendEntry를 받지 못하면 follower들은 자신의 상
>태를 candidate로 변경한다. candidate는 leader로 선출될 수 있는 후보군으로, candidate 중 과반의 투표를 얻은 노드가 leader로 선출된다.


>이전 leader가 네트워크 단절 등으로 한참 후에 돌아오는 경우 자신을 leader로 알고 있는 노드가 복수 개 존재할 수 있는데 이를 해결하기 위해 
>투표가 새로 시작될 때마다 term을 하나씩 증가시킨다. 단절된 leader가 전송하는 AppendEntry는 term이 더 낮으므로 다른 노드들에게 무시되며,
> 단절된 leader는 자신보다 term이 높은 leader의 AppendEntry를 수신하면 자신을 해당 term의 follower로 변경한다.


>각 팔로워 노드들은 처음 켜질 때, 그리고 AppendEntries 를 받을 때마다 랜덤한 타임아웃을 겁니다. 이 타임아웃동안 리더로부터 AppendEntries 를 단 한 번도 못 받았을 경우, 리더 노드가 죽은 것으로 판단합니다. 참고로 이 논문에서는 150~300ms의 타임아웃을 이용한다고 언급했습니다.
>팔로워 노드는 AppendEntries RPC 콜을 못 받아 리더가 죽은 것으로 판단하면 RequestVote RPC 콜을 이용하여 리더 후보 상태로 진입합니다. 이때의 term은 기존 리더의 term number보다 1 높아집니다. 쉽게 설명하자면, “k기 리더가 죽어서 k기 리더의 시대는 끝났다! 내가 k+1기의 리더가 될 것이다!”라는 메세지를 던지는 것입니다. 리더 후보는 클러스터의 노드들에게 과반의 찬성 response (respond positively)를 받으면 리더가 됩니다. 기본적으로 후보는 자기 자신에게 자동으로 찬성표를 던집니다.
> 팔로워 노드가 RequestVote RPC 콜을 이용하여 투표를 요청한 후보에게 찬성 Response를 주는 경우는 아래와 같습니다.
>- 자기 자신보다 term number가 높을 것
>- 다음 리더가 결정되기 전 이미 어떤 노드 v에게 찬성 투표를 했다면, v보다 term number가 높은 노드일 것