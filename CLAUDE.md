# CLAUDE.md

このファイルは、このリポジトリで作業する Claude Code 向けのガイドです。

## プロジェクト概要

- プロジェクト名: task-board
- 内容: タスク管理ボードアプリケーション(タスクの追加・完了切り替え・削除。完了済みはグレー表示)
- タスクは React の state で管理し、ブラウザの localStorage(キー: `task-board.tasks`)に保存している。リロードしても消えない

## 技術スタック

| 区分 | 使用技術 |
| --- | --- |
| UI ライブラリ | React 19(関数コンポーネント + Hooks) |
| ビルドツール / 開発サーバー | Vite 8(`@vitejs/plugin-react`) |
| 言語 | JavaScript(JSX)。TypeScript は未導入 |
| スタイル | 素の CSS(`src/App.css`)。CSS フレームワークは未使用 |
| データ保存 | ブラウザの localStorage(サーバー・DB なし) |
| ランタイム | Node.js 24 / npm |
| ホスティング | GitHub Pages(GitHub Actions で自動デプロイ) |

### ディレクトリ構成

```
task-board/
├── .github/workflows/deploy.yml  # GitHub Pages への自動デプロイ
├── index.html                    # エントリーの HTML
├── vite.config.js                # Vite 設定(base: '/task-board/')
└── src/
    ├── main.jsx                  # React のマウント処理
    ├── App.jsx                   # 画面とタスクの状態管理
    └── App.css                   # スタイル
```

## コンポーネントの命名規約

- **コンポーネント名**: PascalCase(例: `App`, `TaskItem`, `TaskForm`)。
- **ファイル名**: コンポーネント名と同じ名前にし、拡張子は `.jsx`(例: `TaskItem.jsx`)。1ファイルにつき、主なコンポーネントは1つにする。
- **配置**: コンポーネントを分けるときは `src/components/` に置く。スタイルを分ける場合は、同じ名前の `.css` を横に置く(例: `TaskItem.css`)。
- **エクスポート**: コンポーネントは `export default` で書き出す(`App.jsx` に合わせる)。
- **変数・関数**: camelCase。
  - state は名詞で書く(`tasks`, `text`)。更新関数は `set` + 名前(`setTasks`)。
  - イベント処理の関数は動詞 + 対象で書く(`addTask`, `toggleTask`, `deleteTask`)。
  - 親から渡すイベント用 props の名前は `on` で始める(例: `onToggle`, `onDelete`)。
  - 真偽値は状態を表す名前にする(`done`。必要に応じて `is` / `has` を付ける)。
- **定数**: モジュールの一番上に置く定数は UPPER_SNAKE_CASE(例: `STORAGE_KEY`)。
- **CSS クラス名**: 小文字のケバブケース(例: `add-form`, `task-list`)。状態は別のクラスを足して表す(例: `task done`)。
- **画面の文言**: ボタン名、プレースホルダー、`aria-label` などは日本語で書く。

## コミュニケーション

- 回答・コメント・コミットメッセージは日本語で書く。

## Git 運用ルール

- **コードを変更するたびに、コミットして GitHub にプッシュする。**
  - 1つの変更(機能追加・修正・リファクタなど)が終わったら、まとめて放置せずにその都度 `git add` → `git commit` → `git push` まで行う。
  - プッシュ前に、ビルドやテストがある場合は実行して通ることを確認する。
- コミットは意味のある単位で小さく分ける。
- コミットメッセージは日本語で、何を・なぜ変更したかが分かるように書く。
  - 例: `タスクの並び替え機能を追加`、`期限切れタスクの表示崩れを修正`
- `main` ブランチへの force push(`git push --force`)は禁止。
- プッシュに失敗した場合(コンフリクト、認証エラーなど)は、勝手に強制的な解決をせず、状況をユーザーに報告する。

## 機密情報・個人情報の取り扱い

- 社外秘情報(社内の業務データ、顧客情報、内部資料など)をコードやコミットに含めない。
- 個人情報(氏名、メールアドレス、電話番号、住所など)をソースコード、テストデータ、ログ、コミットメッセージに含めない。テストデータにはダミー値を使う。
- API キー、パスワード、トークンなどの認証情報はコミットしない。`.env` などに分離し、`.gitignore` に追加する。
- プッシュ前に、上記の情報が差分に含まれていないか確認する。リポジトリが公開(public)の場合は特に注意する。

## 開発コマンド

```bash
npm install      # 依存パッケージのインストール
npm run dev      # 開発サーバー起動(http://localhost:5173/task-board/)
npm run build    # 本番ビルド(dist/ に出力)
npm run preview  # ビルド結果の確認
```

- テストは未導入。プッシュ前は最低限 `npm run build` が通ることを確認する。

## デプロイ先

https://mabc-nidaira.github.io/task-board/

- `main` へのプッシュで `.github/workflows/deploy.yml` が動き、ビルド結果(`dist/`)が GitHub Pages に自動デプロイされる。
- `vite.config.js` の `base` はリポジトリ名(`/task-board/`)に合わせている。リポジトリ名を変えたら `base` も変更する。
- `main` へのプッシュは、そのまま一般公開につながる。社外秘情報や個人情報が含まれていないか、プッシュ前に必ず確認する。
