# Network Basics Lab

## 目的

VMware上にKali LinuxとUbuntu Serverを構築し、
Host-onlyネットワーク上で通信できることを確認する。

また、`ip addr` や `ping` を使用して、
IPアドレス、ネットワークインターフェース、CIDR、ICMPなどの
ネットワーク基礎を実際の環境と結びつけて理解する。

## Environment

- VMware Workstation
- Kali Linux
- Ubuntu Server 24.04 LTS
- Network Mode: Host-only

## Network

| Machine | Interface | IPv4 |
| --- | --- | --- |
| Kali Linux | eth0 | `192.168.10.129/24` |
| Ubuntu Server | ens33 | `192.168.10.128/24` |

Network:

`192.168.10.0/24`

## IPアドレス確認

### Kali Linux

```bash
ip addr
