---
layout: post
title: UDP Hole Punching
date: 2009-11-16 11:14:54
categories: [Network]
tags: [Network, UDP, HolePunching, STUN]
comments: true
---


## 홀 펀칭 (Hole Punching)
* 정확한 명칭은 STUN (Simple Traversal of User Datagram Protocol Through Network Address Translators)
 
공유기라는 녀석이 라우터의 특성도 함께 가지고 있어 Routing Table 을 작성하기 위해 P2P 통신을 목적으로,
사전에 상대방과 패킷을 주고받고 하여 각자의 공유기에 Routing Table 을 작성하는 것을 **홀 펀칭**이라고 한다.
 
* Full Cone NAT
  * 내부에 있는 호스트들의 모든 요청은, 모두 같은 외부 ip, port 로 맵핑된다.
  * 더군다나 어떤 외부 호스트든 공인 IP가 맵핑된 패킷 보내기에 의해 내부 호스트로 패킷을 보낸다.
* Restricted Cone
  * 목적지의 주소에 따라 NAT에 맵핑되는 포트가 달라진다.
  * 홀 펀칭을 위해서는 목적지의 IP만 동일시하여 뚫어주면 목적지의 패킷을 받을 수 있다.
* Port Restricted Cone
  * 목적지의 주소에 따라 NAT에 맵핑되는 포트가 달라진다.
  * 홀 펀칭을 위해서는 목적지의 IP와 포트를 동일시하여 뚫어주어야만 목적지의 패킷을 받을 수 있다.
* Symmetric Cone
  * 목적지의 주소와 포트에 따라 NAT에 맵핑되는 포트가 달라진다.
 
**P2P로의 1:1 연결에서는 적어도 한 쪽이 Symmetric Cone NAT 가 아니거나 공인 아이피를 소유하고 있는 Peer 여야 한다.**
 
## 홀펀칭 방식
1. Full cone
   * PC에서 UDP 데이터를 공유기 밖으로 보낼 때 해당 PC의 IP와 포트 정보를 공유기가 기억하고 공유기의 포트와 맵핑을 해줌.
   * 공유기의 해당 포트로 데이터가 오면 출발지 IP와 포트 정보를 상관하지 않고 해당 PC에 포워딩을 해줌.
2. Restricted Cone
   * PC에서 UDP 데이터를 공유기 밖으로 보낼 때 해당 PC의 IP와 포트 정보, 목적지 IP를 기억하고 공유기의 포트와 맵핑을 해줌.
공유기의 해당 포트로 데이터가 오면 출발지 IP정보를 비교하여 공유기에 기록된 목적지 IP와 같으면 해당 PC에 포워딩을 해줌.
3. Port Restricted Cone
   * PC에서 UDP 데이터를 공유기 밖으로 보낼 때 해당 PC의 IP와 포트 정보, 목적지 IP, Port 를 기억하고 공유기의 포트와 맵핑을 해줌.
공유기의 해당 포트로 데이터가 오면 출발지 IP 정보를 비교하여 공유기에 기록된 목적지 IP, Port 가 같으면 해당 PC에 포워딩을 해줌.
4. Symmetric NAT
   * PC에서 UDP 데이터를 공유기 밖으로 보낼 때 해당 PC의 IP와 포트 정보, 목적지 IP, Port 를 기억하고 공유기의 포트와 맵핑을 해줌.
   * 만약 목적지 IP나 Port 번호가 바뀌면 새로운 포트로 맵핑해줌.
   * 공유기의 해당 포트로 데이터가 오면 출발지 IP 정보를 비교하여 공유기에 기록된 목적지 IP, Port 가 같으면 해당 PC에 포워딩을 해줌.

## 구현 방법 
UDP 서버 (랑데뷰 피어)로 클라이언트가 UDP 패킷을 전송.
서버에 클라이언트의 IP와 Port 정보가 남는다.
이 정보를 바탕으로 현재 서버에 연결된 소켓에 접속할 IP와 포트 정보만 상대방 IP와 포트 정보를 넣고 상호간에 데이터 전송 시도하면,
Cone 방식은 UDP 홀펀칭이 성공한다.
 
간혹 Symmetric NAT 방식의 공유기가 있는데,
이 공유기 같은 경우에는 같은 소켓을 써도 UDP 데이터를 외부로 쏠 때, 목적지 IP나 포트 정보가 변경되면 공유기에서는 새로운 포트를 할당해 준다.

고로 나가는 것은 되나 들어오는 것이 안 됨.

한 쪽이 Symmetric NAT 방식이라면 상관 없는데 양쪽이 Symmetric NAT 방식이라면 낭패다.
UDP 릴레이 서버를 거치던지 다른 방법을 써야한다.
 
한쪽이 Symmetric NAT 방식이라면 반대쪽에서는 Symmetric NAT 쪽의 데이터를 받을 수 있다.
이 데이터를 받을 때 IP 와 포트 번호를 알아낼 수 있다.
알아낸 IP 와 포트 번호로 데이터 전송하면 됨.