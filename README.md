# 🐳 Docker学習記録：Laravel実践開発

[![Docker](https://img.shields.io/badge/Docker-20.10%2B-blue?logo=docker)](https://www.docker.com/)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-2.0%2B-blue?logo=docker)](https://docs.docker.com/compose/)
[![Laravel](https://img.shields.io/badge/Laravel-9.x-red?logo=laravel)](https://laravel.com/)
[![PHP](https://img.shields.io/badge/PHP-8.2-purple?logo=php)](https://www.php.net/)
[![Nginx](https://img.shields.io/badge/Nginx-Latest-green?logo=nginx)](https://nginx.org/)

**Docker技術を実践的に学習した成果**として作成したLaravel Webアプリケーション開発プロジェクトです。  
コンテナ技術・マイクロサービス・インフラ自動化などのモダン開発手法の学習過程を記録しています。

---

## 🎯 この学習プロジェクトで習得したこと

### 🐳 **Docker技術の実践習得**
- **コンテナ化技術**: アプリケーションの完全な環境分離の実装
- **マルチコンテナ構成**: Nginx + PHP-FPM の実践的アーキテクチャ構築
- **Docker Compose**: 複数サービスのオーケストレーション管理
- **ボリューム管理**: データ永続化とファイル共有の仕組み実装
- **ネットワーク設定**: サービス間通信とセキュリティ設定

### 🏗️ **習得したインフラ実践知識**
- **リバースプロキシ**: Nginxによる高性能Web配信の構築
- **FastCGI通信**: Nginx ↔ PHP-FPMプロセス間通信の実装
- **環境分離**: 開発・本番環境の完全な統一化
- **運用管理**: ログ管理・デバッグ・トラブルシューティング経験

---

## 🏗 Docker アーキテクチャ詳細

### システム構成図
```
🌐 外部リクエスト (localhost:8000)
         ↓
┌─────────────────────────┐
│    Nginx Container      │  ← Webサーバー・リバースプロキシ
│    ポート: 80           │  ← 静的ファイル配信 (CSS/JS/画像)
│    役割: フロントエンド  │
└────────────┬────────────┘
             │ FastCGI通信 (ポート9000)
             ↓
┌─────────────────────────┐
│   PHP-FPM Container     │  ← PHPプロセス管理
│   ポート: 9000          │  ← Laravel実行環境
│   役割: バックエンド     │
└────────────┬────────────┘
             │
             ↓
┌─────────────────────────┐
│    SQLite Database      │  ← データ永続化
│    ファイル形式         │  ← /var/www/html/database/
└─────────────────────────┘
```

### 🔍 Docker学習ポイント
```yaml
# docker-compose.yml で学ぶ重要概念

services:
  nginx:
    # 1. カスタムイメージビルド
    build:
      context: .              # ← ビルドコンテキスト
      dockerfile: docker/nginx/Dockerfile
    
    # 2. ポートマッピング
    ports:
      - "8000:80"            # ← ホスト:コンテナ
    
    # 3. ボリュームマウント
    volumes:
      - ./src:/var/www/html  # ← ファイル共有
    
    # 4. サービス依存関係
    depends_on:
      - php                  # ← 起動順序制御
    
  php:
    # 5. 内部ネットワーク通信
    expose:
      - "9000"               # ← 内部ポート公開
```

---

## 🚀 学習実践の記録

### **Phase 1**: Docker基礎環境構築
```bash
# 学習開始時に実践した手順
git clone https://github.com/tojiinoue-pasona/Docker-Practice.git
cd Docker-Practice

# 環境設定ファイルの理解と準備
cd src && cp .env.example .env && cd ..

# 初回Docker環境構築の実践
docker compose up --build
```

### **Phase 2**: コンテナ理解と運用習得
```bash
# 学習過程で実践したコンテナ管理
docker compose ps

# 実際に取得した結果:
# NAME              COMMAND               STATUS
# laravel-nginx     "nginx -g 'daemon…"  Up
# laravel-php-fpm   "docker-php-entryp…" Up

# コンテナ内部調査の実習
docker exec -it laravel-php-fpm bash  # PHP環境での作業経験
docker exec -it laravel-nginx bash    # Nginx環境での設定確認
```

### **Phase 3**: Docker Compose運用マスター
```bash
# 学習を通じて習得した運用コマンド
docker compose restart nginx    # 個別サービス管理
docker compose logs -f php     # リアルタイムログ監視

# 実践で身についたメンテナンス操作
docker compose down            # 環境停止管理
docker compose down -v         # データ管理の理解
docker system prune -a         # システムメンテナンス
```

---

## 📁 学習成果として構築したプロジェクト構造

```
Docker-Practice/
├── 🐳 docker/                    # Docker設定（学習で構築）
│   ├── 📁 nginx/
│   │   ├── Dockerfile           # 🔧 Nginx環境設定（手作り）
│   │   └── laravel.conf         # ⚙️ FastCGI通信設定（学習成果）
│   └── 📁 php/
│       └── Dockerfile           # 🔧 PHP-FPM環境設定（実装）
│
├── 🐳 docker-compose.yml        # 🎼 マルチコンテナ管理（習得技術）
│
├── 📁 src/                       # Laravel実装（アプリ開発経験）
│   ├── 📁 app/                   # ビジネスロジック実装
│   ├── 📁 database/              # SQLite DB設計・構築
│   └── 📄 .env.example          # 環境変数管理（設定理解）
│
└── 📚 学習記録ドキュメント/
    ├── README.md                # このファイル（学習成果まとめ）
    ├── README_NEW_MEMBER.md     # 学習過程で作成したガイド
    └── SETUP_GUIDE.md           # 実践で得た知識のドキュメント化
```

---

## 🎯 学習過程で習得したDocker技術

### **習得レベル 1: Docker基礎** ✅
- [x] Dockerfileの読み書きを習得
- [x] イメージとコンテナの概念を理解・実践
- [x] ボリュームマウントの仕組みを実装
- [x] ポートマッピングを理解・設定

**実際に実践したこと**:
```bash
# Dockerfileの理解と実装
cat docker/php/Dockerfile      # PHP環境の構築方法を学習
cat docker/nginx/Dockerfile    # Nginx環境の設定方法を習得

# コンテナ管理の実践
docker compose ps              # 運用状況の監視方法を習得
docker compose top             # プロセス管理の理解
```

### **習得レベル 2: Docker Compose実践** ✅
- [x] サービス間通信の理解・実装
- [x] depends_onとlinksの使い分けを習得
- [x] 環境変数の管理方法を実践
- [x] ログ管理とデバッグ手法を習得

**実際に実践したこと**:
```bash
# サービス間通信の検証
docker exec laravel-nginx ping php       # 通信確認を実践
docker exec laravel-php-fpm ping nginx   # 双方向通信の理解

# ログ管理・デバッグの実践
docker compose logs nginx | grep error   # エラー解析スキル習得
docker compose logs php | grep "NOTICE"  # 動作確認方法の習得
```

### **習得レベル 3: Docker運用** ✅
- [x] パフォーマンス最適化を実践
- [x] セキュリティ設定を理解・実装
- [x] 実用的な運用管理を経験
- [x] トラブルシューティング能力を獲得

**実際に実践したこと**:
```bash
# 運用最適化の実践
docker images | grep docker-practice     # イメージサイズ管理
docker system df                         # リソース使用量監視

# セキュリティ確認の実施
docker exec laravel-php-fpm whoami       # ユーザー権限確認
docker exec laravel-nginx ls -la /var/www/html  # ファイル権限確認
```

---

## 🛠 習得したDocker運用ワークフロー

### **実際に身についた日常運用操作**
```bash
# 🌅 開発作業開始時のルーティン
docker compose up -d        # 高速バックグラウンド起動

# 🔧 開発中の監視・管理
docker compose logs -f      # リアルタイムログ監視
docker compose restart php # 設定変更後の個別再起動

# 🌙 作業終了時のクリーンアップ
docker compose down         # 全サービス安全停止
```

### **Laravel開発×Docker統合運用の習得**
```bash
# 学習を通じて身につけたコンテナ内作業
docker exec laravel-php-fpm php artisan migrate           # DB操作
docker exec laravel-php-fpm php artisan make:controller TestController  # 開発作業
docker exec laravel-php-fpm php artisan route:list        # 動作確認

# パッケージ管理の実践
docker exec laravel-php-fpm composer install              # 依存関係管理
docker exec laravel-php-fpm composer require package-name # 新機能追加
```

---

## 🎨 学習成果：機能実装とDocker技術の融合

### **実装した機能と習得したDocker技術**
| 実装機能 | 習得したDocker技術 | 学習成果・実践ポイント |
|------|---------------|-------------|
| **Web表示** | Nginxコンテナ運用 | 静的ファイル配信・リバースプロキシ設定 |
| **認証機能** | PHP-FPMコンテナ管理 | アプリケーション処理・セッション管理 |
| **データベース** | ボリュームマウント | データ永続化・バックアップ戦略の実践 |
| **CSS/JS** | ファイル共有機能 | アセット管理・開発効率化 |

### **習得技術スタック × Docker学習成果**
| カテゴリ | 使用技術 | Docker学習での習得内容 |
|----------|------|-------------------|
| **Container** | Docker | コンテナ化技術の実践的習得 |
| **Orchestration** | Docker Compose | マルチコンテナ管理の運用経験 |
| **Web Server** | Nginx | リバースプロキシ・負荷分散の実装 |
| **App Server** | PHP-FPM | プロセス管理・スケーリングの理解 |
| **Framework** | Laravel | 実用的Webアプリケーション開発 |

---

## 🚨 学習過程で遭遇・解決したDocker問題

### **実際に経験したDocker問題と解決経験**

#### 🐳 **コンテナ起動失敗の解決経験**
```bash
# 学習中に実際に使用した問題診断方法
docker compose ps              # サービス状態の確認手法習得
docker compose logs [service]  # エラーログ解析能力獲得

# 実践で身につけた解決方法
# 1. ポート競合問題の解決
lsof -i :8000                 # ポート使用状況確認技術習得
docker compose down           # 安全な環境停止方法習得

# 2. ビルド問題の解決経験
docker compose build --no-cache  # キャッシュ問題の解決方法習得
docker system prune -a           # システムクリーンアップ技術習得
```

#### 🔗 **サービス間通信問題の解決経験**
```bash
# 学習中に習得したネットワーク診断技術
docker network ls                    # Dockerネットワーク管理理解
docker network inspect [network]    # ネットワーク詳細分析能力

# 実践で身につけた通信テスト技術
docker exec laravel-nginx ping php      # nginx → php 通信確認
docker exec laravel-php-fpm curl nginx  # php → nginx 通信確認
```

#### 📁 **ボリューム問題の解決経験**
```bash
# 実際に経験したボリューム管理
docker volume ls                     # ボリューム管理技術習得
docker inspect laravel-php-fpm      # コンテナ詳細分析能力獲得

# ファイル権限問題の解決実践
docker exec laravel-php-fpm ls -la /var/www/html
docker exec laravel-php-fpm chown -R www-data:www-data /var/www/html
```

---

## 📚 学習で参考にしたDockerリソース

### **実際に学習で活用したリソース**
- 🔍 **Docker公式ドキュメント**: [docs.docker.com](https://docs.docker.com/) - 基本概念の理解に活用
- 🎓 **Docker Compose公式ガイド**: マルチコンテナ設計の学習に使用
- � **Laravel公式Docker情報**: フレームワークとの統合方法を学習
- � **実践ブログ・記事**: 実際の問題解決時に参考にした情報源

### **学習成果の今後の展開予定**
- **CI/CDパイプライン**: GitHub Actions + Docker の導入検討
- **クラウドデプロイ**: AWS ECS、Google Cloud Run への展開
- **Kubernetes学習**: コンテナオーケストレーションの次のステップ
- **モニタリング**: Prometheus + Grafana での運用監視

---

## 🤝 学習成果の共有とフィードバック

### **この学習プロジェクトについて**
このプロジェクトは個人のDocker学習の成果物です。同じような学習を進めている方のご参考になれば幸いです。

1. **質問・議論**: [GitHub Issues](https://github.com/tojiinoue-pasona/Docker-Practice/issues) で学習に関する質問や議論を歓迎
2. **改善提案**: より良い学習材料にするための提案やPull Request歓迎
3. **学習体験の共有**: 同様の学習を行った方の体験談や改善点の共有

### **学習継続のためのコントリビューション歓迎**
- 🐳 Docker設定のより良い実装方法の提案
- 📚 学習過程の記録・ドキュメント改善
- 🛠 新しい学習要素の追加提案
- 🐛 問題の報告・解決方法の共有

---

<div align="center">

## 🎓 Docker学習の成果記録

**このプロジェクトを通じてコンテナ技術を実践的に習得しました**

🐳 **Docker基礎** → 🎼 **Compose実践** → 🏗️ **アーキテクチャ理解** → 📈 **運用経験**

---

**🚀 Docker Learning Journey Completed! 🐳**

学習成果の記録として作成 by [tojiinoue-pasona](https://github.com/tojiinoue-pasona)

**📝 同じような学習を進めている方のご参考になれば幸いです**

</div>
