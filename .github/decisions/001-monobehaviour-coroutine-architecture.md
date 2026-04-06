# 001: MonoBehaviour + Coroutine を基本アーキテクチャとする

## ステータス

- [x] 決定

## コンテキスト

チームのC#経験が浅く、3ヶ月でゲームを完成させる必要がある。
async/await・イベント駆動・DIコンテナ等の高度なパターンは学習コストが高い。

## 決定

- MonoBehaviour を基本単位とする
- 非同期・待機処理は Coroutine (`IEnumerator`) を使う
- C# event / Action / delegate は原則禁止。UnityEvent かメソッド直接呼び出しを使う
- ECS・DI（Zenject等）は導入しない

## 代替案との比較

### 1. 採用案: MonoBehaviour + Coroutine

- **Good**: Unity公式チュートリアルと同じ。調査しやすい。直感的に書ける
- **Bad**: 規模が大きくなるとクラスが肥大化しやすい

### 2. ECS (Entity Component System)

- **Good**: パフォーマンスが高い。大規模ゲームに向く
- **却下理由**: 学習コストが非常に高い。3ヶ月では習得困難

### 3. Zenject / VContainer (DI)

- **Good**: テストしやすい。依存関係が明確
- **却下理由**: フレームワーク習得に時間がかかる。チーム全員への説明コストが高い

## 結果

- **Positive**: 誰でも読めるコード。Unity公式ドキュメントが参照できる
- **Negative**: 大規模化時のクラス肥大化リスク → クラス分割ルール（300行超えたら分割）で対処

## 関連

- [ADR-002](002-scriptableobject-for-data.md)
- `.claude/rules/layer-logic.md`
