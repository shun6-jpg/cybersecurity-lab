# ARP and Neighbor Table

## 目的

Kali LinuxとUbuntu Serverで `ip neigh` を実行し、
IPv4アドレスとMACアドレスの対応を確認する。

## Kali Linux

実行コマンド:

    ip neigh

Ubuntu Serverについて以下の情報を確認した。

- IPv4: `192.168.10.128`
- MAC Address: `00:0c:29:a0:32:13`
- State: `STALE`

## Ubuntu Server

実行コマンド:

    ip neigh

Kali Linuxについて以下の情報を確認した。

- IPv4: `192.168.10.129`
- MAC Address: `00:0c:29:d8:3a:ea`
- State: `STALE`

## ARP

同一LAN内でIPv4通信を行う場合、
IPアドレスだけでなく送信先のMACアドレスも必要になる。

ARP（Address Resolution Protocol）は、
IPv4アドレスに対応するMACアドレスを調べるために使用される。

今回、

`192.168.10.128`

に対応するMACアドレスとして、

`00:0c:29:a0:32:13`

を確認した。

また、

`192.168.10.129`

に対応するMACアドレスとして、

`00:0c:29:d8:3a:ea`

を確認した。

Linuxでは `ip neigh` を利用することで、
OSが保持しているNeighbor Tableを確認できる。

## Neighbor State

`ip neigh` ではNeighbor Entryの状態も確認できる。

- `REACHABLE`: 最近到達可能であることを確認済み
- `STALE`: MACアドレスの情報は残っているが、最近到達確認していない状態

`STALE` はエラーではない。

## OSI参照モデルとの関係

IPv4アドレスは主にOSI参照モデルのLayer 3に関係する。

MACアドレスはLayer 2に関係する。

ARPによって、

IPv4アドレス → MACアドレス

の対応を確認できる。

## 学んだこと

- `ip neigh` でNeighbor Tableを確認できる
- IPv4アドレスとMACアドレスには対応関係がある
- ARPはIPv4アドレスからMACアドレスを調べるために使われる
- `STALE` や `REACHABLE` はNeighbor Entryの状態を表す
