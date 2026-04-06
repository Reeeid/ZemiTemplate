---
description: C# 命名規則・契約プログラミング・Unity コーディングルール。コード生成時に常時参照。
---

# Coding Standards

## 命名規則

| 対象 | 規則 | 例 |
|-----|------|---|
| クラス・メソッド・プロパティ | PascalCase | `PlayerController`, `TakeDamage()` |
| ローカル変数・引数 | camelCase | `moveSpeed`, `damage` |
| プライベートフィールド | `_camelCase` | `_currentHp` |
| 定数 | UPPER_SNAKE_CASE | `MAX_HEALTH` |

---

## 契約プログラミング（必須）

すべての public メソッドに XMLドキュメントで契約を記述すること。

```csharp
/// <summary>
/// ダメージを受けてHPを減少させる。
/// </summary>
/// <param name="amount">ダメージ量。0以上でなければならない。</param>
/// <remarks>
/// Precondition: amount >= 0
/// Precondition: IsAlive == true
/// Postcondition: CurrentHp == Mathf.Max(0, old_CurrentHp - amount)
/// Postcondition: amount > 0 の場合、OnDamaged が発火する
/// </remarks>
public void TakeDamage(int amount)
{
    Debug.Assert(amount >= 0, $"TakeDamage: amount must be >= 0, got {amount}");
    Debug.Assert(IsAlive, "TakeDamage: called on dead character");
    // ...
}
```

**ルール:**
- `Debug.Assert` で前提条件をランタイム検証する（開発ビルドのみ実行される）
- XMLドキュメントに Precondition / Postcondition / Invariant を明記する
- `[RequireComponent]` で依存コンポーネントを強制宣言する

---

## Unity ルール

```csharp
// ✅ インスペクター公開は SerializeField private
[SerializeField] private float _moveSpeed = 5f;

// ❌ public フィールド禁止
public float moveSpeed;

// ✅ 参照は SerializeField か GetComponent
[SerializeField] private Rigidbody2D _rb;

// ❌ GameObject.Find() 禁止
var obj = GameObject.Find("Player");

// ✅ 非同期は Coroutine
private IEnumerator WaitAndDo(float delay)
{
    yield return new WaitForSeconds(delay);
}

// ❌ async/await 禁止
// ❌ C# event / Action / delegate 原則禁止 → UnityEvent を使う
[SerializeField] private UnityEvent OnDeath;

// ✅ 依存コンポーネントは属性で宣言
[RequireComponent(typeof(Rigidbody2D))]
public class PlayerMovement : MonoBehaviour { }
```

---

## コメントルール

- コメントは日本語でOK
- 「何をしているか」ではなく「**なぜそうしているか**」を書く
- 複雑なロジックには必ずコメントを付ける

---

## 禁止パターン

- `Debug.Log` の出力を本番コードに残さない（`#if UNITY_EDITOR` で囲むか削除）
- マジックナンバーは `const` または `SerializeField` に切り出す
- 1クラス300行超えたら分割を検討する
