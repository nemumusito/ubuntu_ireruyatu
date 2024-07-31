# MySQL 8.0データベースのインストールと設定

このセクションでは、Zabbixで使用するMySQL 8.0データベースのインストール、セキュリティ設定、およびZabbix用のデータベースとユーザーの作成を行います。これにより、Zabbixシステムのデータを安全に保存し管理するための環境を整えます。

1. MySQL 8.0をインストール
   ```
   sudo apt install mysql-server
   ```

2. MySQLサービスを起動し、自動起動を有効化
   ```
   sudo systemctl start mysql
   sudo systemctl enable mysql
   ```

3. MySQLのセキュリティ設定を実行
   ```
   sudo mysql_secure_installation
   ```
   - rootパスワードの設定
   - 匿名ユーザーの削除
   - リモートrootログインの無効化
   - テストデータベースの削除
   - 権限テーブルのリロード

4. Zabbix用のデータベースとユーザーを作成
   ```
   sudo mysql -u root -p
   ```
   MySQLプロンプトで以下のコマンドを実行：
   ```sql
   CREATE DATABASE zabbix CHARACTER SET utf8 COLLATE utf8_bin;
   CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'パスワード';
   GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

5. MySQLの設定ファイルを編集（必要に応じて）
   ```
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

6. 設定変更後、MySQLを再起動
   ```
   sudo systemctl restart mysql
   ```