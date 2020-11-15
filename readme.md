# Ubuntu 20.04 - PHP 7.3 - NGINX - ionCube Loader Kurulumu

Bu döküman Ubuntu 20.04 üzerinde nginx ve php7.3 konfigürasyonuna sahip sunucularda ionCube Loader'ın kurulumu ile ilgili yönergeleri içermektedir.

## Adım 1 - ionCube Loader İndirme

```sh
$ wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
$ tar xzf ioncube_loaders_lin_x86-64.tar.gz -C /usr/local
```

## Adım 2 - PHP.ini Dizinini Bulma

```sh
$ php -i | grep php.ini
```
##### Örnek Çıktı:
```sh
Configuration File (php.ini) Path => /etc/php/7.4/cli
Loaded Configuration File => /etc/php/7.4/cli/php.ini
```

## Adım 3 - PHP.ini ionCube Loader Konfigürasyonu
fpm/ ve cli/ dizinlerinde bulunan php.ini dosyalarına aşağıdaki satır eklenmelidir:

```sh
zend_extension = /usr/local/ioncube/ioncube_loader_lin_7.4.so
```

## Adım 4 - Kurulum Doğrulama
Doğrulama için öncelikle php-fpm ve nginx servisleri yeniden başlatılır ve status kodlarının 'active' olduğu doğrulanır:

### PHP.ini Doğrulama:

```sh
sudo service php7.3-fpm restart
sudo service php7.3-fpm status
```

##### Örnek Çıktı
```sh
● php7.3-fpm.service - The PHP 7.3 FastCGI Process Manager
     Loaded: loaded (/lib/systemd/system/php7.3-fpm.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2020-11-15 12:39:02 UTC; 16s ago
       Docs: man:php-fpm7.3(8)
    Process: 3079 ExecStartPost=/usr/lib/php/php-fpm-socket-helper install /run/php/php-fpm.sock /etc/php/7.3/fpm/pool.d/www.conf 73 (code=exited, status=0/SUCCESS)
   Main PID: 3076 (php-fpm7.3)
     Status: "Processes active: 0, idle: 2, Requests: 0, slow: 0, Traffic: 0req/sec"
      Tasks: 3 (limit: 1075)
     Memory: 12.0M
     CGroup: /system.slice/php7.3-fpm.service
             ├─3076 php-fpm: master process (/etc/php/7.3/fpm/php-fpm.conf)
             ├─3077 php-fpm: pool www
             └─3078 php-fpm: pool www

Nov 15 12:39:02 test systemd[1]: Starting The PHP 7.3 FastCGI Process Manager...
Nov 15 12:39:02 test systemd[1]: Started The PHP 7.3 FastCGI Process Manager.
```


### NGINX Doğrulama:

```sh
sudo service nginx restart
sudo service nginx status
```

##### Örnek Çıktı
```sh
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2020-11-15 12:27:02 UTC; 16min ago
       Docs: man:nginx(8)
    Process: 2987 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2998 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 2999 (nginx)
      Tasks: 2 (limit: 1075)
     Memory: 3.2M
     CGroup: /system.slice/nginx.service
             ├─2999 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             └─3000 nginx: worker process

Nov 15 12:27:02 test systemd[1]: Starting A high performance web server and a reverse proxy server...
Nov 15 12:27:02 test systemd[1]: Started A high performance web server and a reverse proxy server.
```


Çıktılar bu şekilde ise ionCube Loader kurulumu tamamlanmıştır.