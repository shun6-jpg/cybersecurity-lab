# IP Address and Network Interface

## 目的

Kali LinuxとUbuntu Serverで `ip addr` を実行し、
IPアドレスとネットワークインターフェースを確認する。

## Kali Linux

実行コマンド:

    ip addr

確認結果:

- Interface: `eth0`
- IPv4: `192.168.10.129/24`
- Loopback Interface: `lo`
- Loopback Address: `127.0.0.1`

## Ubuntu Server

実行コマンド:

    ip addr

確認結果:

- Interface: `ens33`
- IPv4: `192.168.10.128/24`
- Loopback Interface: `lo`
- Loopback Address: `127.0.0.1`

## 確認できたこと

Kali LinuxとUbuntu Serverは異なるIPv4アドレスを持っているが、
どちらも `192.168.10.0/24` に所属している。

`/24` はCIDR表記で、
サブネットマスク `255.255.255.0` に相当する。

今回のネットワークでは、

- Network Address: `192.168.10.0`
- Host Range: `192.168.10.1` ～ `192.168.10.254`
- Broadcast Address: `192.168.10.255`

となる。

また、Kali Linuxでは `eth0`、
Ubuntu Serverでは `ens33` がネットワークインターフェースとして使用されている。

## 学んだこと

- `ip addr` でIPアドレスを確認できる
- Linuxではネットワークインターフェースに名前が付いている
- `lo` はLoopback Interface
- `127.0.0.1` は自分自身を表すLoopback Address
- `/24` はCIDR表記
