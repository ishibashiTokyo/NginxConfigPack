# Nginx server configs

Nginx の設定ファイル

BASIC 認証が設定されているので公開時はコメントアウトする。

## オレオレ証明書発行

1. 秘密鍵の作成

   ```shell
   $ openssl genrsa 2048 > /etc/nginx/ssl/server.key
   ```

1. CSR の作成

   暫定で IP アドレスを使用して発行

   ```shell
   $ openssl req -new -key /etc/nginx/ssl/server.key > /etc/nginx/ssl/server.csr
   ...
   Country Name (2 letter code) [XX]:JP
   State or Province Name (full name) []:Tokyo
   Locality Name (eg, city) [Default City]:xxxxx
   Organization Name (eg, company) [Default Company Ltd]:saku
   Organizational Unit Name (eg, section) []:
   Common Name (eg, your name or your servers hostname) []:{IP_ADDRESS}
   Email Address []:

   Please enter the following 'extra' attributes
   to be sent with your certificate request
   A challenge password []:
   An optional company name []:
   ```

1. 10 年でフィンガープリント

   ```shell
   $ openssl x509 -days 3650 -req -signkey /etc/nginx/ssl/server.key < /etc/nginx/ssl/server.csr > /etc/nginx/ssl/server.crt
   ```

## CMS 導入

### phpMyAdmin

```shell
$ wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz -P /var/www
$ tar zxvf /var/www/phpMyAdmin-latest-all-languages.tar.gz
$ mv phpMyAdmin-[0-9]* phpMyAdmin
```

### WordPress

```shell
$ wget https://ja.wordpress.org/latest-ja.tar.gz -P /var/www
$ tar zxvf /var/www/latest-ja.tar.gz
$ rm -rf /var/www/html | mv /var/www/wordpress /var/www/html
$ sudo chown -R nginx: /var/www/html
```
