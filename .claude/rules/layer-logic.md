---
description: Logic層のルール。ゲームシステム（戦闘・移動・インベントリ等）の実装時に参照。
---

# Layer: Logic

## 責務

- ゲームの**ルールと状態変化**の実装
- 戦闘・移動・インベントリ・スキル等のシステム
- Data 層から ScriptableObject を受け取り、状態を計算・変更する

## 責務外

- データの定義 → Data 層
- UI の更新 → Presentation 層に任せる（直接 UI を触らない）
- Input の受付 → Presentation 層

---

## 実装パターン

```csharp
/// <summary>
/// キャラクターの戦闘状態を管理する。
/// </summary>
/// <remarks>
/// Invariant: CurrentHp は常に 0 以上 MaxHp 以下
/// </remarks>
[RequireComponent(typeof(CharacterData))]
public class CombatHandler : MonoBehaviour
{
    [SerializeField] private CharacterStatsSO _stats;

    private int _currentHp;

    public bool IsAlive => _currentHp > 0;
    public int CurrentHp => _currentHp;

    [SerializeField] private UnityEvent<int> OnDamaged;   // 引数: 受けたダメージ量
    [SerializeField] private UnityEvent OnDeath;

    private void Awake()
    {
        Debug.Assert(_stats != null, $"{name}: _stats is not assigned");
        _currentHp = _stats.MaxHp;
    }

    /// <summary>ダメージを受けてHPを減少させる。</summary>
    /// <remarks>
    /// Precondition: amount >= 0
    /// Precondition: IsAlive == true
    /// Postcondition: CurrentHp == Mathf.Max(0, old - amount)
    /// </remarks>
    public void TakeDamage(int amount)
    {
        Debug.Assert(amount >= 0, $"TakeDamage: amount={amount}");
        Debug.Assert(IsAlive, "TakeDamage: called on dead character");

        _currentHp = Mathf.Max(0, _currentHp - amount);
        OnDamaged?.Invoke(amount);

        if (_currentHp == 0) OnDeath?.Invoke();
    }
}
```

---

## ルール

- Logic クラスは **UI を直接参照しない**（UnityEvent で通知するだけ）
- 状態変化は `UnityEvent` で Presentation 層に通知する
- 重い処理（複数フレームにまたがるもの）は `Coroutine` で書く
- `Update()` は最小限に。毎フレーム不要な処理は `InvokeRepeating` や `Coroutine` で間引く
- ゲームシステムは1クラス1責任（戦闘・移動・インベントリを同じクラスに混ぜない）
