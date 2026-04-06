# GitHub Copilot Instructions

このプロジェクトの AI 指示は `AGENT.md`（プロジェクトルート）に記載されています。
Copilot はそちらを参照してください。

主要ルール:
- 実装前に契約（Precondition / Postcondition）を定義する
- `[RequireComponent]` と `Debug.Assert` で依存を明示・検証する
- async/await 禁止、Coroutine を使う
- C# event/Action 禁止、UnityEvent を使う
- ブランチ: `feature/issue-番号-概要`、コミット: `feat: #番号 概要`

詳細ルール:
- `.claude/rules/coding-standards.md`
- `.claude/rules/layer-data.md` / `layer-logic.md` / `layer-presentation.md`
