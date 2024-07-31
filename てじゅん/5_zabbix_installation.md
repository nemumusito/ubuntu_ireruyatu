# Zabbix 5.4のインストールと設定

このセクションでは、Zabbix 5.4をUbuntu 20.04 LTSにインストールし、基本的な設定を行います。Zabbixは、ネットワークとアプリケーションの監視を行うためのオープンソースソフトウェアです。以下の手順で、Zabbixサーバー、フロントエンド、エージェントのインストールから初期設定までを行います。

1. Zabbixリポジトリを追加
   ```
   wget https://repo.zabbix.com/zabbix/5.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.4-1+ubuntu20.04_all.deb
   sudo dpkg -i zabbix-release_5.4-1+ubuntu20.04_all.deb
   sudo apt update
   ```

2. Zabbixサーバー、フロントエンド、エージェントをインストール
   ```
   sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent
   ```

3. Zabbixデータベースのスキーマとデータをインポート
   ```
   sudo zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | sudo mysql -u zabbix -p zabbix
   ```

4. Zabbixサーバーの設定ファイルを編集
   ```
   sudo nano /etc/zabbix/zabbix_server.conf
   ```
   以下の行を編集または追加：
   ```
   DBPassword=Zabbixユーザーのパスワード
   ```

5. Zabbixサーバーとエージェントを起動し、自動起動を有効化
   ```
   sudo systemctl restart zabbix-server zabbix-agent apache2
   sudo systemctl enable zabbix-server zabbix-agent apache2
   ```

6. Apacheの設定ファイルを編集（必要に応じて）
   ```
   sudo nano /etc/apache2/conf-enabled/zabbix.conf
   ```

7. ブラウザでZabbixフロントエンドにアクセス
   `http://サーバーのIPアドレス/zabbix`

8. Zabbixのセットアップウィザードに従って初期設定を完了
   - データベース接続の設定
   - サーバーの詳細設定
   - GUI設定
   - インストール前の概要確認

9. Zabbixの管理者アカウントでログイン（デフォルト：Admin/zabbix）

10. パスワードを変更し、初期設定を行う