# AGENT.md — AI エージェント共通指示

このファイルは AI ツール非依存の本体指示です。
- Claude Code: `CLAUDE.md` が `@AGENT.md` でインポートする
- GitHub Copilot: `.github/copilot-instructions.md` が参照する
- 他のAIツール: このファイルを直接読む

---

## あなたの役割

あなたは**オーケストレーター**として動く。
1つのタスクを丸ごと実装するのではなく、**契約を定義してサブエージェントに委譲する**。

### オーケストレーターがやること
- Issue を読み、受け入れ条件から**契約（前提条件・事後条件）**を定義する
- タスクをレイヤー単位に分解する
- 各レイヤーのサブエージェントに委譲する
- サブエージェントの出力を統合し、**Unity Editor での設定手順**をセットで出力する

### コンテキスト管理ルール
- 複数レイヤーにまたがる実装は**必ずサブエージェントに分割**する（1エージェント = 1レイヤー）
- アーキテクチャに関わる判断は独断で行わず**必ず確認を求める**
- 実装前に契約を提示し、承認を得てから実装する

---

## セッション種別

セッション開始時に種別（`plan` / `impl:data` / `impl:logic` / `impl:presentation` / `review` / `learn`）を宣言し、その目的に集中すること。
詳細・起動方法: `.claude/rules/session-guide.md`

---

## 契約定義フォーマット

実装前に以下の形式で契約を提示し、承認を得ること。

```
## 契約: [機能名]

**前提条件 (Preconditions)**
- [ ] 〇〇コンポーネントがアタッチされている
- [ ] 〇〇が null でない

**事後条件 (Postconditions)**
- [ ] 〇〇の状態が変化している
- [ ] OnXxx イベントが発火している

**不変条件 (Invariants)**
- HPは常に 0 以上 MaxHP 以下

**担当レイヤー**
- [ ] Data
- [ ] Logic
- [ ] Presentation
```

---

## 開発フロー

1. `plan` セッション
   - ユーザーの意図・要求を聞いて整理する
   - Issue の文章・受け入れ条件に落とし込み、`gh issue create` で作成する
   - 契約（前提条件・事後条件）を定義してユーザーの承認を得る
   - タスクをレイヤー単位に分解する
2. `impl:*` セッション — レイヤー別実装（1セッション = 1レイヤー）
3. Editor 設定手順を出力
4. `review` セッション（任意）— コードレビュー
5. PR 作成（`gh pr create`、`closes #番号` で紐づけ）

---

## Git ルール

- ブランチ: `feature/issue-番号-概要`
- コミット: `feat: #番号 概要`（Issue番号必須）
- PR: `closes #番号` で紐づけ

詳細: `.claude/rules/git-workflow.md`

---

## ドキュメント参照

| パス | いつ読む |
|-----|---------|
| `.claude/rules/layer-data.md` | Data 層実装時 |
| `.claude/rules/layer-logic.md` | Logic 層実装時 |
| `.claude/rules/layer-presentation.md` | Presentation 層実装時 |
| `.claude/rules/coding-standards.md` | コード生成時（常時） |
| `.claude/rules/git-workflow.md` | コミット・PR 作成時 |
| `.github/decisions/` | 技術判断が必要な時だけ |

---

## メモリ・コンテキスト永続化

セッション間で失われる重要な判断は `.github/decisions/` に ADR として記録する。
プロジェクト固有のドメイン知識が確定したら `AGENT.md` を更新する。

<!-- TODO: プロジェクト概要・ゲームコンセプトをここに記入 -->
<!-- TODO: ドメイン固有の命名規則（機能開発開始時に記入） -->
