---
description: Presentation層のルール。UI・MonoBehaviourの接着・Input処理の実装時に参照。
---

# Layer: Presentation

## 責務

- **Input の受付**と Logic 層への橋渡し
- **UI 更新**（HP バー、テキスト、アニメーション等）
- Logic 層の UnityEvent を受け取って画面に反映する
- MonoBehaviour のライフサイクル（Awake/Start/Update）の管理

## 責務外

- ゲームルールの計算 → Logic 層
- データ定義 → Data 層

---

## 実装パターン

### Input → Logic への橋渡し

```csharp
/// <summary>
/// プレイヤーの入力を受け取り、移動処理に渡す。
/// </summary>
/// <remarks>
/// Precondition: PlayerMovement コンポーネントがアタッチされている
/// </remarks>
[RequireComponent(typeof(PlayerMovement))]
public class PlayerInputHandler : MonoBehaviour
{
    private PlayerMovement _movement;

    private void Awake()
    {
        _movement = GetComponent<PlayerMovement>();
        Debug.Assert(_movement != null, $"{name}: PlayerMovement not found");
    }

    private void Update()
    {
        // Input.GetAxisRaw は -1/0/1 を返す（デジタル入力）
        var input = new Vector2(
            Input.GetAxisRaw("Horizontal"),
            Input.GetAxisRaw("Vertical")
        );
        _movement.SetMoveInput(input);
    }
}
```

### Logic の UnityEvent を受けて UI 更新

```csharp
/// <summary>
/// CombatHandler のイベントを受けて HP バーを更新する。
/// </summary>
[RequireComponent(typeof(CombatHandler))]
public class HealthBarUI : MonoBehaviour
{
    [SerializeField] private Slider _hpSlider;
    [SerializeField] private CombatHandler _combat;

    private void Start()
    {
        Debug.Assert(_hpSlider != null, $"{name}: _hpSlider not assigned");
        // 初期値設定
        UpdateBar(_combat.CurrentHp);
    }

    // CombatHandler.OnDamaged から呼び出される（Inspector で紐づけ）
    public void OnDamaged(int amount)
    {
        UpdateBar(_combat.CurrentHp);
    }

    private void UpdateBar(int currentHp)
    {
        _hpSlider.value = (float)currentHp / _combat.Stats.MaxHp;
    }
}
```

---

## Unity Editor 設定が必要な操作

Presentation 層の実装では必ず以下の設定手順を出力すること：

```
【Editor 設定手順】
1. GameObject に HealthBarUI をアタッチ
2. Inspector で _hpSlider に Slider を、_combat に CombatHandler をアサイン
3. CombatHandler の OnDamaged に HealthBarUI.OnDamaged をドラッグ＆ドロップ
```

---

## ルール

- Input は `Input.GetAxisRaw()` を使う（`GetAxis()` はスムージングがかかるため）
- UI スクリプトはゲームロジックを持たない（Logic 層のメソッドを呼ぶだけ）
- UnityEvent の紐づけは Inspector で行う（コードで `AddListener` しない）
- 実装後に必ず **Editor 設定手順**をリストアップする
