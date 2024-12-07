# タスク管理アプリケーション

FastAPIとReactを使用した、シンプルかつモダンなタスク管理アプリケーションです。

## プロジェクト概要

このプロジェクトは、FastAPIバックエンドとReactフロントエンドを組み合わせた、フルスタックのWebアプリケーションです。
タスクの作成、読み取り、更新、削除（CRUD操作）を実装した実践的な学習用プロジェクトです。

### 主な機能

- タスクの作成
- タスク一覧の表示
- タスクの編集
- タスクの削除
- タスクの完了状態の切り替え

### 使用技術

#### バックエンド
- FastAPI
- SQLAlchemy
- Pydantic
- SQLite

#### フロントエンド
- React
- TypeScript
- Tailwind CSS
- shadcn/ui

## プロジェクト構造

```
task-management/
├── backend/
│   ├── app/
│   │   ├── models/
│   │   │   └── task.py
│   │   ├── schemas/
│   │   │   └── task.py
│   │   ├── crud/
│   │   │   └── task.py
│   │   ├── database.py
│   │   └── main.py
│   ├── requirements.txt
│   └── README.md
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── TaskApp.tsx
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json
│   └── README.md
│
└── README.md
```

## 環境構築
### 前提条件

- Gitがインストールされていること
- Homebrewがインストールされていること（macOSの場合）
    - 参考：https://brew.sh/ja/
- Pyenvがインストールされていること
    - 参考：https://hitori-sekai.com/python/mac-python-install/(macOSの場合)
    - 参考：https://almonta2021blog.com/pyenv-version-windows/(Windowsの場合)

## セットアップ手順

### 1. プロジェクトの作成

```bash
# リポジトリのクローン
git clone https://github.com/yamato-snow/python_testproject_venv.git <プロジェクト名>
cd <プロジェクト名>

# 開発用ブランチの作成と切り替え
git branch <ブランチ名>
git checkout <ブランチ名>
```

### 2. 初回セットアップ

```bash
# Pythonバージョンの設定
pyenv local 3.11.5

## Pythonをインストールしていない場合
pyenv install 3.11.5

# 仮想環境の作成
python -m venv .venv

# 仮想環境の有効化
# Windows:(コマンドプロンプトで実行)
.venv\Scripts\activate.bat
# macOS:
source .venv/bin/activate

# 依存パッケージのインストール
pip install --upgrade pip
pip install -r requirements.txt

コマンドプロンプトの場合は、'pip'の前に'python -m 'をつける必要があります。

# 仮想環境の無効化
deactivate

```

### 3. 2回目以降の実行

```bash
# 仮想環境の有効化のみ実行
# Windows:
.venv\Scripts\activate.bat
# macOS:
. .venv/bin/activate
```

### 4. プログラムの実行

```bash
python main.py
```

### 5. 変更の保存

```bash
# 変更をコミット
git add .
git commit -m "変更の説明"

# リモートリポジトリにプッシュ
git push origin <ブランチ名>
```

### バックエンド（FastAPI）

1. Python仮想環境の作成とアクティベート：

```bash
# 仮想環境の作成
python -m venv venv

# 仮想環境のアクティベート
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate
```

2. 必要なパッケージのインストール：

```bash
pip install -r requirements.txt
```

#### requirements.txt の内容

```
fastapi==0.75.0
uvicorn==0.17.6
sqlalchemy==1.4.32
pydantic==1.9.0
python-dotenv==0.19.2
```

3. アプリケーションの起動：

```bash
uvicorn app.main:app --reload
```

サーバーは http://localhost:8000 で起動します。
APIドキュメントは http://localhost:8000/docs で確認できます。

### フロントエンド（React）

1. 必要なパッケージのインストール：

```bash
cd frontend
npm install
```

2. 開発サーバーの起動：

```bash
npm run dev
```

アプリケーションは http://localhost:3000 で起動します。

## API エンドポイント

| メソッド | エンドポイント | 説明 |
|----------|----------------|------|
| GET | /tasks/ | タスク一覧の取得 |
| GET | /tasks/{task_id} | 特定のタスクの取得 |
| POST | /tasks/ | 新規タスクの作成 |
| PUT | /tasks/{task_id} | タスクの更新 |
| DELETE | /tasks/{task_id} | タスクの削除 |

## データモデル

### タスク（Task）

```python
class Task(Base):
    __tablename__ = "tasks"
    
    id: int              # タスクID（主キー）
    title: str          # タスクのタイトル
    description: str    # タスクの説明
    is_completed: bool  # 完了状態
    created_at: datetime # 作成日時
    updated_at: datetime # 更新日時
```

## 開発ガイドライン

### コーディング規約

- PEP 8（Pythonコード）に従う
- ESLint/Prettier（TypeScript/React）の設定に従う
- 関数やクラスには適切なドキュメンテーションを含める
- 変数名は説明的で理解しやすいものを使用する

### バージョン管理

- 機能追加は feature/ ブランチで開発
- バグ修正は hotfix/ ブランチで対応
- コミットメッセージは具体的で理解しやすい内容にする

## 今後の改善予定

- [ ] ユーザー認証の実装
- [ ] タスクの優先度設定
- [ ] タスクの期限設定
- [ ] タスクのカテゴリ分類
- [ ] タスクの検索機能
- [ ] レスポンシブデザインの改善
- [ ] ダークモードの実装

## トラブルシューティング

### よくある問題と解決方法

1. **CORS エラー**
   - バックエンドの CORS 設定を確認
   - フロントエンドのAPI URLが正しいか確認

2. **データベース接続エラー**
   - データベースファイルの権限を確認
   - 接続文字列が正しいか確認

3. **モジュールが見つからないエラー**
   - 仮想環境が有効か確認
   - requirements.txt の内容が最新か確認

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。

## 貢献について

1. このリポジトリをフォーク
2. 機能開発用の新しいブランチを作成
3. 変更をコミット
4. ブランチにプッシュ
5. Pull Requestを作成

## 連絡先

質問や提案がありましたら、Issuesセクションに投稿してください。