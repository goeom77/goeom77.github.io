---
layout: post
title:  " [Network] Public IP and Private IP"
categories: Network
author: start-easy
---
* content
{:toc}


### 1. 공인 IP와 사설 IP

- **공인 IP (Public IP)**는 인터넷 상에서 전 세계적으로 유일한 IP입니다. 즉, 이 IP 주소는 인터넷에 직접 연결된 장비가 갖고 있는 주소로, 다른 인터넷 사용자들이 이 주소로 접근할 수 있습니다.
- **사설 IP (Private IP)**는 특정 네트워크 내에서만 유효한 IP 주소입니다. 예를 들어, 가정이나 회사의 내부 네트워크에서 사용됩니다. 사설 IP는 공인 IP와 달리 외부 인터넷에서는 유일하지 않으며, 여러 네트워크에서 중복될 수 있습니다. 사설 IP의 주요 범위는:
    - 10.0.0.0 ~ 10.255.255.255
    - 172.16.0.0 ~ 172.31.255.255
    - 192.168.0.0 ~ 192.168.255.255

### 2. 사설 IP와 공인 IP의 관계

- **사설 IP**는 보통 네트워크 내에서만 사용되고, 외부 인터넷에 접근할 때는 **공인 IP**를 통해 나가게 됩니다. 이 과정에서 **NAT (Network Address Translation)**라는 기술이 사용됩니다. NAT는 사설 IP를 공인 IP로 변환하거나, 공인 IP로 들어오는 요청을 해당 사설 IP로 전달하는 역할을 합니다.
- 여러 대의 사설 IP를 사용하는 사람들이 공인 IP를 공유할 수 있다는 점은 맞습니다. 즉, 여러 사람의 인터넷 트래픽이 하나의 공인 IP를 통해 나가게 될 수 있습니다. 이 경우, 외부에서 오는 응답은 공인 IP로 돌아오고, NAT는 이를 각 사설 IP로 적절히 다시 전달합니다.

### 3. 누가 보낸 것인지 추적하기

- 사설 IP는 **공인 IP**를 통해 인터넷에 연결되므로, 외부에서 사설 IP를 직접적으로 추적하는 것은 불가능합니다. 즉, 외부 서버는 요청을 보낸 기기의 공인 IP만 알 수 있고, 이 IP가 어느 내부 네트워크의 사설 IP에서 온 것인지는 알 수 없습니다. 이는 NAT의 특성상 공인 IP를 공유하는 여러 사용자들이 있을 수 있기 때문입니다.
- 따라서 **공인 IP**를 통해 외부에서 온 요청을 추적하려면, 해당 요청이 발생한 내부 네트워크의 NAT 로그나 방화벽 로그 등을 통해 사설 IP를 파악해야 합니다. 이 과정은 외부에서 직접 추적하는 것보다 훨씬 더 복잡할 수 있습니다.

`ipconfig` 는 내부 ip를 확인하는 것

`nslookup [myip.opendns.com](http://myip.opendns.com). [resolver1.opendns.com](http://resolver1.opendns.com)` 는 외부 ip를 확인하는 것

### 4. 내 ip 확인 방법

1. 네이버 ip주소 조회를 이용 → 공인 ip가 나온다.
2. cmd(명령 프롬프트)에서 ipconfig의 IPv4 Adress를 확인, window + R > “enter” > cmd > ipconfig
3. 제어판 > 네트워크 및 인터넷 > 네트워크 및 공유 센터 > 연결 값 > 자세히
4. ctrl + alt + esc > 작업관리자 > 성능 > ipv4 주소 확인

1번 만 공인 ip가 나오고 나머지는 사설 ip가 나온다.