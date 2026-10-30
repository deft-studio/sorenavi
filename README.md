# sorenavi

家電の説明書の中身を、AIにふつうの言葉で質問できるWebアプリ（自分用）。持っている家電を登録しておくと、「洗濯機がE211」と聞くだけで、エラーの意味と直し方が分かる。

- 企画の背景：[docs/planning.md](docs/planning.md)
- 設計ドキュメント：[docs/design-spec.md](docs/design-spec.md)

## 特徴

- **型番を言わずに聞ける**：登録した家電の中から、AIがどの家電の話かを選ぶ。同じ種類が2台あれば聞き返す
- **説明書に書いてあることだけで答える**：答えに説明書のページ番号と、そのページを開くボタンを付ける。書いていないことは「書いていない」と答える
- **危ない時は、答えより先に止めさせる**：煙・焦げ・水漏れなどの言葉は、AIに任せずコードで見つけ、決まった警告を先に出す
- **説明書を保存しない**：メーカー公式のPDFのリンクを、そのままAIに渡す
- **使いすぎを止める**：1日20回・月10ドルの上限

## 技術スタック

| 層 | 技術 |
| --- | --- |
| フロントエンド | Next.js・shadcn/ui・Tailwind CSS |
| バックエンド | Next.js（Cloudflare Workersの上で動かす。OpenNext） |
| DB | Cloudflare D1 |
| AI/LLM | Claude API（Anthropic公式のTypeScript SDK。PDFの読み取り・出典のページ・構造化出力） |
| インフラ・デプロイ先 | Cloudflare Workers・Cloudflare Access（ログイン） |
| CI/CD・テスト | GitHub Actions |
| その他 | Figma（画面設計） |

## 構成

- 直下 — アプリ本体のコード
- `docs/` — 企画書・設計書・画面の決まり・法律の確認・技術の確かめ・タスク・判断の記録
- `.github/` — CI/CDの設定・PRのテンプレート

## 開発について

- 企画と設計は、Claude Codeと一問一答で進めた。何を作るかは自分が決め、Claude Codeは調べ物・案・資料の下書きを担当した
- 設計は、4つの役（法律／個人情報とセキュリティ／使う人／作る人・テストする人）のAIに、別々にレビューさせた。出た指摘47件のうち、どれを採るかは自分が決めた
- 実装は、Claude Codeが[docs/tasks.md](docs/tasks.md)の完了条件に沿って行う。PRにはClaudeの自動レビューが付き、マージは自分が確かめてから行う
- 開発の決まり（AIエージェント向け）は[AGENTS.md](AGENTS.md)
