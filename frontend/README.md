# タスク管理アプリケーション（フロントエンド）

このプロジェクトは、ReactとTypeScriptを使用したタスク管理アプリケーションのフロントエンドです。

## セットアップガイド

### 前提条件

- Node.js 16.0以上
- npm 7.0以上

### 環境構築手順

1. プロジェクトのクローン：
```bash
git clone <repository-url>
cd task-management/frontend
```

2. 依存パッケージのインストール：
```bash
npm install
```

3. 開発サーバーの起動：
```bash
npm run dev
```

アプリケーションは http://localhost:3000 で起動します。

## プロジェクト構造

```
frontend/
├── src/
│   ├── components/     # Reactコンポーネント
│   │   ├── TaskApp.tsx    # メインのタスクアプリコンポーネント
│   │   ├── TaskForm.tsx   # タスク作成/編集フォーム
│   │   └── TaskList.tsx   # タスク一覧表示
│   ├── types/         # TypeScript型定義
│   │   └── task.ts    # タスク関連の型定義
│   ├── api/           # API通信関連
│   │   └── tasks.ts   # タスクAPIクライアント
│   ├── utils/         # ユーティリティ関数
│   ├── App.tsx        # ルートコンポーネント
│   └── main.tsx       # エントリーポイント
├── public/           # 静的ファイル
├── package.json      # 依存関係定義
└── README.md         # 本ドキュメント
```

## 主要な機能

### タスク管理機能

1. タスク一覧表示
   - タスクのリスト表示
   - 完了/未完了の表示
   - 作成日時の表示

2. タスク作成
   - タイトルと説明の入力
   - バリデーション機能

3. タスク編集
   - 既存タスクの更新
   - 完了状態の切り替え

4. タスク削除
   - タスクの削除確認
   - 削除後の一覧更新

## 使用技術

- React 18
- TypeScript 4
- Tailwind CSS
- shadcn/ui
- Lucide Icons

## コンポーネント設計

### TaskApp（メインコンポーネント）
```typescript
interface Task {
  id: number;
  title: string;
  description: string;
  is_completed: boolean;
  created_at: string;
  updated_at: string | null;
}

const TaskApp: React.FC = () => {
  // 状態管理
  const [tasks, setTasks] = useState<Task[]>([]);
  
  // タスク取得
  useEffect(() => {
    fetchTasks();
  }, []);
  
  // レンダリング
  return (
    <div>
      <TaskForm />
      <TaskList tasks={tasks} />
    </div>
  );
};
```

### コンポーネント間のデータフロー

1. 親コンポーネント（TaskApp）
   - タスクの状態管理
   - APIとの通信

2. 子コンポーネント
   - TaskForm: タスク作成/編集フォーム
   - TaskList: タスク一覧表示
   - TaskItem: 個別タスク表示

## スタイリング

### Tailwind CSSの使用

```typescript
// コンポーネントの例
<div className="max-w-4xl mx-auto p-4">
  <h1 className="text-2xl font-bold mb-4">
    タスク管理
  </h1>
</div>
```

### shadcn/uiコンポーネント

使用可能なコンポーネント：
- Button
- Card
- Input
- Checkbox
- Textarea
など

## API通信

### エンドポイントとの通信

```typescript
const API_BASE_URL = 'http://localhost:8000';

// タスク一覧取得
const fetchTasks = async () => {
  try {
    const response = await fetch(`${API_BASE_URL}/tasks/`);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching tasks:', error);
    throw error;
  }
};
```

## エラーハンドリング

1. API通信エラー
   - ネットワークエラー
   - サーバーエラー
   - バリデーションエラー

2. ユーザー入力エラー
   - 必須フィールドの検証
   - 入力値の形式チェック

## 開発ガイド

### 新機能の追加手順

1. 型定義の追加（`types/`）
2. APIクライアントの実装（`api/`）
3. コンポーネントの実装（`components/`）
4. スタイルの適用
5. テストの作成

### コーディング規約

- ESLintの設定に従う
- Prettierでコードフォーマット
- 適切なコンポーネント分割
- 型定義の徹底

## テスト

```bash
# テストの実行
npm test

# 特定のコンポーネントのテスト
npm test TaskApp
```

## ビルドとデプロイ

```bash
# プロダクションビルド
npm run build

# ビルドのプレビュー
npm run preview
```

## パフォーマンス最適化

- React.memoの適切な使用
- useMemoとuseCallbackの活用
- 画像の最適化
- コード分割

## トラブルシューティング

よくある問題と解決方法：

1. `npm install`エラー
   → node_modulesを削除して再インストール
   
2. コンポーネントの再レンダリング問題
   → useCallbackやuseMemoの使用を検討
   
3. 型エラー
   → 型定義の確認と修正

## 今後の改善計画

1. レスポンシブデザインの強化
2. ダークモードの実装
3. アニメーションの追加
4. パフォーマンス最適化
5. アクセシビリティの向上

## 参考リンク

- [React 公式ドキュメント](https://reactjs.org/)
- [TypeScript 公式ドキュメント](https://www.typescriptlang.org/)
- [Tailwind CSS ドキュメント](https://tailwindcss.com/)
- [shadcn/ui ドキュメント](https://ui.shadcn.com/)