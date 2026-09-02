# Network Basics / ネットワーク基礎

VMware上のKali LinuxとUbuntu Serverを使用して、
ネットワークの基本動作を実際に確認した記録です。

## Environment / 環境

- Windows 11
- VMware Workstation
- Kali Linux
- Ubuntu Server 24.04 LTS
- Network Mode: Host-only

## Lab Network / 検証ネットワーク

| Machine | Interface | IPv4 |
| --- | --- | --- |
| Kali Linux | eth0 | `192.168.10.129/24` |
| Ubuntu Server | ens33 | `192.168.10.128/24` |

Network: `192.168.10.0/24`

## Labs / 実習一覧

1. [IPアドレスとネットワークインターフェース](./01-ip-address.md)
2. [pingとICMPによる疎通確認](./02-ping-icmp.md)
3. [ARPとNeighbor Table](./03-arp-neighbor-table.md)

## Next

- ARPパケットの観察
- tcpdumpによるパケットキャプチャ
- ルーティング確認
- DNS通信の確認
