# Packet Analysis / パケット解析

Kali LinuxとUbuntu Serverを使用して、
ネットワーク上を流れるパケットを解析した記録です。

この章では、
HTTP平文通信からTLS暗号化通信までを対象に、

- Ethernet
- IPv4
- TCP
- HTTP
- TLS

の関係を実際のパケットから確認しました。

## Environment / 環境

- Windows 11 Host
- VMware Workstation
- Kali Linux
  - `192.168.10.129`
- Ubuntu Server 24.04 LTS
  - `192.168.10.128`
- Host-only Network
  - `192.168.10.0/24`

主に使用したツール：

- tcpdump
- tshark
- curl
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
7. [Packet Analysis Summary](./07-packet-analysis-summary.md)

## この章で確認したこと

### HTTP

- HTTPリクエストを平文で確認
- HTTPレスポンスを平文で確認
- HTTPヘッダを確認
- HTML本文を確認

### Network Layers

- Ethernet
- IPv4
- TCP
- HTTP

の階層構造を確認した。

### Packet Bytes

`tcpdump -X` を使用して、
実際のパケットを16進数で解析した。

- IPv4 Header
- TCP Header
- HTTP Data

の境界を確認した。

### HTTPS

HTTPとHTTPSを比較し、
HTTPSではHTTPデータが
TLSによって暗号化されることを確認した。

一方、

- IPアドレス
- TCPポート番号
- TCP Flags
- パケットサイズ

などは暗号化されず、
パケットキャプチャから確認できた。

### TLS

tsharkを使用して、

- ClientHello
- ServerHello
- Cipher Suites
- supported_groups
- signature_algorithms
- ALPN
- supported_versions
- Application Data

を確認した。

### Certificates and Keys

TLS通信で使用される、

- 証明書
- 公開鍵
- 秘密鍵
- 鍵交換
- 共通鍵

の役割を整理した。

また、

    実際に観測した情報

と、

    TLS仕様上の仕組み

を分けて扱う重要性を確認した。

## この章で身についたこと

この章を通して、

- パケットキャプチャの読み方
- プロトコル階層の考え方
- IP / TCPヘッダの基本的な読み方
- HTTP通信の解析
- HTTPとHTTPSの違い
- TLSハンドシェイクの基本
- 暗号化通信でも確認できる情報
- 証明書と暗号鍵の役割
- 観測事実と推測を区別する考え方

を学んだ。

## Status

`03-packet-analysis` 完了。

## Next Chapter

[04-ids-suricata](../04-ids-suricata/)

次の章では、
Suricataを使用して、

- IDSの基本
- Suricataのインストールと設定
- ルール
- ポートスキャンの検知
- HTTP / TLS通信イベント
- アラートログ

について学習する。
