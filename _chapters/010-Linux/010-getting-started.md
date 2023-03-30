---
title: CentOS7.x 설치 및 설정
slug: CentOS7.x 설치 및 설정
abstract: CentOS 7버전의 설치 및 설정방법
---
1. **local.repo 설정**

```
# mkdir /mnt/RHEL8.5

# df -Th
```

2. iso 마운트 위치 확인

```
# cp -rf /run/media/root/CentOS-7-x86_64/Packages /root/local-repo/CentOS7
# createrepo /root/local-repo/CentOS7
# rm /etc/yum.repos.d/CentOS*
# vi /etc/yum.repos.d/local.repo

[CentOS7_local.repo]
name=CentOS7.9_local.repo
baseurl=file:///root/local-repo/CentOS7
gpgcheck=0
enabled=1
metadata_expire=-1

# yum clean all
# yum repolist
```

3. **방화벽 해제**

```
# systemctl stop firewalld
# systemctl disable firewalld
```

4. **Selinux 해제**

```
# vi /etc/selinux/config
7행 SELINUX=disabled 수정

# setenforce 0
```