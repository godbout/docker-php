# PHP

A PHP image extended to bundle libraries commonly used in PHP projects. This image
does not contain Apache, just psysh. The following libraires are included:

* apcu
* calendar
* Core
* ctype
* curl
* date
* dom
* fileinfo
* filter
* ftp
* gd
* hash
* iconv
* json
* ldap
* libxml
* mbstring
* memcached
* mysqli
* mysqlnd
* openssl
* pcre
* PDO
* pdo_mysql
* pdo_sqlite
* Phar
* posix
* readline
* Reflection
* session
* SimpleXML
* sodium
* SPL
* sqlite3
* standard
* tokenizer
* xdebug
* xml
* xmlreader
* xmlwriter
* zip
* zlib

## Usage

### Simple command line shell

```
docker run --rm -v /path/tiki-source:/var/www/html -w /var/www/html -it tikiwiki/php:7.1-cli psysh

Psy Shell v0.9.9 (PHP 7.1.30 — cli) by Justin Hileman
>>> include 'tiki-setup.php'
>>> 
```


### Development with Atom and php-debug

When creating your container, enable XDebug by setting env variable `XDEBUG`

```
docker run --rm -e XDEBUG=1 -v /path/tiki-source:/var/www/html -w /var/www/html -it tikiwiki/php:7.1-cli psysh
```

Tell your Atom about folder mapping by editin `config.cson`. You have to configure the
section called "php-debug". It should look like this:

```
  "php-debug":
    PathMaps: [
      "remotepath;localpath"
      "/var/www/html/;/path/tiki-source"
    ]
    PhpException:
      CatchableFatalError: false
      Deprecated: false
      FatalError: false
      Notice: false
      ParseError: false
      StrictStandards: false
      UnknownError: false
      Warning: false
      Xdebug: false
    ServerPort: 9000
```


### Extending with new configurations

You can extend this image with a Dockerfile. Imagine you want to let your user
upload 2GB files.


```
FROM tikiwiki/php:7.1-cli
LABEL mantainer "Some Developer <developer@example.com>"

RUN {                                      \
        echo "file_uploads = On";          \
        echo "upload_max_filesize = 2048M";\
        echo "post_max_size = 2048M";      \
        echo "max_file_uploads = 20";      \
    } > /usr/local/etc/php/conf.d/docker-max-upload.ini

USER root
```
