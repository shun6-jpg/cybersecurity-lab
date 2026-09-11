# Network Basics / ネットワーク基礎

VMware上のKali LinuxとUbuntu Serverを使用して、
ネットワークの基本動作を実際に確認した記録です。

IPアドレス、ICMP、ARP、TCPなどを、
実際の通信とパケットキャプチャを通して確認しました。

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

## Records / 実験記録

1. [IPアドレスとネットワークインターフェース](./01-ip-address.md)
2. [pingとICMPによる疎通確認](./02-ping-icmp.md)
3. [ARPとNeighbor Table](./03-arp-neighbor-table.md)
4. [ARPパケットのキャプチャ](./04-arp-packet-capture.md)
5. [ICMPパケットのキャプチャ](./05-icmp-packet-capture.md)
6. [TCP 3-way handshake](./06-tcp-handshake.md)
7. [TCP接続終了の確認](./07-tcp-connection-close.md)

## この章で確認したこと

- `ip addr` を使用したIPアドレスとインターフェースの確認
- 同一ネットワーク内でのICMP通信
- ARPによるIPv4アドレスとMACアドレスの対応確認
- `tcpdump` を使用したARP・ICMPパケットの観察
- TCP 3-way handshakeによる接続確立
- FIN / ACKによるTCP接続終了
- IPアドレス、MACアドレス、ARP、ICMP、TCPの関係
