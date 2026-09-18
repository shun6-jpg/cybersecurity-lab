# Packet Analysis / パケット解析

Kali LinuxとUbuntu Serverを使用して、
ネットワーク上を流れるパケットの内容を解析する実験記録です。

これまでの `01-network-basics` と `02-port-scanning` では、

- IPアドレス
- ARP
- ICMP
- TCP
- UDP
- ポート
- Firewall
- Nmap

などを使用して、
通信の仕組みやポート状態を確認しました。

この章ではさらに一段深く、
実際にパケットの中にどのような情報が含まれているのかを観察します。

## Environment / 環境

- Windows 11 Host
- VMware Workstation
- Kali Linux
- Ubuntu Server 24.04 LTS
- Host-only Network: `192.168.10.0/24`

主に以下のツールを使用します。

- tcpdump
- curl
- Wireshark / tshark
- Apache HTTP Server

## Records / 実験記録

1. [HTTP平文通信のパケットキャプチャ](./01-http-plaintext-capture.md)

## 現在までに確認したこと

- `tcpdump -A` でパケットのペイロードをASCII表示した
- HTTPリクエストの `GET` を確認した
- `Host` ヘッダを確認した
- `User-Agent` を確認した
- HTTPレスポンスの `200 OK` を確認した
- ApacheのServerヘッダを確認した
- `Content-Type: text/html` を確認した
- HTTPレスポンスのHTML本文を確認した
- TCPのペイロードとしてHTTPデータが運ばれていることを確認した
- HTTP通信では内容を平文で読み取れることを確認した

## この章の目標

この章では、

- パケットの各層を理解する
- Ethernetフレームを確認する
- IPヘッダを確認する
- TCP・UDPヘッダを確認する
- アプリケーション層のデータを確認する
- HTTP通信を解析する
- 暗号化されたHTTPS通信と比較する
- Wireshark / tsharkを使用してパケットを詳しく解析する

ことを目標とする。

単にパケットをキャプチャするだけではなく、

    なぜこの情報が見えるのか
    ↓
    どのプロトコルの情報なのか
    ↓
    暗号化された場合は何が変わるのか

まで関連付けて理解する。

## Next

次は、
HTTP通信を構成しているパケットをより詳しく確認し、

    Ethernet
    IP
    TCP
    HTTP

という各層の情報を分けて解析する。
