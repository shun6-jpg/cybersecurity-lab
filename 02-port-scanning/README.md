# Port Scanning / ポートスキャン

Kali LinuxとUbuntu Serverを使用して、
ポートスキャン、Firewall、サービス検出などを実際に検証した記録です。

サーバー内部から見える待受ポートと、
ネットワーク越しに見えるポートの違いや、
Nmapがポートの状態をどのように判断しているかを確認しました。

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
9. [Nmapスキャン方式とポート状態のまとめ](./09-nmap-scan-summary.md)

## この章で確認したこと

- `ss` を使用してサーバー内部の待受ポートを確認した
- Nmapを使用してネットワーク越しに見えるポートを確認した
- TCPの `open`・`closed`・`filtered` を比較した
- TCP SYN Scanのパケットをtcpdumpで確認した
- TCP Connect ScanとTCP SYN Scanの違いを確認した
- UFWによってポート状態が外部からどのように見えるかを確認した
- UDP Scanの `open`・`closed`・`open|filtered` を比較した
- UDPサービスを実際に起動して `open` を確認した
- `nmap -sV` によるサービス・バージョン検出を確認した
- TCPとUDPでは同じポート番号でも別々の状態を持つことを確認した
- 応答がないポートに対するNmapの再送を確認した
- 複数ポートへのSYN Scanを受信側から観察した
- 通常のSSH接続とSYN Scanの通信パターンを比較した
- ポートスキャンの検出では単一パケットではなく通信パターンを見ることが重要だと確認した
- `ss`・Nmap・tcpdump・UFWを組み合わせて通信状態を調査した

## Chapter Summary

ポートスキャンでは、
単に「ポート番号を調べる」のではなく、
対象から返ってくるパケットを利用して状態を判断している。

TCPでは、

    SYN/ACK
    RST/ACK
    no response

などから状態を判断できる。

一方UDPではTCPの3-way handshakeが存在しないため、
応答がない場合に `open` と `filtered` を区別できないことがある。

また、
サーバー内部でサービスが待ち受けていても、
Firewallによって通信が遮断されていれば
外部からの見え方は変化する。

この章では、
攻撃側からNmapを実行するだけではなく、
tcpdumpを使用して受信側からも通信を観察した。

これにより、
Nmapの表示結果と実際のパケットの動きを関連付けて理解した。

## Next Chapter

次は、

[03-packet-analysis](../03-packet-analysis/)

で、
ネットワークを流れるパケットや各プロトコルの内容を
より詳しく解析する。
