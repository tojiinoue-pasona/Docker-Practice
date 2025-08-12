# 📚 Laravel Docker プロジェクト - 新規メンバー向けガイド

> **🎯 このガイドの目的**: 新しくチームに参加したメンバーが、スムーズに開発環境を構築し、プロジェクトの理解を深められるよう、1から丁寧に説明します。

---

## 📋 目次

1. [プロジェクト概要](#-プロジェクト概要)
2. [技術スタック](#-技術スタック)
3. [事前準備](#-事前準備)
4. [環境構築（初回セットアップ）](#-環境構築初回セットアップ)
5. [日常的な開発作業](#-日常的な開発作業)
6. [プロジェクト構成の理解](#-プロジェクト構成の理解)
7. [トラブルシューティング](#-トラブルシューティング)
8. [よくある質問](#-よくある質問)

---

## 🎯 プロジェクト概要

### このプロジェクトは何ですか？
**Laravel**を使用したWebアプリケーション開発プロジェクトです。
**Docker**を使用して開発環境を統一し、チーム全体で一貫した環境での開発を実現しています。

### 主な機能
- 📝 **連絡先管理機能**: ユーザーの連絡先情報の登録・編集・削除
- 🔐 **認証機能**: ユーザー登録・ログイン
- 📊 **データベース管理**: SQLite を使用したデータ永続化

---

## 🛠 技術スタック

### フロントエンド
- **Laravel Blade**: PHPテンプレートエンジン
- **Tailwind CSS**: CSSフレームワーク
- **Vite**: モダンなフロントエンドビルドツール

### バックエンド
- **PHP 8.2**: プログラミング言語
- **Laravel 10+**: PHPウェブアプリケーションフレームワーク
- **SQLite**: 軽量データベース

### インフラ・ツール
- **Docker & Docker Compose**: 開発環境コンテナ化
- **Nginx**: Webサーバー（高性能な静的ファイル配信）
- **PHP-FPM**: PHP実行環境（Nginxとの高速連携）

### アーキテクチャ
```
┌─────────────────┐    FastCGI    ┌─────────────────┐
│   Nginx         │◄─────────────►│   PHP-FPM       │
│   (Web Server)  │   Port 9000   │   (App Server)  │
│   Port 80       │               │   Laravel App   │
└─────────────────┘               └─────────────────┘
         │                                 │
         └────────── Volume Share ─────────┘
                  /var/www/html
```

---

## 🔧 事前準備

### 必要なソフトウェア

#### 1. **Docker Desktop** (必須)
- **Windows**: [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
- **Mac**: [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/)
- **Linux**: [Docker Engine](https://docs.docker.com/engine/install/)

**確認コマンド:**
```bash
docker --version
docker-compose --version
```

#### 2. **Git** (必須)
```bash
git --version
```

#### 3. **テキストエディタ** (推奨)
- **Visual Studio Code**: [ダウンロード](https://code.visualstudio.com/)
- その他: PhpStorm, Sublime Text など

---

## 🚀 環境構築（初回セットアップ）

### Step 1: リポジトリのクローン

```bash
# プロジェクトをクローン
git clone [リポジトリURL]
cd docker-practice
```

### Step 2: プロジェクト構成の確認

```bash
# プロジェクト構成を確認
ls -la

# 期待される出力例:
# docker-compose.yml      # Docker構成定義
# docker/                 # Docker関連ファイル
# src/                    # Laravelアプリケーション
```

### Step 3: Laravel環境ファイルの準備

```bash
# srcディレクトリに移動
cd src

# 設定ファイルのテンプレートをコピー
cp .env.example .env

# プロジェクトルートに戻る
cd ..
```

### Step 4: Docker環境の起動

```bash
# 初回ビルド & 起動（時間がかかります：5-10分程度）
docker-compose up --build

# 起動完了のサイン:
# ✅ "laravel-nginx" と "laravel-php-fpm" が Running 状態
# ✅ ログにエラーがない
```

### Step 5: アプリケーション動作確認

1. **ブラウザを開く**
2. **http://localhost:8000** にアクセス
3. **Laravelのウェルカムページ**が表示されることを確認

### Step 6: 主要機能のテスト

```bash
# 新しいターミナルタブを開き、以下をテスト:

# 1. ユーザー登録ページ
http://localhost:8000/register

# 2. ログインページ  
http://localhost:8000/login

# 3. 連絡先管理ページ（ログイン後）
http://localhost:8000/contacts
```

---

## 💻 日常的な開発作業

### 🌅 作業開始時

```bash
# プロジェクトディレクトリに移動
cd docker-practice

# Docker環境を起動（2回目以降は高速）
docker-compose up

# または、バックグラウンドで起動
docker-compose up -d
```

### 🌙 作業終了時

```bash
# Docker環境を停止
docker-compose down

# または、Ctrl+C でも停止可能（docker-compose up の場合）
```

### 📝 ファイル編集について

**重要**: ファイル編集は **`src/`** ディレクトリ内で行います。

```bash
src/
├── app/                    # Laravel アプリケーションロジック
│   ├── Models/            # データモデル
│   ├── Controllers/       # コントローラー
│   └── ...
├── resources/             # フロントエンドファイル
│   ├── views/            # Bladeテンプレート
│   ├── css/              # スタイルファイル
│   └── js/               # JavaScriptファイル
├── routes/               # ルーティング定義
│   └── web.php           # ウェブルート
└── database/             # データベース関連
    ├── migrations/       # データベースマイグレーション
    └── seeders/          # テストデータ
```

### 🔄 よく使うコマンド

```bash
# ========================
# 基本操作
# ========================

# 環境の状態確認
docker-compose ps

# ログの確認
docker-compose logs

# 特定サービスのログ確認  
docker-compose logs nginx
docker-compose logs php

# ========================
# Laravel操作（コンテナ内実行）
# ========================

# PHP-FPMコンテナに入る
docker exec -it laravel-php-fpm bash

# コンテナ内でのLaravelコマンド例:
# php artisan migrate        # データベースマイグレーション
# php artisan make:controller # コントローラー作成  
# php artisan route:list     # ルート一覧表示

# ========================
# データベース操作
# ========================

# マイグレーション実行（データベース更新）
docker exec laravel-php-fpm php artisan migrate

# データベースリセット（開発時のみ）
docker exec laravel-php-fpm php artisan migrate:fresh --seed
```

---

## 📁 プロジェクト構成の理解

### ディレクトリ構成

```
docker-practice/
├── 📁 docker/                    # Docker関連設定
│   ├── 📁 nginx/                # Nginx設定
│   │   ├── 📄 Dockerfile        # Nginxコンテナ定義
│   │   └── 📄 laravel.conf      # Laravel用Nginx設定
│   └── 📁 php/                  # PHP-FPM設定
│       └── 📄 Dockerfile        # PHP-FPMコンテナ定義
├── 📁 src/                      # ⭐ Laravelアプリケーション（主な作業場所）
│   ├── 📁 app/                  # アプリケーションロジック
│   ├── 📁 resources/            # フロントエンドファイル
│   ├── 📁 routes/               # ルーティング定義
│   ├── 📁 database/             # データベース関連
│   ├── 📄 .env                  # Laravel設定（Git管理外）
│   └── 📄 .env.example          # 設定テンプレート
├── 📄 docker-compose.yml        # Docker環境オーケストレーション
├── 📄 .gitignore               # Git除外設定
└── 📄 README_NEW_MEMBER.md      # このファイル
```

### 設定ファイルの役割

| ファイル | 役割 | 編集頻度 |
|----------|------|----------|
| `docker-compose.yml` | Docker環境全体の定義 | 🔶 低 |
| `docker/*/Dockerfile` | 各コンテナの構成定義 | 🔶 低 |
| `src/.env` | Laravel実行設定 | 🟡 中 |
| `src/` 内の各ファイル | アプリケーション開発 | ⭐ 高 |

---

## 🚨 トラブルシューティング

### よくある問題と解決法

#### 1. **ポート8000が使用中エラー**
```bash
Error: Port 8000 is already in use
```
**解決法:**
```bash
# 使用中のプロセスを確認
lsof -i :8000

# 既存のDockerコンテナを停止
docker-compose down

# または、別のポートを使用
# docker-compose.yml の ports を "8001:80" に変更
```

#### 2. **Docker起動時のビルドエラー**
```bash
Error: failed to solve: no build definition
```
**解決法:**
```bash
# Dockerキャッシュをクリア
docker system prune -a

# 再ビルド
docker-compose up --build --no-cache
```

#### 3. **Laravel App Keyエラー**
```bash
Error: No application encryption key has been specified
```
**解決法:**
```bash
# コンテナ内でキー生成
docker exec laravel-php-fpm php artisan key:generate
```

#### 4. **データベース関連エラー**
```bash
Error: SQLSTATE[HY000] [14] unable to open database file
```
**解決法:**
```bash
# データベースファイル作成 & マイグレーション
docker exec laravel-php-fpm touch database/database.sqlite
docker exec laravel-php-fpm php artisan migrate
```

#### 5. **パーミッション関連エラー**
```bash
Error: Permission denied
```
**解決法:**
```bash
# コンテナ内の権限修正
docker exec laravel-php-fpm chown -R www-data:www-data storage
docker exec laravel-php-fpm chmod -R 775 storage
```

### 🔍 ログの確認方法

```bash
# 全サービスのログ
docker-compose logs

# リアルタイムログ監視
docker-compose logs -f

# 特定サービスのログ
docker-compose logs nginx    # Nginx
docker-compose logs php      # PHP-FPM
```

---

## ❓ よくある質問

### Q1. **なぜDockerを使うの？**
**A:** チーム全員が同じ環境で開発できるため、「私の環境では動くのに...」という問題を避けられます。また、新しいメンバーの環境構築時間を大幅短縮できます。

### Q2. **Apache版とNginx版の違いは？**
**A:** 
- **Apache版**: シンプルな単一コンテナ構成
- **Nginx版**: 高性能な2コンテナ構成（Nginx + PHP-FPM）

本番環境により近い構成で学習できるため、Nginx版を採用しています。

### Q3. **データベースにMySQLではなくSQLiteを使用している理由は？**
**A:** 
- 学習目的に最適（設定が簡単）
- ファイルベースのため管理が容易
- 小〜中規模プロジェクトに適している

### Q4. **ファイルを編集しても変更が反映されない**
**A:** 
- **確認事項**: `src/` ディレクトリ内で編集していますか？
- **解決法**: ブラウザのキャッシュをクリア（Ctrl+Shift+R / Cmd+Shift+R）

### Q6. **プロジェクトルートに.envがないのはなぜ？**
**A:** 
- 設定をシンプルに保つため
- Laravel用の設定（`src/.env`）のみを使用
- 混乱を避けるため不要な設定ファイルは削除

### Q7. **本番環境への展開は？**
**A:** 
- 現在の構成は開発環境用
- 本番展開時は追加のセキュリティ設定が必要
- プロジェクトリーダーに相談してください