# ZemiTemplate — AI-Driven Game Dev Template

Claude Code + GitHub Flow を使った Unity ゲーム開発テンプレート。
契約プログラミングとレイヤー別サブエージェントによる品質管理が特徴。

---

## ブランチ構成

| ブランチ | 内容 |
|---------|------|
| `main` | コアテンプレート（AI非依存・技術スタック非依存） |
| `module/unity` | Unity モジュール付き（このブランチ） |

---

## セットアップ

### 1. リポジトリをクローン

```bash
git clone https://github.com/Reeeid/ZemiTemplate.git
cd ZemiTemplate
```

### 2. Git LFS を有効化

```bash
git lfs install
```

### 3. Unity プロジェクトを作成

Unity Hub で `Assets/` フォルダを含むディレクトリを Unity プロジェクトとして開く（Unity 2022 LTS 推奨）。

### 4. GitHub Actions（CI）の設定

Unity テストを自動実行するには GitHub Secrets に以下を登録する。

| Secret 名 | 取得方法 |
|-----------|---------|
| `UNITY_LICENSE` | ローカルで Activation File を生成 → [game-ci 手順](https://game.ci/docs/github/activation) |
| `UNITY_EMAIL` | Unity アカウントのメールアドレス |
| `UNITY_PASSWORD` | Unity アカウントのパスワード |

### 5. AGENT.md のプロジェクト情報を記入

`AGENT.md` の TODO 欄にゲームコンセプトとドメイン固有の命名規則を追記する。

---

## 開発フロー

```
plan セッション（Claude Codeで）
  └─ 「〇〇機能を作りたい」→ Issue 作成 → 契約定義
       ↓
impl:data / impl:logic / impl:presentation セッション
  └─ レイヤー別サブエージェントが実装
       ↓
Unity Editor で手動設定（Claude が手順書を出力）
       ↓
PR 作成 → マージ
```

詳細: `.claude/rules/session-guide.md`

---

## 参照ドキュメント

| リソース | URL |
|---------|-----|
| Unity 公式ドキュメント | https://docs.unity3d.com/ |
| Unity Scripting API | https://docs.unity3d.com/ScriptReference/ |
| game-ci（GitHub Actions） | https://game.ci/docs/github/getting-started/ |
| Git LFS | https://git-lfs.github.com/ |
| Conventional Commits | https://www.conventionalcommits.org/ |

---

## ファイル構成

```
.
├── AGENT.md                        # AI共通指示（本体）
├── CLAUDE.md                       # Claude Code用（AGENT.mdをインポート）
├── .claude/
│   ├── agents/                     # サブエージェント定義
│   │   ├── data-agent.md           # ScriptableObject専門
│   │   ├── logic-agent.md          # ゲームシステム専門
│   │   ├── presentation-agent.md   # UI・Input専門
│   │   └── learn-agent.md          # コード解説専門
│   └── rules/                      # レイヤー別ルール
│       ├── coding-standards.md
│       ├── git-workflow.md
│       ├── layer-data.md
│       ├── layer-logic.md
│       ├── layer-presentation.md
│       └── session-guide.md
└── .github/
    ├── decisions/                  # ADR（技術判断の記録）
    ├── ISSUE_TEMPLATE/             # Issue テンプレート×4
    ├── PULL_REQUEST_TEMPLATE.md
    ├── workflows/test.yml          # Unity EditMode テスト
    └── dependabot.yml
```
