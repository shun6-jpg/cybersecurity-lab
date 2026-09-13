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
4. [closedポートとfilteredポートのSYN Scan比較](./04-closed-port-syn-scan.md)
5. [TCP Connect ScanとSYN Scanの比較](./05-tcp-connect-vs-syn-scan.md)

## 現在までに確認したこと

- `ss` を使用してUbuntu Server内部の待受ポートを確認した
- Kali LinuxからNmapを使用して外部から見えるポートを確認した
- `nmap -sV` でサービスとソフトウェア情報を確認した
- UFWによってポートが `filtered` になることを確認した
- UFWで通信を許可すると `filtered` から `open` に変化することを確認した
- TCP SYN Scanの通信を `tcpdump` で観察した
- openなポートでは `SYN → SYN/ACK → RST` が発生することを確認した
- closedなポートでは `SYN → RST/ACK` が発生することを確認した
- filteredなポートではSYNに対する応答が返らない場合があることを確認した
- Nmapの `open`・`closed`・`filtered` の判定をパケットレベルで比較した
- TCP Connect Scanでは通常の3-way handshakeが完了することを確認した
- TCP Connect ScanとTCP SYN Scanでは、同じ `open` 判定でも通信方法が異なることを確認した

## Next

次はUDP Scanを実行し、
TCPスキャンとの違いを確認する。

UDPではTCPのような3-way handshakeが存在しないため、
Nmapがどのようにポート状態を判断するのかを観察する。
