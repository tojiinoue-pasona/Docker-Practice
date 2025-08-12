# 🚀 ローカル開発環境構築ガイド

## 📋 概要

このガイドでは、Laravel Docker プロジェクトのローカル開発環境を構築する手順を詳しく説明します。
Docker + Nginx + PHP-FPM構成を使用し、チーム全体で統一された開発環境を提供します。

---

## 🛠 前提条件

### 対応OS
- ✅ **macOS** (推奨: macOS 12.0以降)
- ✅ **Windows** (Windows 10/11 + WSL2)
- ✅ **Linux** (Ubuntu 20.04以降推奨)

### 必要なソフトウェア
- **Docker Desktop** (必須)
- **Git** (必須)
- **Node.js** (v16以降推奨)

---

## 📦 1. Dockerのインストール

### macOS の場合

既にDockerがインストールされている場合は [2. プロジェクトセットアップ](#2-プロジェクトセットアップ) へ進んでください。

1. **Docker Desktop for Mac をダウンロード**
   - [公式サイト](https://docs.docker.com/desktop/install/mac-install/) からダウンロード
   - Apple Silicon (M1/M2) または Intel Mac を選択

2. **インストール実行**
   ```bash
   # ダウンロードした .dmg ファイルを開き、
   # Docker.app を Applications フォルダにドラッグ&ドロップ
   ```

3. **Docker Desktop を起動**
   ```bash
   # Applications から Docker を起動
   # 初回起動時は管理者権限の入力が必要
   ```

4. **インストール確認**
   ```bash
   # ターミナルで以下のコマンドを実行
   docker --version
   docker-compose --version
   
   # 期待される出力例:
   # Docker version 24.0.2, build cb74dfc
   # Docker Compose version v2.18.1
   ```

### Windows の場合

1. **WSL2 を有効化**
2. **Docker Desktop for Windows をインストール**
   - [公式サイト](https://docs.docker.com/desktop/install/windows-install/) からダウンロード

### Linux の場合

```bash
# Docker Engine をインストール
sudo apt-get update
sudo apt-get install docker.io docker-compose

# ユーザーを docker グループに追加
sudo usermod -aG docker $USER
```

---

## 📁 2. プロジェクトセットアップ

### 2-1. プロジェクトのクローン

```bash
# GitHubからプロジェクトをクローン
git clone https://github.com/tojiinoue-pasona/Docker-Practice.git

# プロジェクトディレクトリに移動
cd Docker-Practice
```

### 2-2. プロジェクト構成の確認

```bash
# プロジェクト構成を確認
ls -la

# 期待される出力:
# docker-compose.yml      # Docker構成定義
# docker/                 # Docker関連ファイル
# src/                    # Laravelアプリケーション
# README_NEW_MEMBER.md    # このガイド
```

---

## 🐳 3. Docker環境の構築

### 3-1. Laravel環境ファイルの準備

```bash
# srcディレクトリに移動
cd src

# 環境ファイルをコピー
cp .env.example .env

# プロジェクトルートに戻る
cd ..
```

### 3-2. Docker バージョンの確認と選択

Docker Compose は Docker CLI に統合されており、使用するコマンドが異なります：

#### 📊 Docker バージョンとコマンドの対応

| Docker Version | 推奨コマンド | サポート状況 |
|----------------|--------------|-------------|
| 20.10.0以降 | `docker compose` | ✅ 推奨 |
| 1.27.0-20.9.x | `docker-compose` | 🔶 レガシー |

#### バージョン確認方法

```bash
# Docker バージョンを確認
docker --version

# 出力例と対応コマンド:
# Docker version 24.0.2 → docker compose 使用
# Docker version 19.03.x → docker-compose 使用
```

### 3-3. Docker環境の初期化・構築

**※ 初回構築時間**: 約5-10分程度かかります

#### Docker 20.10.0以降の場合（推奨）

```bash
# 初回ビルド＆起動
docker compose up --build

# バックグラウンド起動する場合
docker compose up --build -d
```

#### Docker 20.10.0未満の場合（レガシー）

```bash
# 初回ビルド＆起動
docker-compose up --build

# バックグラウンド起動する場合
docker-compose up --build -d
```

### 3-4. 起動完了の確認

正常に起動すると以下のような出力が表示されます：

```bash
✅ Container laravel-php-fpm  Running
✅ Container laravel-nginx    Running

# ログに以下が表示されれば成功:
laravel-nginx    | ready for start up
laravel-php-fpm  | NOTICE: fpm is running, pid 1
```

---

## 🌐 4. アプリケーションの動作確認

### 4-1. Webアプリケーションへのアクセス

1. **ブラウザを開き、以下にアクセス**
   ```
   http://localhost:8000
   ```

2. **Laravelウェルカムページが表示されることを確認**

### 4-2. 主要機能のテスト

```bash
# 1. ユーザー登録ページ
http://localhost:8000/register

# 2. ログインページ
http://localhost:8000/login

# 3. 連絡先管理ページ（ログイン後）
http://localhost:8000/contacts
```

---

## 🎨 5. フロントエンド開発環境のセットアップ

### 5-1. Node.js依存関係のインストール

```bash
# srcディレクトリに移動
cd src

# npm依存関係をインストール
npm install

# 開発用ビルドを実行
npm run dev
```

### 5-2. フロントエンドの監視モード（開発時）

```bash
# CSS/JavaScriptの変更を監視
npm run dev

# または、本番用ビルド
npm run build
```

**注意**: CSS/JavaScriptファイルを修正した際は、必ず上記コマンドを再実行してください。

---

## 🐞 6. デバッグ・開発ツール

### 6-1. データベースの確認

**SQLite データベースファイル**:
```bash
# データベースファイルの場所
src/database/database.sqlite

# コンテナ内でSQLiteにアクセス
docker exec -it laravel-php-fpm sqlite3 database/database.sqlite
```

### 6-2. ログの確認方法

```bash
# 全サービスのログを確認
docker compose logs

# リアルタイムでログを監視
docker compose logs -f

# 特定サービスのログのみ
docker compose logs nginx    # Nginx
docker compose logs php      # PHP-FPM
```

### 6-3. コンテナ内での作業

```bash
# PHP-FPMコンテナにログイン
docker exec -it laravel-php-fpm bash

# コンテナ内でLaravelコマンドを実行
php artisan migrate
php artisan make:controller ExampleController
php artisan route:list
```

---

## 🔧 7. 日常的な開発ワークフロー

### 7-1. 作業開始時

```bash
# プロジェクトディレクトリに移動
cd Docker-Practice

# Docker環境を起動（2回目以降は高速）
docker compose up

# または、バックグラウンドで起動
docker compose up -d
```

### 7-2. 作業終了時

```bash
# Docker環境を停止
docker compose down

# または、Ctrl+C でも停止可能（前景起動の場合）
```

### 7-3. よく使用するコマンド

```bash
# ========================
# 基本操作
# ========================

# 環境の状態確認
docker compose ps

# コンテナの再起動
docker compose restart

# 特定サービスの再起動
docker compose restart nginx

# ========================
# Laravel開発
# ========================

# マイグレーション実行
docker exec laravel-php-fpm php artisan migrate

# データベースリセット（開発時）
docker exec laravel-php-fpm php artisan migrate:fresh --seed

# キャッシュクリア
docker exec laravel-php-fpm php artisan cache:clear

# ========================
# フロントエンド開発
# ========================

# CSS/JS の変更監視
cd src && npm run dev

# 本番用ビルド
cd src && npm run build
```

---

## 🚨 8. トラブルシューティング

### 8-1. よくある問題と解決法

#### ❌ **ポート8000が使用中エラー**
```bash
Error: Port 8000 is already in use
```

**解決法**:
```bash
# 使用中のプロセスを確認
lsof -i :8000

# 既存のDockerコンテナを停止
docker compose down

# または、別のポートを使用
# docker-compose.yml の ports を "8001:80" に変更
```

#### ❌ **Docker起動時のビルドエラー**
```bash
Error: failed to solve: no build definition
```

**解決法**:
```bash
# Dockerキャッシュをクリア
docker system prune -a

# 強制リビルド
docker compose up --build --no-cache
```

#### ❌ **Laravel App Keyエラー**
```bash
Error: No application encryption key has been specified
```

**解決法**:
```bash
# コンテナ内でキー生成
docker exec laravel-php-fpm php artisan key:generate
```

#### ❌ **npm installエラー**
```bash
Error: npm ERR! peer dep missing
```

**解決法**:
```bash
# srcディレクトリで依存関係を再インストール
cd src
rm -rf node_modules package-lock.json
npm install
```

### 8-2. コンフリクト解決

**同名Dockerイメージが複数存在する場合**:

```bash
# 未使用のDockerリソースをクリーンアップ
docker system prune -a

# 特定イメージを削除
docker images | grep "docker-practice"
docker rmi [IMAGE_ID]

# 再構築
docker compose up --build
```

---

## 🔧 9. PHP バージョンの管理（macOS + Homebrew）

### 9-1. 背景

ローカル開発時に、Docker内のPHP（8.2）とローカルのPHP（8.3等）のバージョン差によりエラーが発生することがあります。

### 9-2. PHP 8.2のインストール

```bash
# Homebrewで PHP 8.2 をインストール
brew install php@8.2

# 現在のPHPから切り替え
brew unlink php && brew link --overwrite --force php@8.2

# バージョン確認
php -v
# 期待される出力: PHP 8.2.x

# Composerの動作確認
composer --version
```

### 9-3. PHPバージョンの切り替え

```bash
# PHP 8.2 に切り替え（Docker環境に合わせる）
brew unlink php && brew link --overwrite --force php@8.2

# 最新PHPに戻す
brew unlink php@8.2 && brew link --overwrite --force php

# 使用可能なPHPバージョン確認
brew list | grep php
```

---

## 💻 10. 推奨開発ツール

### 10-1. VSCode拡張機能

#### 必須拡張機能
```bash
# Laravel関連
- Laravel Blade Snippets
- Laravel Artisan
- Laravel goto view

# Docker関連  
- Docker
- Remote-Containers

# PHP関連
- PHP Debug (xdebug.php-debug)
- PHP Intelephense
```

#### 推奨拡張機能
```bash
# フォーマット
- PHP CS Fixer (junstyle.php-cs-fixer)
- Prettier - Code formatter

# その他
- GitLens
- Auto Rename Tag
```

### 10-2. PHP CS Fixer設定

```bash
# PHP CS Fixerのインストール確認
php-cs-fixer -V

# 期待される出力:
# PHP CS Fixer 3.13.2 (3952f08) Oliva by Fabien Potencier and Dariusz Ruminski.
# PHP runtime: 8.2.x
```

**使用方法**:
- PHPファイルを開いて `Option + Shift + F` (Mac) でフォーマット実行

---

## 📈 11. パフォーマンス最適化

### 11-1. Docker性能向上

```bash
# Docker Desktop の設定で以下を調整:
# - Memory: 4GB以上を割り当て
# - CPU: 2コア以上を割り当て
# - Disk space: 十分な容量を確保
```

### 11-2. Laravel最適化

```bash
# 本番環境準備時のコマンド
docker exec laravel-php-fpm php artisan config:cache
docker exec laravel-php-fpm php artisan route:cache
docker exec laravel-php-fpm php artisan view:cache
```