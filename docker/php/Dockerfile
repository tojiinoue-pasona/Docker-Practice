# PHP-FPM用のDockerfile
# Apache版から変更点：php:8.2-apache → php:8.2-fpm
FROM php:8.2-fpm

# PHP拡張インストール（SQLite用）
# ※Apache版と同じ拡張が必要
RUN apt-get update && apt-get install -y \
    zip unzip git curl libzip-dev libonig-dev sqlite3 libsqlite3-dev \
    && docker-php-ext-install pdo pdo_sqlite mbstring zip

# Composerインストール
# ※これもApache版と同じ（PHP依存関係のため）
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 作業ディレクトリ
# ※Nginxとファイル共有するため同じパスを使用
WORKDIR /var/www/html

# アプリケーションのコピー
# ※Apache版と同じ（Laravelアプリケーション）
COPY src/ .

# .envファイルの準備（.env.exampleからコピー）
# ※Laravel設定のため必須
RUN cp .env.example .env

# Composer依存関係インストール
# ※Apache版と同じ（PHP依存関係）
RUN composer install --no-dev --optimize-autoloader --no-scripts

# ストレージ権限設定
# ※重要：PHP-FPMはwww-dataユーザーで実行されるため権限設定が必須
RUN mkdir -p storage/logs \
    && chown -R www-data:www-data storage \
    && chmod -R 775 storage \
    && chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html/storage

# PHP-FPMのポート公開
# ※Apache版はHTTP(80)だったが、PHP-FPMは内部通信用(9000)
EXPOSE 9000

# Laravel初期化とPHP-FPM起動
# ※重要な変更点：apache2-foreground → php-fpm
# php-fpmはバックグラウンドで動作し、FastCGIプロトコルでNginxと通信
CMD ["sh", "-c", "touch database/database.sqlite && php artisan key:generate --force && php artisan migrate:fresh --seed --force && php-fpm"]
