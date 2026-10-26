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
3. [Port Scan Detection with Suricata](./03-port-scan-detection.md)

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
- 自作ルールをSuricataへ読み込ませた
- ICMP Echo Requestを検知するルールを作成
- KaliからUbuntuへのpingを検知
- `fast.log` に自作アラートが記録されることを確認

### Port Scan Detection

- TCP SYNを対象とするルールを作成
- `flags:S` でSYNパケットを指定
- `flow:stateless` を使用
- `threshold` による回数ベースの検知を設定
- `track by_src` で送信元IPごとに追跡
- 10秒以内に5回以上のSYNを検知条件とした
- KaliからNmap SYNスキャンを実行
- Suricataで `LAB Possible TCP SYN Port Scan` を検知
- Nmap側のスキャン結果とIDS側のアラートを対応させた
- `tee` によるログ表示と保存を確認
- thresholdの値によって誤検知とのバランスが変わることを理解した

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

[04-http-tls-events](./04-http-tls-events.md)

として、
SuricataがHTTPとTLS通信から
どのような情報を抽出できるか確認する。

HTTP平文通信とTLS暗号化通信を比較し、

    暗号化される情報
    暗号化されても観測できる情報

をIDS側から整理する。
