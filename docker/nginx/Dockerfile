# Nginx用のDockerfile
# 軽量なAlpine Linuxベースを使用（本番環境でよく使われる）
FROM nginx:alpine

# カスタム設定ファイルをコピー
# デフォルトのnginx.confを上書きして、Laravel用設定を適用
# ※重要：context: . なので、ルートディレクトリからの相対パスで指定
COPY docker/nginx/laravel.conf /etc/nginx/conf.d/default.conf

# HTTPポート公開（お客さんがアクセスするポート）
EXPOSE 80

# Nginxをフォアグラウンドで起動
# daemon off; により、Dockerコンテナが終了しないように設定
CMD ["nginx", "-g", "daemon off;"]
