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