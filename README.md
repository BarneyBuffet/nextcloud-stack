# Nextcloud Stack

Docker compose stack for running Nextcloud with Cloudflare (reverse proxy) and Watchtower (automatic image updates)

## Environmental Variables

Environmental config variables should be set in `.env`. An example environmental config can be found at `env.example`.

Image versions are not set by default so the latest container image is pulled. Alternative versions can be set in the `.env` file for more granularity.

## Redis Database

To use Redis database, edit `/config/www/nextcloud/config/config.php` to include:

```php
'redis' => [
    'host' => 'redis',
    'port' => 6379,
],
'memcache.locking' => '\OC\Memcache\Redis',
```

## Trusted Domains and Reverse Proxy (Cloudflare)

To reverse proxy your FQN domain needs to be added as a trusted_domain in `/config/www/nextcloud/config/config.php`

```php
'trusted_domains' =>
[
    'localhost:3443',
    'nextcloud.domain.com'
],
```

## Increase PHP Memory Usage

To help with larger file sizes, lets increase PHP memory by adding the below to `/www/nextcloud/.htaccess`

```properties
php_value upload_max_filesize 16G
php_value post_max_size 16G
php_value max_input_time 3600
php_value max_execution_time 3600
php_value memory_limit 2048M
```

## SMTP Settings

Gmail can be used for SMTP (Nextcloud emails) using the below settings. App specific password should be generated in Gmail.

* **Send Mode:** SMTP
* **Encryption:** SSL/TLS
* **From:** nextcloud@mydomain.com
* **Authentication method:** Login
* **Authentication Required:** Yes
* **Server address:** smtp.gmail.com:465
* **Credentials:** gmailusername:gmail-app-password-in-settings

## Cloudflare

Use internal docker container names for FQN in cloudflare tunnel service URL.

## References

* [NextCloud with CloudFlare Tunnels](https://dbt3ch.com/books/nextcloud-with-cloudflare-tunnels/page/nextcloud-with-cloudflare-tunnels)