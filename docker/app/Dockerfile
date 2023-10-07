# RockeyLinux8のOSをインストール
FROM rockylinux/rockylinux:8
RUN dnf -y update

# PHP
RUN dnf -y module enable php:7.4 -y

# PHPとWebサーバの設定ファイルを読み込ませる
ADD php.ini /etc/
ADD httpd.conf /etc/httpd/conf/
ADD 00-mpm.conf /etc/httpd/conf.modules.d/

# PHPのパッケージ系のインストール
RUN dnf -y install php php-mbstring php-pear php-mysqlnd php-gd php-pecl-zip php-json php-curl php-zip php-opcache

# Apache
RUN dnf -y install httpd
EXPOSE 80

# composer
RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer

# Node.js
RUN dnf -y install nodejs
RUN dnf -y install npm

# その他ツール
RUN dnf -y install \
git \
zip \
unzip \
vim

# Laravel系の処理とcomposerの実行
RUN cd /var/www && composer global require "laravel/installer" && composer create-project --prefer-dist laravel/laravel anonymous-board "8.*"
RUN chown -R apache:apache /var/www/anonymous-board
RUN chmod -R 777 /var/www/anonymous-board/storage
RUN cd /var/www/anonymous-board/ && composer require laravelcollective/html

# supervisor経由でApacheをデーモン化して起動
RUN dnf -y install epel-release
RUN dnf -y install supervisor.noarch

ADD supervisord.conf /etc/supervisord.conf

CMD ["/usr/bin/supervisord"]