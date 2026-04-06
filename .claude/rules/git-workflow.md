---
description: Git コミット・ブランチ・PR ルール。コミットや PR 作成時に参照。
---

# Git Workflow

## ブランチ命名（Issue番号必須）

```
feature/issue-12-player-movement
fix/issue-34-camera-clip-bug
refactor/issue-56-inventory-class
```

禁止: `feature/new-feature`（Issue番号なし）、`my-branch`、`test`

---

## コミットメッセージ

**形式**: `type: #issue番号 概要`

```
feat: #12 プレイヤー移動スクリプト追加
fix: #34 カメラのクリッピング問題を修正
refactor: #56 インベントリクラスを分割
chore: #78 GitHub Actions ワークフロー更新
docs: #90 ADR-003 追加
```

- Issue番号は**必須**（初回セットアップコミットのみ例外）
- Body には「**なぜその変更が必要だったか**」を書く

---

## PR ルール

- **PR 前にローカルで動作確認**（Unity Editor で再生して確認）
- **1 PR = 1 機能**（巨大 PR は分割する）
- PR 本文は必ず `PULL_REQUEST_TEMPLATE.md` に従う
- **トレードオフの記載必須**（採用したアプローチのデメリットと却下した代替案）
- **テストしていないことの明記必須**
- `closes #番号` で Issue を紐づける

---

## ADR

重要な技術判断（ライブラリ選定・アーキテクチャ方針）を行う場合は `.github/decisions/` に ADR を作成すること。
ADR は実装 PR に含めて一緒にレビューする。
