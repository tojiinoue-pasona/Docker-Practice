# 🐳 Laravel Docker Practice

[![Docker](https://img.shields.io/badge/Docker-20.10%2B-blue?logo=docker)](https://www.docker.com/)
[![Laravel](https://img.shields.io/badge/Laravel-9.x-red?logo=laravel)](https://laravel.com/)
[![PHP](https://img.shields.io/badge/PHP-8.2-purple?logo=php)](https://www.php.net/)
[![Nginx](https://img.shields.io/badge/Nginx-Latest-green?logo=nginx)](https://nginx.org/)

DockerとNginx + PHP-FPM構成を使用したLaravel学習・練習プロジェクトです。
チーム開発に適した環境構築とドキュメント整備を重視した構成になっています。

---

## 📋 プロジェクト概要

### 🎯 目的
- **Docker学習**: コンテナ技術の実践的学習
- **Laravel開発**: 認証・CRUD機能を含むWebアプリケーション開発
- **チーム開発**: 統一された開発環境とドキュメント整備
- **アーキテクチャ学習**: Nginx + PHP-FPM の高性能Web構成

### 🏗 アーキテクチャ
```
┌─────────────────┐    ┌─────────────────┐
│     Nginx       │────│    PHP-FPM      │
│  (Web Server)   │    │   (Laravel)     │
│   Port: 8000    │    │   Port: 9000    │
└─────────────────┘    └─────────────────┘
         │                       │
         └───────────┬───────────┘
                     │
           ┌─────────────────┐
           │   SQLite DB     │
           │ (database.sqlite)│
           └─────────────────┘
```

---

## 🚀 クイックスタート

### 前提条件
- **Docker Desktop** がインストール済み
- **Git** がインストール済み

### 1. リポジトリクローン
```bash
git clone https://github.com/tojiinoue-pasona/Docker-Practice.git
cd Docker-Practice
```

### 2. 環境ファイル準備
```bash
cd src
cp .env.example .env
cd ..
```

### 3. Docker環境構築・起動
```bash
# 初回ビルド＆起動
docker compose up --build

# バックグラウンド起動
docker compose up --build -d
```

### 4. アプリケーションアクセス
ブラウザで以下にアクセス：
```
http://localhost:8000
```

**🎉 Laravelウェルカムページが表示されれば成功！**

---

## 📁 プロジェクト構造

```
Docker-Practice/
├── 📁 docker/                    # Docker設定ファイル
│   ├── 📁 nginx/                 # Nginx設定
│   │   ├── Dockerfile           # Nginxコンテナ定義
│   │   └── laravel.conf         # Laravel用Nginx設定
│   └── 📁 php/                   # PHP-FPM設定
│       └── Dockerfile           # PHP-FPMコンテナ定義
├── 📁 src/                       # Laravelアプリケーション
│   ├── 📁 app/                   # アプリケーションロジック
│   ├── 📁 database/              # DB関連（マイグレーション・シーダー）
│   ├── 📁 resources/             # ビュー・CSS・JS
│   ├── 📁 routes/                # ルーティング定義
│   └── 📄 .env.example          # 環境設定テンプレート
├── 📄 docker-compose.yml        # Docker構成定義
├── 📄 README.md                 # このファイル
├── 📄 README_NEW_MEMBER.md      # 新メンバー向け詳細ガイド
└── 📄 SETUP_GUIDE.md            # エンタープライズレベルセットアップガイド
```

---

## 🛠 開発機能

### 実装済み機能
- ✅ **ユーザー認証** (Laravel Breeze使用)
  - ユーザー登録・ログイン・ログアウト
  - パスワードリセット機能
- ✅ **連絡先管理システム**
  - CRUD操作 (作成・参照・更新・削除)
  - バリデーション
- ✅ **データベース設計**
  - ユーザー・連絡先・エリア・ルート・ショップテーブル
  - リレーション設定
- ✅ **フロントエンド**
  - TailwindCSS によるレスポンシブデザイン
  - Vite による高速ビルド

### 技術スタック

| カテゴリ | 技術 | バージョン | 用途 |
|----------|------|-----------|------|
| **Backend** | PHP | 8.2 | サーバーサイド言語 |
| | Laravel | 9.x | PHPフレームワーク |
| | SQLite | 3.x | データベース |
| **Frontend** | TailwindCSS | Latest | CSSフレームワーク |
| | Vite | Latest | ビルドツール |
| | Blade | - | テンプレートエンジン |
| **Infrastructure** | Docker | 20.10+ | コンテナ化 |
| | Nginx | Latest | Webサーバー |
| | PHP-FPM | 8.2 | プロセス管理 |

---

## 🎯 学習ポイント

### 1. Docker学習
- **マルチステージビルド**: 効率的なイメージ作成
- **サービス間通信**: Nginx ↔ PHP-FPM間のFastCGI通信
- **ボリューム管理**: ホスト-コンテナ間のファイル共有
- **ネットワーク設定**: Docker内部ネットワークによるサービス連携

### 2. Webアーキテクチャ学習
```bash
# リクエストの流れ
Browser → Nginx (Port 8000) → PHP-FPM (Port 9000) → Laravel → SQLite
```

### 3. Laravel学習
- **MVC アーキテクチャ**: Model-View-Controller パターン
- **認証システム**: Laravel Breeze による実装
- **データベース操作**: Eloquent ORM を使用
- **バリデーション**: フォーム入力検証

---

## 📚 ドキュメント

このプロジェクトには、レベル別に詳細なドキュメントを用意しています：

### 🔰 初心者向け
- **[README_NEW_MEMBER.md](README_NEW_MEMBER.md)** (13KB)
  - ステップ・バイ・ステップガイド
  - 豊富なスクリーンショット
  - よくある質問（FAQ）
  - トラブルシューティング

### 🎓 上級者向け
- **[SETUP_GUIDE.md](SETUP_GUIDE.md)** (565行)
  - エンタープライズレベルの詳細セットアップ
  - マルチOS対応 (macOS/Windows/Linux)
  - パフォーマンス最適化
  - プロダクション環境準備

---

## 🔧 日常的な開発コマンド

### 基本操作
```bash
# 環境起動
docker compose up -d

# 環境停止
docker compose down

# ログ確認
docker compose logs -f

# 環境状態確認
docker compose ps
```

### Laravel開発
```bash
# マイグレーション実行
docker exec laravel-php-fpm php artisan migrate

# データベースリセット
docker exec laravel-php-fpm php artisan migrate:fresh --seed

# キャッシュクリア
docker exec laravel-php-fpm php artisan cache:clear

# 新しいコントローラー作成
docker exec laravel-php-fpm php artisan make:controller ExampleController
```

### フロントエンド開発
```bash
# 依存関係インストール
cd src && npm install

# 開発モード（変更監視）
cd src && npm run dev

# 本番ビルド
cd src && npm run build
```

---

## 🧪 テスト

```bash
# PHPUnitテスト実行
docker exec laravel-php-fpm php artisan test

# 特定のテストクラスのみ実行
docker exec laravel-php-fpm php artisan test --filter=ContactFormTest
```

---

## 🚨 トラブルシューティング

### よくある問題

#### ❌ ポート8000が使用中
```bash
# 解決法
docker compose down
lsof -i :8000  # 使用中プロセスを確認
```

#### ❌ Laravel App Keyエラー
```bash
# 解決法
docker exec laravel-php-fpm php artisan key:generate
```

#### ❌ npm install エラー
```bash
# 解決法
cd src
rm -rf node_modules package-lock.json
npm install
```

詳細なトラブルシューティングは [SETUP_GUIDE.md](SETUP_GUIDE.md) を参照してください。

---

## 🤝 コントリビューション

### 開発に参加するには

1. **フォーク** このリポジトリをフォーク
2. **ブランチ作成** `git checkout -b feature/amazing-feature`
3. **コミット** `git commit -m 'Add some amazing feature'`
4. **プッシュ** `git push origin feature/amazing-feature`
5. **プルリクエスト** Pull Requestを作成

### コード規約
- **PHP**: PSR-12 準拠
- **JavaScript**: Prettier + ESLint
- **コミット**: [Conventional Commits](https://www.conventionalcommits.org/) 形式推奨

---

## 📄 ライセンス

このプロジェクトは [MIT License](https://opensource.org/licenses/MIT) の下で公開されています。

---

## 🙏 謝辞

- **Laravel Team** - 素晴らしいフレームワークの提供
- **Docker Community** - コンテナ技術の普及
- **Open Source Community** - 数多くのツールとライブラリ

---

## 📞 サポート・質問

- **Issues**: [GitHub Issues](https://github.com/tojiinoue-pasona/Docker-Practice/issues) で質問・バグ報告
- **Discussions**: プロジェクトに関する議論
- **Email**: 緊急時のお問い合わせ

---

## 📈 今後の予定

- [ ] **CI/CD パイプライン** 実装 (GitHub Actions)
- [ ] **API機能** 追加 (RESTful API)
- [ ] **テストカバレッジ** 向上
- [ ] **Docker最適化** (マルチステージビルド改善)
- [ ] **プロダクション環境** 対応ガイド

---

<div align="center">

**🚀 Happy Coding with Docker & Laravel! 🐳**

Made with ❤️ by [tojiinoue-pasona](https://github.com/tojiinoue-pasona)

</div>
