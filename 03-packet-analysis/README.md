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
- tshark
- curl
- Wireshark
- Apache HTTP Server
- OpenSSL
- Python

## Records / 実験記録

1. [HTTP平文通信のパケットキャプチャ](./01-http-plaintext-capture.md)
2. [HTTP通信をEthernet・IP・TCP・HTTPの各層から解析](./02-http-layer-analysis.md)
3. [IPv4・TCPヘッダとHTTPデータの境界解析](./03-ip-tcp-http-boundary.md)
4. [HTTPとHTTPSのパケットキャプチャ比較](./04-http-vs-https.md)
5. [TLSハンドシェイクとClientHelloの解析](./05-tls-handshake-analysis.md)
6. [TLS証明書・公開鍵・鍵交換・共通鍵の概念整理](./06-tls-key-concepts.md)

## 現在までに確認したこと

- HTTPリクエスト・レスポンスを平文で確認した
- HTTPヘッダやHTML本文をtcpdumpから読み取った
- Ethernet・IP・TCP・HTTPの各層を確認した
- MACアドレス・IPアドレス・TCPポート番号を確認した
- `tcpdump -X` でパケットを16進数とASCIIで表示した
- IPv4ヘッダとTCPヘッダの境界を確認した
- TCPヘッダとHTTPデータの境界を確認した
- HTTP文字列を実際のバイト列から確認した
- HTTPとHTTPSの通信内容を比較した
- HTTPSではHTTPデータがTLSによって暗号化されることを確認した
- HTTPSでもIPアドレスやTCPポート番号は確認できることを確認した
- tsharkでTLS通信を解析した
- ClientHelloとServerHelloを確認した
- Cipher Suitesを確認した
- supported_groupsを確認した
- ALPNで `h2` と `http/1.1` が提示されることを確認した
- supported_versionsでTLS 1.3対応を確認した
- Application Dataが暗号化されていることを確認した
- 証明書・公開鍵・秘密鍵の役割を整理した
- 鍵交換と共通鍵暗号の関係を整理した
- 実際に観測した情報とTLSの一般的な仕組みを区別した

## この章の目標

この章では、

- パケットの各層を理解する
- Ethernetフレームを確認する
- IPヘッダを確認する
- TCP・UDPヘッダを確認する
- アプリケーション層のデータを確認する
- HTTP通信を解析する
- HTTPとHTTPSを比較する
- TLSハンドシェイクを解析する
- 証明書と暗号鍵の基本的な役割を理解する
- tcpdump / tsharkを使用して通信を解析する

ことを目標とする。

単にパケットを表示するだけでなく、

    Ethernet
        ↓
    IP
        ↓
    TCP
        ↓
    TLS / HTTP
        ↓
    Application Data

という通信の階層と、

    平文
        ↓
    TLSハンドシェイク
        ↓
    暗号化通信

という変化を関連付けて理解する。

## Next

次回は、
`03-packet-analysis` で行った実験全体をまとめる。

そのまとめをもって、
この章を一区切りとする。

その後、

[04-ids-suricata](../04-ids-suricata/)

へ進み、
これまで観察してきた通信を
IDSがどのように検知するのかを学ぶ。
