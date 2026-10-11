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
2. [Suricata Custom Rules](./02-suricata-rules.md)

## 現在までに確認したこと

### Suricata Setup

- Suricata 7.0.3の動作を確認
- `HOME_NET` を `192.168.10.0/24` に設定
- `ens33` を監視対象に設定
- `suricata-update` で検知ルールを取得
- `suricata -T` で設定テストに成功
- `eve.json` にイベントが記録されることを確認
- HTTP通信を自動解析できることを確認

### Custom Rules

- `local.rules` を作成
- `rule-files` に自作ルールを追加
- ICMP Echo Requestを検知するルールを作成
- `itype:8` でping要求を指定
- `sid:1000001` を自作ルールIDとして設定
- 自作ルールを含む設定テストに成功
- KaliからUbuntuへのpingを実行
- `LAB ICMP Ping detected` のアラートを確認
- `fast.log` に検知結果が記録されることを確認
- ping 3回に対してアラートも3回発生することを確認

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

[03-port-scan-detection](./03-port-scan-detection.md)

として、
Nmapによる複数ポートへのアクセスを
Suricataで検知する。

これまでのポートスキャン実験と
IDSによる検知をつなげ、

    Scan
        ↓
    Detection
        ↓
    Alert

という流れを確認する。
