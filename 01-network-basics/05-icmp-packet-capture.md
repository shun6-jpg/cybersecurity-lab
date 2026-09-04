# ICMP Packet Capture with tcpdump

## 目的

`tcpdump`　を使用してICMP通信をキャプチャし、
Kali LinuxからUbuntu Serverへ　`ping`　を実行した際に、
ICMP Echo RequestとICMP Echo Replyが実際に発生することを確認する。

前回はARPによってIPv4アドレスからMACアドレスを取得する通信を確認した。

今回は、その後に行われる実際のIP通信を観察する。

## 環境

- Kali Linux
  - IPv4: `192.168.10.129`
  - Interface: `eth0`

- Ubuntu Server
  - IPv4: `192.168.10.128`
  - Interface: `ens33`

- VMware Host-only Network
  - Network: `192.168.10.0/24`
 
## 1. Ubunutu Serverで以下のコマンドを実行した。

    sudo tcpdump -ni ens33 icmp

オプションの意味:

 - `-n`: IPアドレスを名前解決せず、そのまま表示する
 - `-i ens33`: `ens33` インターフェースを監視する
 - `icmp`: ICMP通信だけを表示する
 
## 2. Kali Linuxからpingを実行

Kali LinuxからUbuntu Serverへ1回だけpingを実行した。

    ping -c 1 192.168.10.128

### Kali Linuxでの実行結果

![Kali Linux ping result](./imgaes/05-kali-ping.png)

結果:

- 1 packet transmitted
- 1 received
- 0% packet loss
- ttl=64
- time=0.507 ms

Ubuntu Serverへの疎通が成功した。

## 3. tcpdumpでICMP通信を確認

Ubuntu Server側のtcpdumpで以下の通信を確認した。

![Ubuntu tcpdump ICMP capture](./images/05-ubuntu-tcpdump-icmp)

主な結果:

    IP 192.168.10.129 > 192.168.10.128: ICMP echo request, id 31864, seq 1, length 64

    IP 192.168.10.128 > 192.168.10.129: ICMP echo reply, id 31864, seq 1, length 64

## 4. ICMP Echo Request

以下の通信を確認した。

    IP 192.168.10.129 > 192.168.10.128: ICMP echo request

送信元:

`192.168.10.129`

Kali Linux

送信先:

`192.168.10.128`

Ubuntu Server

つまり、Kali LinuxがUbunutu Serverに対して
ICMP echo Requestを送信している。

## 5. ICMP echo Reply

続いて以下の通信を確認した。

    192.168.10.128 > 192.168.10.129: ICMP echo reply

送信元:

`192.168.10.128`

Ubuntu Server

送信先:

`192.168.10.129`

Kali Linux

Ubuntu ServerがEcho Requestを受信し、
Kali LinuxへEcho Replyを返している。

このReplyを受信できたため、
Kali Linux側ではpingが成功した。

## 6 . idとseq

tcpdumpでは以下の情報も確認できた。

    id 31684, seq 1

`id`　は、どのping処理に属するICMP通信か識別するために利用される。

`seq`　はEcho Requestの通し番号である。

今回は　`ping -c 1`　を使用したため、
1回だけ送信され、　`seq 1`　となっている。

RequestとReplyで同じidとseqを確認できた。

## 7. パケットサイズ

Kali Linuxのpingでは以下の表示を確認した。

    56(84) bytes of data

tcpdumpでは以下を確認した。

    length 64

今回のpingではデータ部分が56 bytesである。

ICMPには8 bytesのヘッダが存在するため、

56 bytes (データ)
+
8 bytes (ICMP Header)
=
64 bytes

となる。

さらにIPv4 Headerの20 bytesを加えると、

64 bytes
+
20 bytes (IPv4 Header)
=
84 bytes

となる。

そのため、Kali Linuxのpingで表示された `56(84)` と
tcpdumpの　`length 64` は矛盾していない。

## 8. 前回のARPとの関係

前回の実験ではARP通信を確認した。

Kali LinuxがUbuntu Serverへ同一LAN内で通信する際、
必要に応じてまずARPを使用して送信先のMACアドレスを取得する。

通信の流れは以下のようになる。

IPv4アドレスを指定

`192.168.10.128`

↓

ARP Request

↓

ARP Reply

↓

Ubuntu ServerのMACアドレスを取得

↓

Ethernet Frameを送信可能になる

↓

ICMP Echo Request

↓

ICMP Echo Reply

今回の実験によって、
前回確認したARPの後にICMP通信が行われることを確認できた。

## 9. OSI参照モデルとの関係

今回確認したICMPとIPv4は、
主にOSI参照モデルのLayer 3（Network Layer）に関係する。

一方、前回確認したMACアドレスやEthernetは
Layer 2（Data Link Layer）に関係する。

同一LAN内の通信では、

Layer 3:

IPv4 / ICMP

と、

Layer 2:

Ethernet / MAC Address

が組み合わされて通信が行われる。

## 学んだこと

- `tcpdump` を使用してICMP通信を直接観察できる
- `ping` ではICMP Echo RequestとEcho Replyが使用される
- `>` の左右を見ることで送信元IPと送信先IPを確認できる
- Echo RequestはKali LinuxからUbuntu Serverへ送信された
- Echo ReplyはUbuntu ServerからKali Linuxへ返された
- RequestとReplyでは同じidとseqが対応している
- pingの `56(84)` とtcpdumpの `length 64` はパケット構造の違いによる表示である
- ARPでMACアドレスを取得した後、ICMPによるIP通信が行われることを実際に確認できた

## Next

次はTCP通信を観察し、
TCP 3-way handshakeの

SYN

SYN/ACK

ACK

をtcpdumpで実際に確認する。
