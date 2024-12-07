# タスク管理アプリケーション（バックエンド）

このプロジェクトは、FastAPIを使用したタスク管理アプリケーションのバックエンドAPIです。

## セットアップガイド

### 前提条件

- Python 3.8以上
- pip（Pythonパッケージマネージャー）

### 環境構築手順

1. プロジェクトのクローン：
```bash
git clone <repository-url>
cd task-management/backend
```

2. 仮想環境の作成と有効化：
```bash
# 仮想環境の作成
python -m venv .venv

# 仮想環境の有効化
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate
```

3. 依存パッケージのインストール：
```bash
pip install -r requirements.txt
```

4. データベースの初期化：
```bash
python init_database.py
```

5. 開発サーバーの起動：
```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

## プロジェクト構造

```
backend/
├── app/
│   ├── models/           # SQLAlchemyモデル
│   │   └── task.py      # タスクモデル定義
│   ├── schemas/         # Pydanticスキーマ
│   │   └── task.py      # タスクスキーマ定義
│   ├── crud/           # CRUDロジック
│   │   └── task.py     # タスク操作の実装
│   ├── database.py     # DB設定
│   └── main.py         # アプリケーションのエントリーポイント
├── tests/              # テストファイル
├── requirements.txt    # 依存パッケージリスト
└── README.md          # 本ドキュメント
```

## API仕様

### エンドポイント一覧

#### タスク関連 `/tasks`

| メソッド | エンドポイント | 説明 | リクエスト例 | レスポンス例 |
|---------|---------------|------|-------------|-------------|
| GET | `/tasks` | タスク一覧取得 | - | `[{"id": 1, "title": "タスク1", ...}]` |
| GET | `/tasks/{id}` | 個別タスク取得 | - | `{"id": 1, "title": "タスク1", ...}` |
| POST | `/tasks` | タスク作成 | `{"title": "新規タスク", ...}` | `{"id": 2, ...}` |
| PUT | `/tasks/{id}` | タスク更新 | `{"title": "更新後", ...}` | `{"id": 1, ...}` |
| DELETE | `/tasks/{id}` | タスク削除 | - | `{"id": 1, ...}` |

### リクエスト/レスポンス形式

#### タスク作成（POST）
```json
// リクエスト
{
  "title": "タスクのタイトル",
  "description": "タスクの説明",
  "is_completed": false
}

// レスポンス
{
  "id": 1,
  "title": "タスクのタイトル",
  "description": "タスクの説明",
  "is_completed": false,
  "created_at": "2024-01-01T00:00:00",
  "updated_at": null
}
```

## 開発ガイド

### 新しい機能の追加方法

1. モデルの定義（`app/models/`）
2. スキーマの定義（`app/schemas/`）
3. CRUD操作の実装（`app/crud/`）
4. エンドポイントの追加（`app/main.py`）

### コーディング規約

- PEP 8に準拠
- 型ヒントを積極的に使用
- docstringでの関数説明
- 意味のある変数名・関数名の使用

### テスト

テストの実行：
```bash
pytest
```

## データベース

### モデル定義

```python
class Task(Base):
    __tablename__ = "tasks"
    
    id = Column(Integer, primary_key=True, index=True)
    title = Column(String(100), nullable=False)
    description = Column(String(1000))
    is_completed = Column(Boolean, default=False)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
    updated_at = Column(DateTime(timezone=True), onupdate=func.now())
```

### マイグレーション

現在はSQLiteを使用しているため、明示的なマイグレーションは不要です。
将来的にPostgreSQLなどに移行する際は、Alembicを使用してマイグレーションを管理します。

## エラーハンドリング

主なエラーケースとその対処：

1. データベース接続エラー
   - ログの確認
   - データベースファイルの権限確認
   - 接続文字列の確認

2. バリデーションエラー
   - リクエストボディの形式確認
   - 必須フィールドの確認
   - 文字列長などの制約確認

3. 404エラー
   - URLパスの確認
   - IDの存在確認

## セキュリティ

現在実装されているセキュリティ機能：

- CORSミドルウェア
- 入力バリデーション
- SQLインジェクション防止（SQLAlchemy使用）

今後追加予定の機能：

- JWT認証
- レート制限
- SSLサポート

## トラブルシューティング

よくある問題と解決方法：

1. `ModuleNotFoundError`
   → 仮想環境が有効化されているか確認
   
2. データベースエラー
   → `task_app.db`ファイルの存在と権限を確認
   
3. CORS関連エラー
   → `main.py`のCORS設定を確認

## 次のステップ

1. ユーザー認証の実装
2. タスクの優先度機能の追加
3. タスクのカテゴリ分類機能の追加
4. PostgreSQLへの移行
5. ユニットテストの拡充

## 参考リンク

- [FastAPI 公式ドキュメント](https://fastapi.tiangolo.com/)
- [SQLAlchemy ドキュメント](https://docs.sqlalchemy.org/)
- [Pydantic ドキュメント](https://pydantic-docs.helpmanual.io/)