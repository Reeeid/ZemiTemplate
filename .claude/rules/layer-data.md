---
description: Data層のルール。ScriptableObject・データ定義の実装時に参照。
---

# Layer: Data

## 責務

- ゲームデータの**定義と保持**（変更しないデータ）
- ScriptableObject によるデータアセット
- データ構造の型定義（`struct` / `[Serializable] class`）

## 責務外（他レイヤーに属する）

- ゲームロジックの計算 → Logic 層
- UI 更新 → Presentation 層
- MonoBehaviour のライフサイクル処理 → Presentation 層

---

## ScriptableObject パターン

```csharp
/// <summary>
/// キャラクターの基本ステータス定義。
/// Inspector から設定し、実行時には読み取り専用で扱うこと。
/// </summary>
/// <remarks>
/// Invariant: MaxHp > 0
/// Invariant: BaseAttack >= 0
/// </remarks>
[CreateAssetMenu(fileName = "CharacterStats", menuName = "Game/Character Stats")]
public class CharacterStatsSO : ScriptableObject
{
    [SerializeField] private int _maxHp = 100;
    [SerializeField] private int _baseAttack = 10;

    public int MaxHp => _maxHp;
    public int BaseAttack => _baseAttack;

    private void OnValidate()
    {
        // エディタ上での値検証
        Debug.Assert(_maxHp > 0, $"{name}: MaxHp must be > 0");
        Debug.Assert(_baseAttack >= 0, $"{name}: BaseAttack must be >= 0");
    }
}
```

---

## 配置場所

```
Assets/
└── Settings/
    ├── Characters/   ← キャラクターステータス
    ├── Items/        ← アイテム定義
    ├── Skills/       ← スキル定義
    └── Enemies/      ← 敵定義
```

---

## ルール

- ScriptableObject のデータは**実行時に書き換えない**（読み取り専用）
- 実行時に変化するデータ（現在HP等）は Logic 層の MonoBehaviour フィールドで管理
- `OnValidate()` で Inspector 上のバリデーションを実装する
- `[CreateAssetMenu]` を必ず付ける
