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
- OpenSSL
- Python

## Records / 実験記録

1. [HTTP平文通信のパケットキャプチャ](./01-http-plaintext-capture.md)
2. [HTTP通信をEthernet・IP・TCP・HTTPの各層から解析](./02-http-layer-analysis.md)
3. [IPv4・TCPヘッダとHTTPデータの境界解析](./03-ip-tcp-http-boundary.md)
4. [HTTPとHTTPSのパケットキャプチャ比較](./04-http-vs-https.md)

## 現在までに確認したこと

- `tcpdump -A` でHTTP通信の内容をASCII表示した
- HTTPリクエストの `GET` を確認した
- HTTPレスポンスの `200 OK` を確認した
- HTTPヘッダやHTML本文を平文で確認した
- `tcpdump -e` でEthernetヘッダを確認した
- 送信元・宛先MACアドレスを確認した
- EtherTypeがIPv4であることを確認した
- 送信元・宛先IPアドレスを確認した
- TCPの送信元・宛先ポート番号を確認した
- Ethernet・IP・TCP・HTTPが入れ子構造になっていることを確認した
- `tcpdump -X` でパケットを16進数とASCIIで表示した
- IPv4ヘッダとTCPヘッダの境界を確認した
- TCPヘッダとHTTPデータの境界を確認した
- HTTP文字列が実際にはバイト列として送信されていることを確認した
- HTTPSテストサーバーを構築した
- TCP 443番ポートでHTTPS通信を行った
- HTTPではGETやHTML本文を平文で確認できることを確認した
- HTTPSではHTTPデータがTLSによって暗号化されることを確認した
- HTTPSでもIPアドレス・ポート番号などは確認できることを確認した
- TLSがTCPとHTTPの間でHTTPデータを暗号化することを確認した
- tcpdumpで監視するインターフェースと通信経路の関係を確認した

## この章の目標

この章では、

- パケットの各層を理解する
- Ethernetフレームを確認する
- IPヘッダを確認する
- TCP・UDPヘッダを確認する
- アプリケーション層のデータを確認する
- HTTP通信を解析する
- 暗号化されたHTTPS通信と比較する
- TLS通信を観察する
- Wireshark / tsharkを使用してパケットを詳しく解析する

ことを目標とする。

単にパケットをキャプチャするだけではなく、

    なぜこの情報が見えるのか
    ↓
    どのプロトコルの情報なのか
    ↓
    どの層のヘッダなのか
    ↓
    どこから暗号化されているのか
    ↓
    暗号化後も何が観察できるのか

まで関連付けて理解する。

## Next

次はHTTPS通信を構成するTLSを詳しく観察する。

    TCP Connection
        ↓
    TLS Handshake
        ↓
    Encrypted Application Data

という流れを確認し、
HTTPデータが暗号化される前に
どのような処理が行われているのかを解析する。
