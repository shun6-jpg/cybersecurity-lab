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
2. [HTTP通信をEthernet・IP・TCP・HTTPの各層から解析](./02-http-layer-analysis.md)

## 現在までに確認したこと

- `tcpdump -A` でHTTP通信の内容をASCII表示した
- HTTPリクエストの `GET` を確認した
- HTTPレスポンスの `200 OK` を確認した
- HTTPヘッダやHTML本文を平文で確認した
- `tcpdump -e` でEthernetヘッダを表示した
- 送信元・宛先MACアドレスを確認した
- EtherTypeがIPv4であることを確認した
- 送信元・宛先IPアドレスを確認した
- TCPの送信元・宛先ポート番号を確認した
- TCPのペイロードとしてHTTPデータが送信されることを確認した
- Ethernet・IP・TCP・HTTPが入れ子構造になっていることを確認した
- `tcpdump -X` でパケットを16進数とASCIIの両方で表示した
- HTTPの文字列もバイト列として通信されていることを確認した
- HTTPリクエストとレスポンスが同じTCP接続上でやり取りされることを確認した

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
    どの層のヘッダなのか
    ↓
    ペイロードには何が入っているのか
    ↓
    暗号化された場合は何が変わるのか

まで関連付けて理解する。

## Next

次は、
パケットの16進数表示を利用して、

    IP Header
        ↓
    TCP Header
        ↓
    HTTP Data

の境界を確認する。

実際のバイト列と、
tcpdumpが表示している情報を対応させて解析する。
