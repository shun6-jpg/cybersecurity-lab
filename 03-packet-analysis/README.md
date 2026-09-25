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

## 現在までに確認したこと

- `tcpdump -A` でHTTP通信の内容をASCII表示した
- HTTPリクエストの `GET` を確認した
- HTTPレスポンスの `200 OK` を確認した
- HTTPヘッダやHTML本文を平文で確認した
- `tcpdump -e` でEthernetヘッダを確認した
- 送信元・宛先MACアドレスを確認した
- EtherTypeがIPv4であることを確認した
- IP・TCP・HTTPが入れ子構造になっていることを確認した
- `tcpdump -X` でパケットを16進数とASCIIで表示した
- IPv4ヘッダとTCPヘッダの境界を確認した
- TCPヘッダとHTTPデータの境界を確認した
- HTTP文字列がバイト列として送信されていることを確認した
- HTTPとHTTPSのパケット内容を比較した
- HTTPSではHTTPデータがTLSによって暗号化されることを確認した
- HTTPSでもIPアドレスやTCPポート番号は確認できることを確認した
- TLSがTCPとHTTPの間で動作することを確認した
- tsharkでTLS通信を解析した
- ClientHelloとServerHelloを確認した
- TLSハンドシェイク後にApplication Dataが送信されることを確認した
- ClientHelloのCipher Suitesを確認した
- ALPNで `h2` と `http/1.1` が提示されることを確認した
- supported_versionsでTLS 1.3対応を確認した
- TLS 1.3のlegacy Versionフィールドについて確認した
- TLS 1.3ではServerHello以降のハンドシェイク情報の多くが暗号化されることを確認した

## この章の目標

この章では、

- パケットの各層を理解する
- Ethernetフレームを確認する
- IPヘッダを確認する
- TCP・UDPヘッダを確認する
- アプリケーション層のデータを確認する
- HTTP通信を解析する
- HTTPS通信とHTTP通信を比較する
- TLSハンドシェイクを解析する
- 証明書や鍵交換の仕組みを理解する
- Wireshark / tsharkを使用してパケットを詳しく解析する

ことを目標とする。

単にパケットをキャプチャするだけではなく、

    TCP接続
        ↓
    TLSハンドシェイク
        ↓
    暗号化通信の確立
        ↓
    Application Data

というHTTPS通信全体の流れを理解する。

## Next

次は、
TLSハンドシェイクで使用される、

    証明書
        ↓
    公開鍵
        ↓
    鍵交換
        ↓
    共通鍵
        ↓
    Application Dataの暗号化

という関係を整理する。

HTTPSがどのように安全な暗号化通信を確立しているのかを
鍵の観点から理解する。
