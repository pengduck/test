---
title: 보안 (Security)
author: 펭덕
date: 2023-03-12 19:50:00 +0900
categories: [CS지식, OS]
tags: [공룡책, OS, Operating System, 운영체제, 보안, 암호화, 인증]
math: true
mermaid: true
image:
   src: https://user-images.githubusercontent.com/82709090/224715790-33df1d3b-750a-43c2-ae9f-a2be60689d9b.png
---

공룡책(10판) Chapter 16 요약

인프런 강의에 있어 슬슬 마지막 파트..! 보안과 보호이다. 비슷비슷해 보이지만, 보안은 인증(authentication), 보호는 인가(authorization), 권한과 관계된 부분을 다루게 된다.

## 보안 문제 _ Security Problem

컴퓨터 자원들이 모든 상황에서 의도된 대로 사용되고 접근된다면 안전하다고 할 수 있다. 보안이란, 실수로 혹은 고의로 자원이 잘못 사용되는 것, 혹은 공격을 감지하여 방어하거나 실수를 제어하는 것이다. cryptography(암호 기법)으로 주로 방어를 수행하게 될 것이다.

보안 위배 요소로 다음을 구분한다.

- 위협(threat) : 취약성 발견과 같은 보안 위반 잠재 가능성, 실수에 의해 발생하는 경우
- 공격(attack) : 악의적인, 보안을 깨뜨리기 위한 시도

보안 위반의 형태는 다음과 같은 것들이 있다.

- 기밀성 침해(confidentiality): 인증받지 않고 자료를 읽는 행위
- 무결성 침해(integrity): 인증받지 않고 자료를 변경
- 가용성 침해(availability): 인증받지 않고 자료를 파괴
- 서비스 가로채기(Theft of service): 인증받지 않고 자료를 사용, 설치
- 서비스 거부: 시스템의 적법한 사용을 막는다 (denial of service, DOS)
- (+) 권한 상승: 침입자의 권한을 초과하여 부여

시스템 보안을 위해 네 가지 수준의 보안 대책을 수립해야 한다.

1. Physical: 물리적 보호. 그냥 컴퓨터를 물리적으로 보호하는것
2. Network: 무단 액세스 제한, 통신 중단 방지 대책 수립
3. Operating System: 취약점을 잡아 공격 영역을 줄여 침입을 방어
4. Application: 보안 버그 주의

<br>

## 프로그램 위협 _ Program Threats

프로그램이 보안 침해를 유발할 때 사용하는 방법들이다.

1. 악성 코드 (Malware): 컴퓨터 시스템을 악용하여 비활성화, 손상되도록 설계된 소프트웨어이다. 권한 영역을 오용하여 실행되거나 비밀스럽거나 악의적인 방식으로 프로그램이 작동한다.
   - 트로이 목마, 스파이웨어, 랜섬웨어
   - 트랩 도어(백도어), 논리 폭탄
   > 철저한 권한 관리(전자)와 코드 검토(후자)를 생활화해야 하는데.. 어렵다.

2. 코드 주입 (Code Injection): 특정 소프트웨어에 코드를 심어서 의도적으로 재프로그래밍, 변경시킨다.
   - 버퍼 오버플로 공격, 스크립트 키디
   > 안전하지 않은 프로그래밍 패러다임의 결과물로 저수준 언어에서 많이 발견된다. 

3. 바이러스, 웜 (Viruses and Worms)
   - 바이러스: 다른 프로그램을 감염시키도록 고안된 프로그램으로 파일을 변경하거나 파괴한다.
   - 웜: 바이러스와의 차이는 인간의 활동 유무에 따라 구분된다. 혼자 복제하는 것이 웜
   > 쓰기 보호를 잘 걸거나 스팸, 피싱을 주의하자.

<br>

## 시스템과 네트워크 위협 _ System and Network Threats

더 개방적이고 더 많은 서비스, 더 많은 기능일 수록 악용할 버그가 많아진다. 역시 광범위한 네트워크는 더더욱 그렇다.

- 네트워크 트래픽을 통한 공격
   - 스니핑(sniffing): 네트워크 트래픽을 가로채서 정보를 훔친다.
   - 스푸핑(spoofing): 가짜 사이트로 위장하여 사용자의 개인정보를 얻어낸다.
   - 서비스 거부(Denial of Service): 자원을 획득, 훔치는게 아니라 시스템의 사용을 방해, 혹은 불가능하게 한다.
      - 분산 서비스 거부 공격(Distributed Denial of Service, DDoS): 여러 곳에서 동시 구동, 집중포화
   - 포트 스캐닝(Port Scanning): 자체로는 공격이 아니지만, Port 정보를 스캐닝하여 취약점을 찾아낸다. 방화벽으로 방어한다.

<br>

## 보안 도구로서 암호 기법 _ Cryptography as a Security Tool

가장 유용한 도구는 역시 암호 기법(Cryptography), 암호학이다. 송신자(senders)와 수신자(receivers) 간의 네트워크 메시지 전달에서, 소스, 종착지 간의 신뢰를 구현하려면 네트워크 자체의 신뢰성에 의존하지 않는, 암호 기법이 필요하다. 현대의 암호 기법은 `키(key)`를 활용하여 메시지 암복호화를 수행한다.

### - 암호화 _ Encryption

발신자가 특정 키를 가진 수신자만이, 혹은 데이터 작성자 본인만이 메시지를 읽는 것을 보장할 수 있도록 한다.

암호화 알고리즘은 아래 요소들로 구성된다.
- 키의 집합 K
- 메시지의 집합 M
- 암호문의 집합 C
- 함수 E: K -> (M -> C) ; K에 속한 모든 k에 대해 E<sub>k</sub>는 메시지로부터 암호문을 생성하는 함수 (무작위 매핑) - 암호화(encrypt)
- 함수 D: K -> (C -> M) ; K에 속한 모든 k에 대해 D<sub>k</sub>는 암호문으로부터 메시지를 생성하는 함수 - 복호화 (decrypt)

즉 key를 가진 사용자만이 암호화된 메시지를 복호화할 수 있다. 여기서 두 가지 유형의 암호화 알고리즘이 있다.

#### 대칭(symmetric) 암호화 알고리즘

A와 B가 동일한 key를 사용하기에, 비밀이 반드시 유지되어야 한다. 가장 일반적으로 사용되는 알고리즘은 데이터 암호화 표준 (data encryption standard, DES), 고급 암호화 표준(Advanced Encryption Standard, AES) 이다. 같은 키가 많이 사용될 수록 공격에 취약하고 블록 크기보다 긴 메시지를 처리하지 못하는 단점이 있다. 그리고 탈취되면 기냥 끝. 그래서 다른 제 3기관의 도움을 받음 (ex. SSL 인증서, HTTPS)

#### 비대칭(asymmetric) 암호화

암호화와 복호화의 키가 다르다. 두 개의 키를 생성하여, 그 중 하나 공개 키를 원하는 사람들에게 공개한다. 암호화는 공개된 키로 누구나 할 수 있으나, 오직 수신자만이 메시지를 복호화할 수 있다. Diffie와 hellman이 수학적으로 증명하였다.  

- RSA(Rivest, Shamir, Adleman): 가장 널리 사용되는 비대칭 알고리즘이다. 

#### 인증 _ Authentication

전송자의 메시지를 보호한다. 해싱 기법을 사용하여, 메시지가 변경되지 않았다는 것을 증명한다. 메시지 요약 함수는 해시를 생성하는 MD5, SHA-1 등을 포함하나, 메시지의 변경을 탐지하기 위해 인증 알고리즘을 사용한다. 대칭 암호화를 사용한 `메시지 인증 코드(Message Authentication Code, MAC)`, 비대칭 암호화를 사용한 `디지털 서명 알고리즘(Digital Signature Algorithm)`이 있다.

<br>

소개하지 못한 내용은 다음 기회에!