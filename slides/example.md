---
marp: true
theme: company
paginate: true
---

<!-- _class: title -->
![bg](../assets/images/background.png)

# プロジェクト管理改革
## アジャイル開発で変わる組織文化

### 実践事例と導入ガイド
2025-02-02 / 技術カンファレンス

---

<!-- _class: agenda -->

## 本日のアジェンダ

1. 現代のプロジェクト管理における課題
2. アジャイル開発の基本原則と実践方法
3. 組織変革の具体的なステップ
4. 導入事例と成果指標
5. よくある失敗パターンと対策

---

<!-- _class: section -->

# なぜ今、アジャイルなのか
## 従来の開発手法の限界

---

## ウォーターフォールの課題

チーム内の会話は低コストだが、チーム間は調整・承認・ドキュメントが必要で高コスト。

結果として、**高コストな境界でモジュールが切れる**。

従来のウォーターフォール開発では、要件定義から実装、テスト、リリースまで順次進行するため、**市場の変化に対応できない**という致命的な問題があります。

顧客のニーズは日々変化するのに、半年後にしかフィードバックが得られない状況では、**リリース時には既に時代遅れ**になってしまいます。

---

<!-- _class: closing -->

# セクション1のまとめ

課題は明確になりました
次は解決策を見ていきましょう

---

<!-- _class: summary -->

## アジャイル開発の3つの核心価値

<div class="summary-items">

<div class="summary-item">

### 短期的なフィードバックループ
2週間スプリントで継続的に価値を検証し、方向転換のコストを最小化する

</div>

<div class="summary-item">

### チーム全体の透明性
デイリースタンドアップとレトロスペクティブで、問題を早期発見・早期解決

</div>

<div class="summary-item">

### 顧客との協調
プロダクトオーナーを通じて顧客価値を最大化し、無駄な機能開発を排除

</div>

</div>

---

<!-- _class: section -->

# 実践的な導入ステップ
## 具体的にどう始めるか

---

<!-- _class: content-2col -->

## フェーズ1: パイロットチームの選定

<div>

### 成功しやすいチームの特徴

- メンバー5〜9名の適切なサイズ
- 技術スキルの多様性がある
- 変化に前向きな文化
- 明確なプロダクトビジョン

</div>

<div>

### 避けるべきチームの特徴

- レガシーシステムに縛られている
- 固定マインドセットが強い
- 外部依存が多すぎる
- 経営層のサポートがない

</div>

---

<!-- _class: content-2col-comparison -->

## フェーズ2: 環境整備の重要性

<div>

### Before(導入前)

- 手作業によるデプロイ
- テストは週1回のみ
- ドキュメントは数百ページのWord
- コミュニケーションはメール中心

</div>

<div>

### After(導入後)

- CI/CDによる自動デプロイ
- テストは毎コミット時に実行
- ドキュメントはWikiで常に最新
- Slackでリアルタイムコラボレーション

</div>

---

<!-- _class: content-3col -->

## フェーズ3: 基本的なセレモニーの実施

<div>

### デイリースタンドアップ

- 毎朝15分
- 昨日やったこと
- 今日やること
- 障害はあるか

</div>

<div>

### スプリントプランニング

- スプリント開始時
- 2週間分の計画
- ストーリーポイント
- タスク分解

</div>

<div>

### レトロスペクティブ

- スプリント終了時
- KPT形式
- 改善アクション
- 次への活かし方

</div>

---

<!-- _class: content-image-right -->

## ツールの選定と活用

<div>

### プロジェクト管理ツール

**Jira、Linear、GitHub Projects**などのツールを活用することで、タスクの可視化と進捗管理が容易になります。

ツール選定のポイント:
- チームサイズに適した機能
- 既存ツールとの連携性
- カスタマイズの柔軟性
- コストパフォーマンス

導入初期は**シンプルなツールから始める**ことが成功の鍵です。

</div>

<div>

![ツールイメージ](https://via.placeholder.com/400x300/1B4565/FFFFFF?text=Project+Tools)

</div>

---

<!-- _class: content-center -->

## 最も重要なこと

### 完璧を目指すのではなく、継続的な改善を目指す

アジャイルは目的地ではなく旅である

---

<!-- _class: content-grid-2x2 -->

## 成功事例: 4つの企業パターン

<div>

![事例1](https://via.placeholder.com/350x200/1B4565/FFFFFF?text=Startup)

### スタートアップA社
開発サイクルを3ヶ月から2週間に短縮し、リリース頻度が12倍に向上

</div>

<div>

![事例2](https://via.placeholder.com/350x200/3E9BA4/FFFFFF?text=Enterprise)

### エンタープライズB社
チーム間のサイロ化を解消し、部門横断プロジェクトの成功率が80%向上

</div>

<div>

![事例3](https://via.placeholder.com/350x200/1ab394/FFFFFF?text=SaaS)

### SaaS企業C社
顧客フィードバックループを確立し、機能満足度が45%から87%に改善

</div>

<div>

![事例4](https://via.placeholder.com/350x200/4b5563/FFFFFF?text=Legacy)

### レガシーシステムD社
段階的移行により、モノリスからマイクロサービスへ3年で完全移行

</div>

---

<!-- _class: content-steps -->

## 導入ロードマップ: 3つのフェーズ

<div class="steps-container">

<div class="step-item">
<div class="step-number">1</div>
<div class="step-content">

### 準備フェーズ (1〜2ヶ月)
経営層の合意形成、パイロットチームの選定、基礎トレーニングの実施、ツール選定と環境構築を行います。

</div>
</div>

<div class="step-item">
<div class="step-number">2</div>
<div class="step-content">

### 実験フェーズ (3〜6ヶ月)
最初の2〜3スプリントを実施し、問題点を洗い出します。メトリクスを収集し、定期的に振り返りを行いながら改善します。

</div>
</div>

<div class="step-item">
<div class="step-number">3</div>
<div class="step-content">

### 展開フェーズ (6ヶ月〜1年)
成功事例を横展開し、他チームへの段階的な導入を進めます。組織全体のマインドセットを変革します。

</div>
</div>

</div>

---

<!-- _class: content-list-panel -->

## よくある失敗パターンと対策

<div>

### 典型的な5つの失敗

1. **形だけのアジャイル**: セレモニーだけ導入し、本質を理解していない
2. **経営層の無理解**: トップダウンの圧力で現場が疲弊
3. **完璧主義**: 全てを一度に変えようとして失敗
4. **メトリクスの誤用**: ベロシティなどを個人評価に使う
5. **文化の無視**: 既存の組織文化と衝突

</div>

<div class="panel">

### 成功の鍵

**小さく始めて、学びながら成長する**

失敗を恐れずに実験し、振り返りから学ぶ文化を育てることが最も重要です。

完璧な計画よりも、行動と学習のサイクルを回すことを優先しましょう。

</div>

---

<!-- _class: section -->

# 成果指標とKPI
## 何を測るべきか

---

## 重要な4つのメトリクス

<div class="box-light">

**デプロイ頻度**: どれだけ頻繁に本番環境にデプロイできるか
週1回 → 1日数回へ

</div>

<div class="box-medium">

**変更のリードタイム**: コミットから本番反映までの時間
1週間 → 数時間へ

</div>

<div class="box-strong">

最も重要なのは「学習のスピード」である

</div>

---

<!-- _class: stats -->

## 実際の導入効果(平均値)

<div class="stats-container">

<div class="stat-item">
<div class="stat-number">3.2x</div>
<div class="stat-label">デプロイ頻度の向上</div>
</div>

<div class="stat-item">
<div class="stat-number">68%</div>
<div class="stat-label">バグ発生率の削減</div>
</div>

<div class="stat-item">
<div class="stat-number">+24%</div>
<div class="stat-label">従業員満足度の向上</div>
</div>

</div>

---

## テキスト強調とコードの活用

アジャイルの本質は<span class="highlight-primary">人とコミュニケーション</span>を中心に据えることです。

継続的な改善を通じて組織全体が進化します。

<span class="highlight-secondary">顧客価値の最大化</span>が最終目標です。

```python
# スプリント計画の例
class Sprint:
    def __init__(self, duration_weeks=2):
        self.duration = duration_weeks
        self.stories = []

    def add_story(self, story, points):
        self.stories.append({"story": story, "points": points})
```

---

## 主要な概念の比較表

| 手法 | リリースサイクル | フィードバック | 適用場面 |
|------|----------------|--------------|---------|
| ウォーターフォール | 6ヶ月〜1年 | リリース後のみ | 要件が確定している |
| アジャイル | 2週間〜1ヶ月 | スプリント毎 | 要件が変化する |
| DevOps | 毎日〜毎週 | リアルタイム | 継続的デリバリー |

---

<!-- _class: closing -->

# Thank You

質問はありますか？

contact@example.com
@your_twitter
