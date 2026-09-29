# Packet Analysis Summary

## 目的

`03-packet-analysis` で行った実験を振り返り、

- HTTP平文通信
- Ethernet / IP / TCP / HTTPの階層
- 16進数によるパケット解析
- HTTPとHTTPSの違い
- TLSハンドシェイク
- 証明書・公開鍵・秘密鍵・共通鍵

について整理する。

この章を通して、
ネットワーク上を流れる通信を
パケット単位で観察し、
各プロトコルの役割を関連付けて理解することを目的とした。

## 環境

- Windows 11 Host
- VMware Workstation
- Kali Linux
  - IPv4: `192.168.10.129`
- Ubuntu Server 24.04 LTS
  - IPv4: `192.168.10.128`
- Host-only Network
  - `192.168.10.0/24`

使用した主なツール：

- tcpdump
- tshark
- curl
- Apache HTTP Server
- OpenSSL
- Python

## 1. HTTP平文通信

最初に、
Kali LinuxからUbuntu ServerのHTTPサーバーへアクセスし、
tcpdumpで通信を確認した。

HTTPでは、

    GET / HTTP/1.1

や、

    Host: 192.168.10.128
    User-Agent: curl
    Accept: */*

などのリクエストヘッダを
そのまま読み取ることができた。

レスポンス側では、

    HTTP/1.1 200 OK

やHTML本文も確認できた。

このことから、
HTTP通信ではアプリケーション層のデータが
平文で送信されることを確認した。

## 2. 通信の階層

HTTP通信を詳細に確認することで、

    Ethernet
        ↓
    IP
        ↓
    TCP
        ↓
    HTTP

という階層構造を確認した。

各層では、

Ethernet：
- MACアドレス

IP：
- 送信元IPアドレス
- 宛先IPアドレス
- TTL
- Protocol

TCP：
- 送信元ポート
- 宛先ポート
- Sequence Number
- Acknowledgment Number
- Flags

HTTP：
- GET
- HTTPヘッダ
- HTMLデータ

などの情報を持っていることを確認した。

## 3. 16進数によるパケット解析

`tcpdump -X` を使用し、
実際のパケットを16進数で確認した。

今回確認したパケットでは、

    45

からIPv4ヘッダが始まっていた。

最初の4はIPv4を表し、
5はIHLを表していた。

IHLが5であるため、

    5 × 4 = 20 bytes

となり、
IPv4ヘッダが20バイトであることを確認した。

IPv4ヘッダの後からTCPヘッダが始まり、
TCP Data Offsetを確認することで
TCPヘッダ長も判断できた。

今回のパケットでは、

    IP Header = 20 bytes
    TCP Header = 32 bytes

であり、

    20 + 32 = 52 bytes

となった。

そのため、
オフセット52からHTTPデータが始まっていた。

16進数では、

    47 45 54 20

が、

    GET 

を表していることを確認した。

## 4. HTTPとHTTPSの違い

HTTP通信では、

    GET / HTTP/1.1

やHTML本文を
パケットキャプチャから直接読み取ることができた。

一方、
HTTPS通信では、

    Ethernet
        ↓
    IP
        ↓
    TCP
        ↓
    TLS
        ↓
    HTTP

という構造になる。

HTTPS通信をtcpdumpで確認すると、

- IPアドレス
- TCPポート番号
- TCP Flags
- パケットサイズ

などは確認できた。

しかし、

    GET / HTTP/1.1

や、

    <h1>HTTPS TEST</h1>

などのHTTPデータは
平文では確認できなかった。

このことから、
TLSによってHTTPデータが暗号化されていることを確認した。

## 5. TLSハンドシェイク

tsharkを使用してTLS通信を解析した。

実際に、

    Client Hello

    Server Hello

    Application Data

を確認した。

ClientHelloでは、

- Cipher Suites
- supported_groups
- signature_algorithms
- ALPN
- supported_versions

などを確認した。

ALPNでは、

    h2
    http/1.1

が提示されていた。

これは、
クライアントがTLS上で使用可能な
アプリケーションプロトコル候補を
サーバーへ提示していることを表している。

また、
supported_versionsでは
TLS 1.3への対応を確認した。

## 6. TLS 1.3の互換性フィールド

ClientHelloでは、

    Version: TLS 1.2

のように表示される部分が存在した。

一方、

    supported_versions

ではTLS 1.3が確認できた。

TLS 1.3では、
古い実装との互換性のために
従来のVersionフィールドへ
TLS 1.2相当の値を残す仕組みがある。

実際に利用可能なTLSバージョンは、
supported_versionsなどによって
ネゴシエーションされる。

そのため、

    Version: TLS 1.2

と、

    TLS 1.3

が同時に表示されても
矛盾ではない。

## 7. 証明書と鍵

HTTPS通信では、
サーバー認証のために証明書が使用される。

今回のラボでは、

    cert.pem

と、

    key.pem

を作成した。

`cert.pem` は、
公開鍵やサーバーに関する情報を含む証明書である。

一方、

`key.pem` は秘密鍵であり、
外部へ公開してはいけない。

通常のWebサイトでは、
認証局が証明書へ署名する。

今回使用した証明書は
自己署名証明書であるため、

    curl -k

を使用して証明書検証を省略した。

ただし、
`-k` を使用しても
TLSによる暗号化自体が無効になるわけではない。

## 8. 公開鍵暗号と共通鍵暗号

HTTPSでは、
HTTPデータすべてを
公開鍵暗号だけで暗号化しているわけではない。

公開鍵暗号は一般に処理コストが高いため、

    証明書 / 公開鍵暗号
    → 認証や安全な鍵確立

    共通鍵暗号
    → 実際の大量データ通信

という役割分担が行われる。

TLSでは鍵交換によって
クライアントとサーバーが
同じ秘密情報を計算し、

そこから通信暗号化用の鍵を導出する。

その鍵を使用して、

    Application Data

を暗号化する。

## 9. Application Data

TLS通信では、
HTTPデータなどの実際の通信内容が

    Application Data

として送信される。

今回の通信では、
Application Dataの中に、

    GET / HTTP/1.1

や、

    <h1>HTTPS TEST</h1>

などのHTTPデータが含まれていた。

しかし、
TLSによって暗号化されているため、
パケットキャプチャから直接読むことはできなかった。

## 10. パケット解析で重要な考え方

今回の実験を通して、
パケット解析では

    実際に確認できた事実

と、

    プロトコル仕様上の仕組み

を分けて考えることが重要だと分かった。

例えば今回、

    key_share

については
実際の保存データから明確に確認できなかった。

そのため、

    key_shareを確認した

とは記録しなかった。

実験記録では、
観測していない内容を
観測した事実として扱わないことが重要である。

## 学んだこと

- HTTPでは通信内容を平文で確認できる
- Ethernet / IP / TCP / HTTPにはそれぞれ役割がある
- MACアドレスはEthernet層で使用される
- IPアドレスはIP層で使用される
- ポート番号はTCP層で使用される
- 16進数からIP・TCPヘッダの境界を確認できる
- TCP payloadとしてHTTPデータが運ばれる
- HTTPSではHTTPデータがTLSによって暗号化される
- HTTPSでもIPアドレスやTCPポート番号などは確認できる
- TLSではClientHelloとServerHelloで通信条件を調整する
- Cipher Suitesは暗号化方式などの候補を示す
- ALPNによってTLS上のアプリケーションプロトコルを選択できる
- 証明書には公開鍵などが含まれる
- 秘密鍵はサーバーのみで保持する
- 公開鍵暗号と共通鍵暗号には役割の違いがある
- Application Dataには暗号化されたHTTPデータなどが含まれる
- パケット解析では観測事実と推測を分ける必要がある

## Conclusion

この章では、

    通信が発生する
        ↓
    パケットを取得する
        ↓
    各プロトコルのヘッダを読む
        ↓
    アプリケーションデータを確認する
        ↓
    HTTPとHTTPSの違いを比較する
        ↓
    TLSの仕組みを理解する

という流れでパケット解析を行った。

これにより、
単に通信が成功したかどうかを見るだけでなく、

    ネットワーク上で
    実際に何が起きているのか

をパケット単位で確認できるようになった。

次の章では、
これまで観察してきた通信を対象に、

    正常な通信
    異常な通信
    攻撃的な通信

をIDSがどのように検知するのかを学ぶ。

## Next Chapter

[04-ids-suricata](../04-ids-suricata/)
