# 必要なシステムパッケージのインストール

このセクションでは、Zabbixサーバーの運用に必要な基本的なシステムパッケージのインストールと設定を行います。これには、ネットワークツール、時刻同期、セキュリティ設定、およびシステム全体の更新が含まれます。

1. wgetのインストール（既にインストールされている可能性あり）
   ```
   sudo apt install wget
   ```

2. tzdataのインストールと設定
   ```
   sudo apt install tzdata
   ```
   インストール中にタイムゾーンの選択を求められたら、「Asia」→「Tokyo」を選択

3. その他の必要なパッケージをインストール
   ```
   sudo apt install curl gnupg2 ca-certificates lsb-release
   ```

4. システムの更新と不要なパッケージの削除
   ```
   sudo apt update
   sudo apt upgrade -y
   sudo apt autoremove -y
   ```

5. NTPサーバーの設定（時刻同期のため）
   ```
   sudo apt install ntp
   sudo systemctl start ntp
   sudo systemctl enable ntp
   ```

6. ファイアウォールの設定（必要に応じて）
   ```
   sudo ufw allow 22/tcp  # SSH
   sudo ufw allow 80/tcp  # HTTP
   sudo ufw allow 443/tcp # HTTPS
   sudo ufw allow 10050/tcp  # Zabbix Agent
   sudo ufw allow 10051/tcp  # Zabbix Server
   sudo ufw enable
   ```

7. システムのホスト名を設定（必要に応じて）
   ```
   sudo hostnamectl set-hostname zabbix-server
   ```

8. /etc/hosts ファイルを編集してホスト名を追加
   ```
   sudo nano /etc/hosts
   ```
   以下の行を追加：
   ```
   127.0.1.1 zabbix-server
   ```

9. システムを再起動して変更を適用
   ```
   sudo reboot
   ```