# Architecture Decision Records (ADR)

重要な技術的意思決定の記録。「なぜこの技術を選んだか」「どの代替案を検討したか」を残す。

**Claude Code へ**: 技術判断が必要な時だけ参照すること。通常の実装では不要。

---

## ADR を作成するタイミング

- アーキテクチャレベルの選択（同期 vs 非同期、パターンの採用等）
- 主要ライブラリ・フレームワークの採用
- セキュリティ・パフォーマンス方針
- 過去の決定を覆す場合

❌ 不要: 命名規則・軽微なバグ修正・コードスタイル

---

## 作成方法

```bash
# 次の番号を確認
ls .github/decisions/*.md | sort | tail -1

# テンプレートからコピー
cp .github/decisions/TEMPLATE.md .github/decisions/003-your-decision.md
```

ADR は**実装 PR に含める**（単独 PR は不要）。

---

## ステータス

- **提案中**: 議論中
- **決定**: 採用・実装済み
- **棄却**: 検討したが不採用
- **非推奨**: 別の ADR で置き換えられた

---

## 一覧

| No. | タイトル | ステータス | 日付 |
|-----|---------|----------|------|
| [001](001-monobehaviour-coroutine-architecture.md) | MonoBehaviour + Coroutine を基本アーキテクチャとする | 決定 | 2026-04-06 |
| [002](002-scriptableobject-for-data.md) | ScriptableObject でゲームデータを管理する | 決定 | 2026-04-06 |
| [003](003-layer-based-subagent-architecture.md) | レイヤー単位のサブエージェント分割とClaude Code運用方針 | 決定 | 2026-04-06 |
