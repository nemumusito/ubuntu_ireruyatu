## youkenteigi

# Zabbix環境の要件定義

1. オペレーティングシステム
   - Ubuntu 20.04 LTS

2. Webサーバー
   - Apache2
     - モジュール: mod_php

3. プログラミング言語
   - PHP 7.4以上
     - 必要なモジュール: mysql, gd, bcmath, mbstring, xml, ldap

4. データベース
   - MySQL 8.0
     - Zabbix用データベース名: zabbix
     - Zabbix用ユーザー名: zabbix

5. Zabbixコンポーネント
   - Zabbix 5.4
     - サーバー
     - フロントエンド
     - エージェント

6. システムパッケージ
   - wget
   - tzdata
   - その他Zabbixの依存パッケージ

7. システム設定
   - タイムゾーン: Asia/Tokyo

8. セキュリティ
   - ファイアウォール設定
   - SELinux設定（必要に応じて）

9. パフォーマンス要件
   - CPU: 2コア以上推奨
   - メモリ: 4GB以上推奨
   - ストレージ: 50GB以上推奨（監視対象数と保存期間による）

10. ネットワーク要件
    - 固定IPアドレス
    - ポート開放: 80/443 (Web), 10050/10051 (Zabbix)

11. バックアップ要件
    - データベースの定期バックアップ
    - 設定ファイルのバックアップ

12. 監視要件
    - 監視対象デバイス数: XX台
    - 監視項目数: 約XX項目
    - データ保存期間: XX日

13. スケーラビリティ
    - 将来の拡張性を考慮したリソース設計

14. ドキュメント
    - インストールおよび設定手順書
    - 運用マニュアル

15. トレーニング
    - 管理者向けZabbix操作トレーニング

16. サポート
    - 技術サポート体制の確立



# プロジェクト名

## 1_ubuntu_installation

# Ubuntu 20.04 LTSのインストール

1. Ubuntu 20.04 LTSのISOイメージをダウンロードする
   - Ubuntu公式サイトから最新のLTSバージョンをダウンロード

2. ブートメディアを作成する
   - USBメモリまたはDVDにISOイメージを書き込む

3. ターゲットマシンを起動し、インストールメディアから起動する

4. 言語を選択し、「Ubuntu をインストール」を選択

5. キーボードレイアウトを選択

6. インストールタイプを選択（通常は「通常のインストール」）

7. インストール先のディスクを選択し、パーティションを設定

8. タイムゾーンを選択（Asia/Tokyo）

9. ユーザー名とパスワードを設定

10. インストールが完了したら、システムを再起動

11. 初回ログイン後、以下のコマンドでシステムを更新：
    ```
    sudo apt update
    sudo apt upgrade -y
    ```

12. 必要に応じて、SSHサーバーをインストールしリモートアクセスを設定
    ```
    sudo apt install openssh-server
    ```
## 2_apache_installation

# Apache2 Webサーバーのインストールと設定

Apache2は、オープンソースのWebサーバーソフトウェアです。このセクションでは、Apache2をインストールし、基本的な設定を行います。これにより、Webアプリケーションをホストし、インターネット上で公開することができるようになります。

1. Apache2をインストール
   ```
   sudo apt install apache2
   ```

2. Apache2サービスを起動し、自動起動を有効化
   ```
   sudo systemctl start apache2
   sudo systemctl enable apache2
   ```

3. ファイアウォールの設定（必要に応じて）
   ```
   sudo ufw allow 'Apache'
   ```

4. Apache2の動作確認
   - ブラウザでサーバーのIPアドレスにアクセスし、Apacheのデフォルトページが表示されることを確認

5. Apache2の設定ファイルを編集（必要に応じて）
   ```
   sudo nano /etc/apache2/apache2.conf
   ```

6. バーチャルホストの設定（必要に応じて）
   ```
   sudo nano /etc/apache2/sites-available/zabbix.conf
   ```

7. 設定変更後、Apache2を再起動
   ```
   sudo systemctl restart apache2
   ```
## 3_php_installation

# PHP 7.4以上のインストールと設定

このセクションでは、Zabbixの動作に必要なPHP 7.4以上をインストールし、適切に設定します。PHPはZabbixのウェブインターフェースを動作させるために不可欠です。

1. PHP 7.4とZabbixに必要なモジュールをインストール
   ```
   sudo apt install php7.4 php7.4-mysql php7.4-gd php7.4-bcmath php7.4-mbstring php7.4-xml php7.4-ldap
   ```

2. PHPの設定ファイルを編集（必要に応じて）
   ```
   sudo nano /etc/php/7.4/apache2/php.ini
   ```

3. 主な設定項目の確認と調整
   - memory_limit = 128M
   - upload_max_filesize = 16M
   - post_max_size = 16M
   - max_execution_time = 300
   - max_input_time = 300
   - date.timezone = Asia/Tokyo

4. Apache2のPHPモジュールを有効化
   ```
   sudo a2enmod php7.4
   ```

5. Apache2を再起動してPHPの変更を反映
   ```
   sudo systemctl restart apache2
   ```

6. PHPの動作確認
   ```
   echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/phpinfo.php
   ```
   ブラウザで`http://サーバーのIPアドレス/phpinfo.php`にアクセスしPHPの情報ページを確認

7. 確認後、セキュリティのためphpinfo.phpファイルを削除
   ```
   sudo rm /var/www/html/phpinfo.php
   ```
## 4_mysql_installation

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
## 5_zabbix_installation

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
## 6_system_packages

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
