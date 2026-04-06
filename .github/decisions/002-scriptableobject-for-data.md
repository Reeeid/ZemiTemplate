# 002: ScriptableObject でゲームデータを管理する

## ステータス

- [x] 決定

## コンテキスト

キャラクターステータス・アイテム定義・スキル情報などの静的ゲームデータをどこで管理するか。

## 決定

静的データ（実行時に変わらない定義・数値）は ScriptableObject で管理する。

## 代替案との比較

### 1. 採用案: ScriptableObject

- **Good**: Unityエディタ上でGUI操作できる。デザイナーがコード不要で調整可能。YAML形式でバージョン管理しやすい
- **Bad**: 実行時データには使えない（読み取り専用前提）

### 2. JSON / CSV ファイル

- **Good**: 外部ツールで編集できる
- **却下理由**: Unityとの統合に追加コードが必要。型安全性が低い

### 3. MonoBehaviour に直接持つ

- **Good**: シンプル
- **却下理由**: データが GameObject に依存する。再利用できない

## 結果

- **Positive**: データ変更がコード変更なしにできる。`OnValidate()` でエディタ上のバリデーション可能
- **Negative**: 実行時に変わるデータ（現在HP等）は別管理が必要

## 関連

- [ADR-001](001-monobehaviour-coroutine-architecture.md)
- `.claude/rules/layer-data.md`
