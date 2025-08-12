# Laravel Docker環境

## 🚀 クイックスタート

### 前提条件
- Docker
- Docker Compose

### 起動方法

1. **リポジトリをクローン**
```bash
git clone <repository-url>
cd docker-practice
```

2. **Docker環境を起動**
```bash
docker-compose up --build
```

3. **アプリケーションにアクセス**
- メインサイト: http://localhost:8000
- ログイン画面: http://localhost:8000/login

### 🛠️ 環境詳細

- **PHP**: 8.2-apache
- **データベース**: SQLite (ファイルベース)
- **フレームワーク**: Laravel

### 📁 プロジェクト構成

```
docker-practice/
├── src/                    # Laravelアプリケーション
├── Dockerfile             # Dockerイメージ定義
├── docker-compose.yml     # Docker Compose設定
└── README_DOCKER.md       # このファイル
```

### ⚡ 開発時のTips

**コンテナを停止**
```bash
docker-compose down
```

**ログを確認**
```bash
docker-compose logs -f app
```

**コンテナ内でコマンド実行**
```bash
docker-compose exec app bash
```

### 🔧 トラブルシューティング

**ポート8000が使用中の場合**
`docker-compose.yml`の`ports`を変更してください：
```yaml
ports:
  - "8080:80"  # 8080に変更
```

**データベースをリセットしたい場合**
```bash
docker-compose down
docker-compose up --build
```
