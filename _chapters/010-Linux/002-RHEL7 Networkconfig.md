---
title: RHEL7 네트워크 구성
slug: RHEL7 네트워크 구성
abstract: RHEL7 네트워크 구성(nmcli, script, nmtui)
---

## 네트워크 구성

| Network Configuration Using nmcli<br/>Network Configuration Using CLI(Script)<br/>Network Configuration Using nmtui |
| ------------------------------------------------------------ |

### NMCLI를 이용한 네트워크 구성

네트워크를 구성하기 위해, 해당 서버의 네트워크 디바이스의 이름을 먼저 확인한다. 디바이스 이름이 확인된 다음, 해당 장치에 부여할 ip, 넷마스크의 prefix값, 게이트웨이 주소, dns 주소 등을 차례로 설정한 뒤, 네트워크를 활성화한다.

```
#네트워크 디바이스 확인
nmcli dev
#ip 설정
nmcli con mod [네트워크 디바이스 이름] ipv4.address [IP주소]/[넷마스크]
#gateway 설정
nmcli con mod [네트워크 디바이스 이름] ipv4.gateway [게이트웨이 주소]
#dns 설정
nmcli con mod [네트워크 디바이스 이름] ipv4.dns [DNS 주소]
#ip 고정 설정
nmcli con mod [네트워크 디바이스 이름] ipv4.method manual
#네트워크 활성화
nmcli con up [네트워크 디바이스 이름]
```

네트워크가 정상적으로 활성화 되었는지 확인하기 위해 다음의 명령어를 사용한다. ping이 잘 나가면, 해당 네트워크는 정상적으로 연결된 것이다.

```bash
#네트워크 인터페이스 확인
nmcli con show [네트워크 디바이스 이름] | grep -I ip[IP주소]
ip addr
ping 8.8.8.8
```

### Network Script를 이용한 네트워크 구성

vi 편집기로 /etc/sysconfig/network-scripts/ 디렉토리에 있는 ifcfg-[네트워크 디바이스 이름] 파일을 열어 아래의 내용으로 편집한다. 

```bash
vi /etc/sysconfig/network-scripts/ifcfg-[네트워크 디바이스 이름]
	TYPE=Ethernet
	PROXY_METHOD=none
	BROWSER_ONLY=no
	BOOTPROTO=none 또는 static
	DEFROUTE=yes
	IPV4_FAILURE_FATAL=no
	NAME=
	UUID=
	DEVICE=
	ONBOOT=yes
	GATEWAY=
	DNS1=
	NETMASK=
```

DHCP로 사용하는 경우, BOOTPROTO의 값을 "dhcp"로 변경하며, 고정 ip 사용시 "none" 또는 "static"의 값을 입력하여 사용한다.

ONBOOT 값을 yes로 지정하여, 재부팅시에도 해당 네트워크로 연결되도록 설정한다.

해당 설정 완료 후 네트워크 데몬을 재시작한 뒤, 설정한 값이 정상적으로 반영되었는지의 여부와 정상동작을 확인한다.

```bash
systemctl restart network
ip addr
ping 8.8.8.8
```

### NMTUI를 이용한 네트워크 구성

```bash
#NetworkManager 서비스 상태 및 활성화 확인
systemctl status NetworkManager
systemctl enable NetworkManager
#nmtui 서비스 실행
nmtui
```

![image-20230330162625533](C:\Git\ahekrltql.github.io\_chapters\010-Linux\002-RHEL7 네트워크 설정.assets\image-20230330162625533.png)

nmtui 명령 실행 후, 해당 화면이 표출되며, 여기서 "Edit a connection"을 선택한다.

Ethernet이 있는 경우에는 "Edit"을, 없는 경우에는 "Add > Ethernet > Create"를 진행한다.

각 항목을 작성한 뒤, "Automatically connect" 옵션을 선택한다. 해당 옵션은 script의 "onboot=yes"와 동일한 역할을 한다.

![image-20230330163140951](C:\Git\ahekrltql.github.io\_chapters\010-Linux\002-RHEL7 네트워크 설정.assets\image-20230330163140951.png)

필요한 설정이 완료되었다면, "OK"를 눌러 설정을 저장한 뒤 "Quit"을 선택하여 cli 화면으로 돌아온다.

설정이 잘 반영이 되었는지의 여부와 정상 동작 여부를 확인한다.

```bash
ip addr
ping 8.8.8.8
```





