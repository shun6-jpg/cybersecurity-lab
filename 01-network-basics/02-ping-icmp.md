# Ping and ICMP

## 目的

Kali LinuxからUbuntu Serverへ `ping` を実行し、
同一ネットワーク内で通信できることを確認する。

## 実行

Kali LinuxからUbuntu Serverへ以下を実行した。

    ping 192.168.10.128

## 結果

以下の結果を確認した。

- 3 packets transmitted
- 3 received
- 0% packet loss

Kali LinuxからUbuntu Serverへの通信が成功した。

## ICMP

`ping` はTCPやUDPではなくICMPを利用する。

今回の通信では、

Kali Linux `192.168.10.129`

から

Ubuntu Server `192.168.10.128`

へICMP Echo Requestが送信され、

Ubuntu ServerからICMP Echo Replyが返された。

## 確認できたこと

- Kali LinuxとUbuntu Server間でIP通信が可能
- `ping` は疎通確認に利用できる
- `ping` ではICMPが利用される
- `0% packet loss` なので送信したパケットがすべて返ってきた
