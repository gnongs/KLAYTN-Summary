<h1 align="center">Klaytn 클레이튼 블록체인 어플리케이션 만들기</h1>
<p align="center">Klaytn을 통해 블록체인에 대한 이론을 정리하고 dapp만들어보기</p>

## 기존 블록체인 플랫폼의 약점
### 1. Scalability
확장성, 얼마나 많은 일을 신속히 처리할 수 있는지를 뜻하고, Etherium과 비트코인에선 확장성이 낮다고 평가됨

### 2. Finality
최종성, 변경불가능한 최종적인 상태, 이더리움과 비트코인에선 확률론적 최종성만 제공하게 됨

<strong>블록이 Final 하다 = 블록에 담긴 TX가 바뀔 수 없다는 걸 보증</strong>

### 3. Fork
블록들간의 연결이 두 개이상의 분기로 갈라지는 현상을 의미, 모든 참여자들이 독립적으로 채굴을 할 수 있기 때문에 가능

<strong>작업증명(PoW) 방식</strong>

1. 블록체인에 블록 추가하기 위해 문제를 품(Hash값 찾기)
2. 비슷한 시간에 문제해결이 동시에 일어날 경우 분기가 발생하게 됨
3. 자식 블록이 먼저 생성된 블록을 기준으로 계속해서 이어지게됨(Longest Chain Rule -> 51%를 누군가 가지고 있을 경우 사기의 위험이 있음)

## 클레이튼 이해하기
### 클레이튼 합의 알고리즘 "IBFT"
강력한 보안 및 성능을 유지하면서 엔터프라이즈 급 편의성을 제공을 통해 공개를 통한 개인적인 합의 신뢰모델을 제공
### IBFT의 단계
1. Propose

    한 노드가 Proposer로 설정이 됨

2. pre-prepare

    Proposer 노드가 다른 노드들에게 블록을 제안하는 단계

3. prepare

    검증자 노드들(Validator)이 Proposer에게 받은 메세지를 잘 받았다고 다른 노드들에게 메세지 전송

4. commit 

    Proposer에게 받은 블록을 수락할 것인지 다른 노드들과 소통하며 결정하는 단계 2/3이상이 합의를 인정하면 블록 승인을 하고 다음 단계로 넘어감, final상태가 없음

5. reply

### IBFT의 특징
1. 합의 노드들끼리 통신을 통해 합의를 이끌어내고 그 즉시 완결성을 가짐

2. 합의 노드가 많아질수록 통신량이 기하급수적으로 늘어나지만 이러한 부분은 부분적으로 합의노드들을 추출하는 방식을 가짐

### 블록 생성 및 전파
<strong>블록생성 사이클</strong>
- 블록 생성 주기 = 라운드 (round)
- 블록 생성 간격 = 약 1초

<strong>제안자와 위원회 선택(Proposer and Committee Selection)</strong>
- 제안자를 무작위 & 결정적으로 *Governance Council 노드들을 중 선택
- 각각의 합의 노드가 가장 최근의 블록 헤더에서 파생된 난수를 사용하여 자기가 라운드에 선택됐는지를 증명하는 것을 의미
- 제안자는 제안자의 공개키를 통해 입증가능한 암호 증명을 사용
- 제안자와 위원회를 파악하면 제안자가 블록을 만들고 합의

    _*Governace Concil: 합의 노드들의 집합_

<strong>블록전파(Block Propagation)</strong>
- 위원회에서 합의에 이르게 되면 새로운 블록이 모든 합의 노드들에게 전달되고 합의 라운드는 종료
- 프록시 노드들을 통해 엔드 노드들에게 해당 블록이 전달되게 됨

### 네트워크 구조

Core Cell Network = CNN + PNN = Core Cell = Consensus Node(CN) + Proxy Node(PN)<br>
CNN(Consensus Node Network) = 여러 개의 CN + CN BootNode<br>
PNN(Proxy Node Network) = 여러 개의 PN + PN BootNode<br>
ENN(Endpoint Node Network) = Endpoint Node + EN BootNode<br>
_각각의 노드들 끼리는 연결되어 있다._
_EndpointNode가 실제 서비스를 제공하는 노드들이다._

### 코어 셀(Core cell)
사용자가 많아 확장이 필요할 때 클레이튼의 경우 코어 셀 노드 자체의 성능을 늘려야 함. 하지만 하나의 노드만 높힐 게 아니라 전체 노드들의 성능을 높혀야 함<br>
코어 셀 네트워크는 합의를 담당하는 노드이기 때문에 Connection 때문에 성능의 문제가 생기면 안되기 때문에 PN을 사용하게 되고, Endpoint의 확장성을 높힐 수 있음<br>

<strong>CN(합의 노드) 참여 조건</strong>
- Physical core가 40개 이상
- 256GB RAM
- 1년치의 데이터 약 14TB 저장
- 10G 네트워크