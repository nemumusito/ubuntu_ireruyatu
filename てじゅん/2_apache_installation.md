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