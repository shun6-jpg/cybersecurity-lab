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
6. [UDP Scanとclosed・open|filteredの比較](./06-udp-scan.md)
7. [UDPサービスとopenポートの確認](./07-udp-open-service.md)
8. [複数ポートへのSYN Scanと通常通信の比較](./08-multi-port-syn-scan.md)

## 現在までに確認したこと

- `ss` を使用してUbuntu Server内部の待受ポートを確認した
- Kali LinuxからNmapを使用して外部から見えるポートを確認した
- `nmap -sV` でサービスとソフトウェア情報を確認した
- UFWによってポートが `filtered` になることを確認した
- TCP SYN Scanを `tcpdump` で観察した
- TCPの `open`・`closed`・`filtered` をパケットレベルで比較した
- TCP Connect ScanとTCP SYN Scanの違いを確認した
- UDPにはTCPのような3-way handshakeが存在しないことを確認した
- UDPのclosedポートではICMP Port Unreachableが返ることを確認した
- UDPで応答がない場合に `open|filtered` と判定されることを確認した
- UDPサービスが応答を返した場合に `open` と判定されることを確認した
- UDPの `closed`・`open|filtered`・`open` を実際の通信で比較した
- 複数ポートへのSYN Scanでは短時間に多数のSYNが送信されることを確認した
- 応答がないポートに対してNmapがSYNを再送する場合があることを確認した
- SYN Scanと通常のSSH接続ではTCP接続開始時の動作が異なることを確認した
- ポートスキャン検出では単一パケットではなく通信パターンを見ることが重要だと確認した
- `ss`・UFW・tcpdump・Nmapを組み合わせてポート状態を確認した

## Next

次回はこれまで使用したNmapの代表的なスキャン方式と、
各ポート状態の判定方法を整理する。

そのまとめをもって、
`02-port-scanning` 章を一区切りとする。
