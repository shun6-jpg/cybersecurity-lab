# IDS / Suricata

Suricataを使用して、
ネットワーク通信の解析・検知・アラート生成を学ぶ実験記録です。

これまでの章では、

- ネットワーク基礎
- ポートスキャン
- パケット解析
- HTTP / HTTPS
- TLS

について、
tcpdumpやtsharkを使用して通信を観察しました。

この章では、
これまで人間が確認していた通信を
IDSがどのように自動解析・検知するのかを確認します。

## Environment / 環境

- Windows 11 Host
- VMware Workstation
- Kali Linux
  - `192.168.10.129`
- Ubuntu Server 24.04 LTS
  - `192.168.10.128`
- Host-only Network
  - `192.168.10.0/24`

Ubuntu Server上でSuricataを動作させ、
`ens33` を監視します。

## Chapter Structure

1. Suricataの導入・基本設定
2. Suricataルール
3. ポートスキャン検知
4. HTTP / TLSイベント
5. 章まとめ

## Records / 実験記録

1. [Suricata Setup and HTTP Event Detection](./01-suricata-setup.md)

## 現在までに確認したこと

- Suricata 7.0.3がインストールされていることを確認
- `HOME_NET` を `192.168.10.0/24` に設定
- `ens33` を監視インターフェースに設定
- `suricata-update` で検知ルールを取得
- 約53,000ルールが有効化された
- `suricata -T` で設定テストに成功
- Suricata Engineの起動を確認
- `eve.json` の生成を確認
- DHCP / DNS / Flow / HTTPなどのイベントを確認
- KaliからのHTTP通信をSuricataが自動解析することを確認
- HTTP Method、URL、User-Agent、Statusなどを確認
- `fast.log` にアラートがない場合でも通信解析は行われていることを確認

## この章の目標

この章では、

- IDSの基本を理解する
- Suricataを設定する
- Suricataルールを理解する
- 独自ルールを作成する
- アラートを発生させる
- ポートスキャンを検知する
- HTTP / TLS通信を解析する
- Suricataログを読む

ことを目標とする。

最終的には、

    Network Traffic
        ↓
    Suricata
        ↓
    Protocol Analysis
        ↓
    Rule Matching
        ↓
    Alert / Event Log

というIDSの基本的な流れを理解する。

## Next

次回は、

[02-suricata-rules](./02-suricata-rules.md)

として、
Suricata独自ルールを作成する。

単に通信を解析するだけでなく、

    条件に一致した通信を検知
        ↓
    alert

までを実際に確認する。
