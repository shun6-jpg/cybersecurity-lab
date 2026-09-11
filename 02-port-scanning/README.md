# Port Scanning / ポートスキャン

Kali LinuxとUbuntu Serverを使用して、
ポートスキャン、Firewall、サービス検出などを実際に検証した記録です。

サーバー内部から見える待受ポートと、
ネットワーク越しに見えるポートの違いや、
Nmapがポートの状態をどのように判断しているかを確認しています。

## Environment / 環境

- Kali Linux
- Ubuntu Server 24.04 LTS
- VMware Workstation
- Host-only Network: `192.168.10.0/24`

## Records / 実験記録

1. [待受ポートとNmapによるサービス検出](./01-port-scan-and-service-detection.md)
2. [Firewallとfilteredポート](./02-firewall-and-filtered-ports.md)
3. [TCP SYN Scanのパケット解析](./03-syn-scan-packet-analysis.md)

## 現在までに確認したこと

- `ss` を使用してUbuntu Server内部の待受ポートを確認した
- Kali LinuxからNmapを使用して外部から見えるポートを確認した
- `nmap -sV` でサービスとソフトウェア情報を確認した
- UFWによってポートが `filtered` になることを確認した
- UFWで通信を許可すると `filtered` から `open` に変化することを確認した
- TCP SYN Scanの通信を `tcpdump` で観察した
- openなポートでは `SYN → SYN/ACK → RST` が発生することを確認した
- 通常のTCP 3-way handshakeとSYN Scanの違いを確認した

## Next

次はclosedポートに対してTCP SYN Scanを実行し、
openポートの場合とパケットの応答を比較する。

その後、必要に応じて `open`・`closed`・`filtered` の違いを整理する。
