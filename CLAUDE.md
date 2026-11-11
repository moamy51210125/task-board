# task-board

タスク管理ボードアプリのプロジェクトです。

## プロジェクト概要

- 機能: タスクの追加、完了・未完了の切り替え、削除、完了済みタスクのグレー表示
- データ保存: ブラウザのローカルストレージ（キー: `task-board.tasks`）。サーバー・DBはなし

## デプロイ先

https://moamy51210125.github.io/task-board/

- GitHub Pages で公開。`main` への push で [.github/workflows/deploy.yml](.github/workflows/deploy.yml) が自動ビルド・デプロイする
- 公開URLがリポジトリ名配下になるため、[vite.config.js](vite.config.js) でビルド時のみ `base: '/task-board/'` を設定している。リポジトリ名を変えたらここも変える

## 技術スタック

| 分類 | 使用技術 |
|---|---|
| UI | React 19（関数コンポーネント + Hooks） |
| ビルド・開発サーバー | Vite 7（`@vitejs/plugin-react`） |
| 言語 | JavaScript（JSX）。TypeScript は未使用 |
| スタイル | 素の CSS（CSSフレームワーク・CSS-in-JS は未使用） |
| ホスティング | GitHub Pages（GitHub Actions でデプロイ） |
| Node.js | ローカル v26 / CI は v22 |

### コマンド

- `npm install` … 依存パッケージのインストール
- `npm run dev` … 開発サーバー起動（http://localhost:5173/）
- `npm run build` … 本番用ビルド（`dist/` に出力）
- `npm run preview` … ビルド結果の確認
- テスト・Lint は未導入

### ディレクトリ構成

```
index.html          エントリHTML
vite.config.js      Vite設定（GitHub Pages用の base を含む）
src/
  main.jsx          Reactの起動処理
  App.jsx           アプリ本体（タスクの状態管理と画面）
  App.css           スタイル
.github/workflows/
  deploy.yml        GitHub Pages デプロイ用ワークフロー
```

## コンポーネントの命名規約

現在のコード（[src/App.jsx](src/App.jsx)）に合わせた規約です。新しいコードもこれに従ってください。

- **コンポーネント**: PascalCase の関数コンポーネントにする（例: `App`, `TaskItem`）
- **ファイル名**: コンポーネント名と同じ PascalCase + `.jsx`（例: `App.jsx`）。1ファイル1コンポーネントを基本とし、`export default` で公開する
- **配置**: `App` 以外のコンポーネントを追加するときは `src/components/` に置く（今後の方針。現在は `App.jsx` のみ）
- **スタイル**: コンポーネントと同名の CSS ファイル（例: `App.css`）にまとめる
- **イベント処理関数**: 「動詞 + 対象」の camelCase（例: `addTask`, `toggleTask`, `deleteTask`）
- **state**: 名詞の camelCase（例: `tasks`, `text`）。更新関数は `set` + 名前（例: `setTasks`）
- **定数**: UPPER_SNAKE_CASE（例: `STORAGE_KEY`）
- **CSS クラス名**: kebab-case（例: `add-form`, `task-list`）。状態は修飾クラスで表す（例: `task done`）
- **コメント**: 日本語で、処理の意図を短く書く

## コーディング方針

- 変更は依頼された範囲に絞り、無関係なリファクタリングはしない
- 既存のコードスタイル・命名規則に合わせる
- 機密情報（APIキー、トークン、`.env` など）をコードやコミットに含めない

## Git運用ルール

### 基本方針: コードを変更するたびに GitHub へプッシュする

コードを変更したら、作業の区切りごとに以下の流れで GitHub に反映します。

1. `git status` / `git diff` で変更内容を確認する
2. 関係するファイルだけを `git add` する（`git add .` / `git add -A` は避け、機密ファイルが混ざっていないか確認）
3. 変更内容が分かるコミットメッセージで `git commit` する
4. **プッシュ前にユーザーへ「これから `git push` を実行します、良いですか？」と確認する**
5. 承認を得てから `git push` する

> 手順4の確認は省略しないこと。グローバル設定（`~/.claude/CLAUDE.md`）で `git push` は「外に出る操作」として毎回の事前確認が必要とされており、このルールはそれを上書きしません。

### コミットメッセージ

- 1行目に変更の要約を簡潔に書く（例: `タスクの並び替え機能を追加`）
- 必要に応じて2行目以降に理由や詳細を書く
- 1コミット = 1つのまとまった変更 を意識する

### ブランチ

- `main` に直接コミットせず、作業用ブランチ（例: `feature/xxx`, `fix/xxx`）を切って作業する
- `main` への反映はプルリクエスト経由で行う（作成前にユーザーへ確認する）

### 禁止事項（事前確認なしに実行しない）

- `git push --force` / `git push -f`
- `git reset --hard`, `git clean -fd`, `git checkout -- .`, `git restore .`
- `git branch -D`, `git stash drop`
- push済みコミットへの `git commit --amend` や `git rebase`
- リモートの追加・変更（`git remote add`, `git remote set-url`）

### リポジトリ情報

- リモート: `origin` → https://github.com/moamy51210125/task-board.git
- デフォルトブランチ: `main`
- コミット作成者（このリポジトリ内のみ）: `moamy51210125` / `moamy51210125@users.noreply.github.com`
- `.gitignore` で `.env` などの機密ファイル、`node_modules/`、ビルド成果物を除外済み
