---
marp: true
theme: claude
paginate: true
size: 16:9
---

<style>
/* 画像は後から差し替えるため、確定するまで枠と撮影内容だけを置く */
.shot {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  min-height: 400px;
  border: 1px dashed var(--claude-muted-soft);
  padding: 24px;
  text-align: center;
}

.shot-label {
  color: var(--claude-coral);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.shot-note {
  max-width: 82%;
  color: var(--claude-muted);
  font-size: 15px;
  line-height: 1.6;
}

/* 送信量の増加は面ではなく線の長さで示す */
.turns {
  margin-top: var(--claude-space-2);
}

.turn {
  display: grid;
  grid-template-columns: 96px 1fr 280px;
  align-items: center;
  gap: 24px;
  border-top: 1px solid var(--claude-hairline);
  padding: 26px 0;
}

/* 同じ長さのバーを並べて比べるため、ラベル列を広く取る */
.turns.compare .turn {
  grid-template-columns: 140px 1fr 340px;
}

.turn-no {
  color: var(--claude-muted);
  font-size: var(--claude-fs-s);
}

.turn-bar {
  display: flex;
  align-items: center;
  gap: 4px;
}

.turn-bar i {
  display: block;
  height: 10px;
}

/* Marpit が style 属性を落とすため、履歴の長さはクラスで持たせる */
.turn-bar .past {
  background: #ddd3c1;
}

.turn-bar .past.w1 {
  width: 130px;
}

.turn-bar .past.w2 {
  width: 340px;
}

.turn-bar .past.w3 {
  width: 550px;
}

.turn-bar .now {
  width: 130px;
  background: var(--claude-coral);
}

.turn-note {
  color: var(--claude-body);
  font-size: var(--claude-fs-xs);
}

.legend {
  display: flex;
  gap: var(--claude-space-3);
  margin-top: var(--claude-space-2);
  color: var(--claude-muted);
  font-size: 15px;
}

.legend span {
  display: flex;
  align-items: center;
  gap: 8px;
}

.legend i {
  display: block;
  width: 28px;
  height: 10px;
}

.legend .past {
  background: #ddd3c1;
}

.legend .now {
  background: var(--claude-coral);
}

.bar {
  height: 10px;
  background: #ddd3c1;
}

.bars {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: var(--claude-space-3) 0 12px;
}

.bars .bar {
  flex: 1;
}

/* 会話の切れ目は短い縦バーで示す */
.cut {
  display: block;
  width: 2px;
  height: 28px;
  background: var(--claude-coral);
}

.bar-caption {
  display: flex;
  gap: 12px;
  margin-bottom: var(--claude-space-3);
  color: var(--claude-muted);
  font-size: 15px;
}

.bar-caption span {
  flex: 1;
  text-align: center;
}

.flow-note {
  color: var(--claude-muted);
  font-size: 15px;
}

/* グリッドのレイアウトでは補足が 1 列目に落ちるため、全幅に戻す */
section.content-2col-comparison > .caption,
section.content-3col > .caption,
section.content-steps > .caption {
  grid-column: 1 / -1;
  margin-top: var(--claude-space-3);
}
</style>

<!-- _class: title -->

# Claude Code を使い倒す

---

## プロフィール

- Claude Code は 2025 年 6 月末から使用

---

<!-- _class: content-center -->

# こんな経験ありますか？

---

<!-- _class: content-center -->

# 「指示しても守ってくれない！」

---

<!-- _class: content-center -->

## 原因はモデルの性能ではなく、<br> 会話履歴の長さにあります。

やり取りが長くなるほど過去の履歴が増加し、必要な指示が埋もれてしまいます。

---

<!-- _class: agenda -->

## 本資料のアジェンダ

1. なぜ指示が埋もれるのか
2. 依頼の記述方法を見直す
3. 会話セッションを適切に分割する
4. 意図がずれた際の巻き戻し
5. 渡す情報量の最適化

---

<!-- _class: section -->

# 1. なぜ指示が埋もれるのか

## 会話履歴は毎回まとめてモデルへ送信される

---

## 会話が進むほど、毎回送信するデータ量が増加する

<div class="turns">

<div class="turn">
<span class="turn-no">1 回目</span>
<span class="turn-bar"><i class="now"></i></span>
<span class="turn-note">指示のみを送信</span>
</div>

<div class="turn">
<span class="turn-no">2 回目</span>
<span class="turn-bar"><i class="past w1"></i><i class="now"></i></span>
<span class="turn-note">過去 1 回分のやり取りを含めて送信</span>
</div>

<div class="turn">
<span class="turn-no">3 回目</span>
<span class="turn-bar"><i class="past w2"></i><i class="now"></i></span>
<span class="turn-note">過去 2 回分のやり取りを含めて送信</span>
</div>

<div class="turn">
<span class="turn-no">4 回目</span>
<span class="turn-bar"><i class="past w3"></i><i class="now"></i></span>
<span class="turn-note">過去 3 回分のやり取りを含めて送信</span>
</div>

</div>

<div class="legend">
<span><i class="now"></i>今回の指示</span>
<span><i class="past"></i>これまでの会話履歴およびシステムプロンプト</span>
</div>

---

## キャッシュが有効でも、送信する履歴の長さは変わらない

<div class="turns compare">

<div class="turn">
<span class="turn-no">キャッシュあり</span>
<span class="turn-bar"><i class="past w2"></i><i class="now"></i></span>
<span class="turn-note">変更のない箇所の処理を再利用する。応答は速い</span>
</div>

<div class="turn">
<span class="turn-no">キャッシュなし</span>
<span class="turn-bar"><i class="past w2"></i><i class="now"></i></span>
<span class="turn-note">履歴を最初から再処理する。応答に時間を要する</span>
</div>

</div>

<div class="box-medium">

どちらの場合もモデルへ送信されるデータ量は同一です。<br>キャッシュは処理を高速化する仕組みであり、コンテキストの圧迫自体は防げません。

</div>

---

<!-- _class: content-3col -->

## キャッシュが無効化される 3 つの要因

<div>

### 時間の経過

メインの会話は 1 時間、通常のリクエストは 5 分で期限切れとなります。

→ 次回リクエスト時に履歴が再処理されます

</div>

<div>

### モデルの切り替え

会話の途中で別のモデルに変更した場合です。

→ 対象の会話のキャッシュは引き継がれません

</div>

<div>

### effort の変更

会話の途中で effort の設定値を変更した場合です。

→ 多くのモデルでキャッシュが無効になります

</div>

---

<!-- _class: callout -->

## コンテキストは計画的に管理する

タスク着手時にモデルと effort を確定し、不要な変更を避けます。<br>以降の 4 つの実践手法により、コンテキストを適切に保ちます。

---

<!-- _class: section -->

# 2. 依頼の記述方法を見直す

## モデルへの解釈の依存度を下げる

---

<!-- _class: content-2col-comparison -->

## 5 つの構成要素を定義し、作業範囲を依頼時点で確定させる

<div>

### 曖昧な指示

```text
ログイン周りのバリデーション直しておいて
```

改修の判断をモデルに委ねると、想定外に API 側まで変更が及ぶ恐れがあります。

</div>

<div>

### 目的と完了条件の明記

```text
# 目的
入力値検証を仕様どおりにする
# 範囲
src/features/login/ 配下のみ
# 進め方
方針を提示してから実装
# 制約
既存テストを破損させない
# 完了条件
pnpm test が成功し、空白のみの入力を除外する
```

</div>

<p class="caption">構成要素は「目的・範囲・進め方・制約・完了条件」です。日常的な質問で毎回すべてを記述する必要はありませんが、実装範囲や達成基準が曖昧になりやすいタスクで有効です。</p>

---

<!-- _class: content-dark-code -->

## 要件が未整理な場合は、定型プロンプトで逆質問させる

<div>

### 期待される応答

前提条件の不足が質問形式で提示されます。

- 対象は既存画面の改修か、新規作成か
- 認証は既存の仕組みを利用するか（推奨: 利用する。理由は…）

番号で選択するだけで、要件定義を円滑に進められます。

</div>

<div>

<div class="status-line">● 要件定義を促す定型プロンプト</div>

```text
適切なアウトプットを得るために、
確認が必要な事項を厳選して質問してください。
各質問には推奨案と理由を添え、
選択肢は番号で提示してください。
```

</div>

---

<!-- _class: content-2col-comparison -->

## 指示を羅列するよりも、模範例を 1 つ提示する

<div>

### 指示を羅列した場合

「箇条書きを用い、丁寧な敬語で、過度に堅苦しくならず、部署名は正式名称を使い、個人名は記載しすぎない形で議事録を作成してください」

</div>

<div>

### 模範例を指定した場合

「過去に作成した `meeting-2026-05-01.md` と同じトーンで議事録を作成してください」

</div>

---

<!-- _class: content-steps -->

## 模範例がない場合は、一度作成して保存・再利用する

<div class="steps-container">

<div class="step-item">
<div class="step-number">01</div>
<div class="step-content">

### 対話形式で完成させる

期待どおりの出力が得られるまで、その場でフィードバックを重ねて基準となる成果物を 1 つ完成させます。

</div>
</div>

<div class="step-item">
<div class="step-number">02</div>
<div class="step-content">

### ファイルとして保管する

`meeting-2026-05-01.md` や `UserService.ts` のように、次回以降参照可能な場所に保存します。

</div>
</div>

<div class="step-item">
<div class="step-number">03</div>
<div class="step-content">

### 次回以降の指示で参照する

「`UserService.ts` と同じスタイルで実装してください」と指定します。

</div>
</div>

</div>

<p class="caption">模範例を作成する過程でも会話履歴は蓄積されます。成果物が完成した段階で、セッションを忘れずに初期化します。</p>

---

<!-- _class: section -->

# 3. 会話セッションを適切に分割する

## 決定事項はチャットではなくファイルに記録する

---

<!-- _class: content-2col-comparison -->

## 単一の長い会話を避け、タスク単位でセッションを分割する

<div>

### 同一セッションを継続した場合

<div class="bars">
<div class="bar"></div>
</div>

<div class="bar-caption"><span>最初から最後まで 1 つのセッション</span></div>

<p class="flow-note">決定事項がチャット履歴に残り、初期の指示が後半に埋もれてしまいます。</p>

</div>

<div>

### タスクごとに分割した場合

<div class="bars">
<div class="bar"></div><span class="cut"></span><div class="bar"></div><span class="cut"></span><div class="bar"></div>
</div>

<div class="bar-caption"><span>タスク 1</span><span>タスク 2</span><span>タスク 3</span></div>

<p class="flow-note">区切りの段階で決定事項をファイルへ出力して <code>/clear</code> を実行します。後続の会話を最小限のコンテキストから再開できます。</p>

</div>

---

<!-- _class: content-steps -->

## 決定事項をファイルに保存したうえで `/clear` を実行する

<div class="steps-container">

<div class="step-item">
<div class="step-number">01</div>
<div class="step-content">

### 決定事項の外部記録

確定した設計や要件は、Issue や Markdown に記録します。

</div>
</div>

<div class="step-item">
<div class="step-number">02</div>
<div class="step-content">

### 引継ぎプロンプトの生成

次回用の指示文を生成させ、クリップボードへ格納します。

</div>
</div>

<div class="step-item">
<div class="step-number">03</div>
<div class="step-content">

### `/clear` による初期化

新しいセッションに貼り付け、不要な履歴を引き継がずに再開します。

</div>
</div>

</div>

---

<!-- _class: content-dark-code -->

## 引継ぎ用の指示文は簡潔にまとめる

<div>

### セッション終了直前の 1 行指示

次回指示文をクリップボードに保存し、<br>`/clear` 後に貼り付けて再開します。<br>決定事項はファイルや Issue に残します。

</div>

<div>

<div class="status-line">● macOS のプロンプト例</div>

```text
次のセッションで実行すべき指示文を簡潔に作成し、
pbcopy コマンドでクリップボードに格納してください。
```

</div>

---

## 基本は `/clear` を利用し、文脈の維持が必要な場面に限り `/compact` を選ぶ

| 状況 | 選択するコマンド | 結果 |
| --- | --- | --- |
| 決定事項を外部ファイルに保存済み | `/clear` | 過去の履歴を破棄し、クリーンな状態で作業を再開する |
| 同一タスクの文脈を維持したまま継続したい | `/compact` | 過去の履歴を要約してコンテキストを圧縮する |

<p class="caption"><code>/compact</code> は要約処理によって詳細情報が欠落する可能性があります。また、長時間のアイドル後に実行すると、要約生成のために履歴全体の再処理が発生する場合もあります。設計や要件を事前に外部ファイルへ記録しておけば、大半の業務は <code>/clear</code> で十分に対応可能です。</p>

---

<!-- _class: section -->

# 4. 意図がずれた際の巻き戻し

## 追加指示の重ねがけは、履歴の無駄な増大を招く

---

<!-- _class: content-image-right -->

## 実装着手前に計画を立案させる

<div>

Claude Code は指示を受けると即座に実装へ進む傾向があります。想定と異なる方針で進んだ場合、消費トークンと作業時間の双方でロスが生じます。

- `Shift+Tab` で Plan mode を選択する
- 個別の依頼プロンプトに `/plan` を付与する
- 未整理の要件は `/grill-me` で質問させる

</div>

<div>

<div class="shot">
<div class="shot-label">画像 TODO</div>
<div class="shot-note">Plan mode で実装計画が提示され、承認待ちの画面。<code>Shift+Tab</code> のモード表示が確認できるように撮影する。</div>
</div>

</div>

---

<!-- _class: content-list-panel -->

## 策定された計画をそのまま適用せず、検証を行う

<div>

### 客観的な視点による検証

立案された計画には検討不足な点が含まれる可能性があります。作成した計画をサブエージェントに検証させ、実装着手前に懸念事項を洗い出します。

</div>

<div class="panel">

### 指示プロンプトの例

計画ファイルをシニアエンジニア役のサブエージェントに検証させ、妥当な指摘を反映してください。迷う論点は自己判断せず利用者に確認してください。

</div>

---

<!-- _class: content-2col-comparison -->

## 意図と違ったら、指示を重ねずに巻き戻す

<div>

### 指示を重ねるといつまでも食い違う…

```text
私「議事録作成して」
🤖「はい、作成しました」
私「これ、敬語が丁寧口調すぎるな…もうちょいフレンドリーにして。」
🤖「はい、フレンドリーにしました(もはやタメ口)」
私「え？フレンドリーすぎん？相手はお客様で取引相手だよ？」
私「なんか思ってたんと違うなぁ….」
```

</div>

<div>

### 過去に戻ってなかったことにする！

```text
私「議事録作成して」
🤖「はい、作成しました」

// このタイミングでエスケープキー 2 回(または `/rewind`)で
　　最初の指示まで戻す

私「議事録作成して。相手はお客様で取引相手です。
　　敬語がベースだけど、堅苦しくなりすぎず、過去に作成した
　　`meeting-2026-05-01.md` くらいの少しだけフレンドリーな
　　口調で作成して。」
🤖「はい、作成しました」
```

</div>

---

<!-- _class: content-image-right -->

## Esc 2 回押下または `/rewind` でセッションを巻き戻す

<div>

エスケープキーを 2 回押すか、`/rewind` を実行して過去の会話地点まで状態を巻き戻します。

想定と異なる回答が返された場合は追加指示で修正せず、最初の指示まで巻き戻して要件を再整理します。模範例を併せて提示すると、より確実に出力を制御できます。

</div>

<div>

![image](https://pub-151065dba8464e6982571edb9ce95445.r2.dev/images/c4bab60837c4dcdd0f3a378149b5ae57.png)

</div>

---

<!-- _class: section -->

# 5. 提供する情報量の最適化

## プロンプトに含める情報を必要最小限に絞り込む

---

## タスクの性質に応じたモデル選定

| 場面 | 推奨モデル |
| --- | --- |
| 関連ファイルの探索、簡潔な要約 | 軽量モデル（Haiku など） |
| コードベースの調査、一般的な実装 | Sonnet |
| アーキテクチャ設計、要件定義、複雑な実装 | Opus / Fable |

<p class="caption">サブエージェントも同様の挙動となります。モデルが明示されていないサブエージェントは、メインセッションのモデル設定を継承します。簡易なタスクと判定された場合に限り、公式の Explorer サブエージェント（Haiku）が呼び出されます。</p>

---

<!-- _class: content-dark-code -->

## 調査用途のサブエージェントには軽量モデルを指定する

<div>

### エージェント定義ファイルでの model 指定

`.claude/agents/` 配下の定義ファイルで、利用するモデルを指定できます。指定可能なモデル名は随時更新されるため、導入時に最新の公式情報を確認します。

</div>

<div>

<div class="status-line">● .claude/agents/quick-investigator.md</div>

```markdown
---
name: quick-investigator
description: 関連ファイルの調査・特定に使用
tools: Read, Grep, Glob
model: haiku
---

関連するファイル名と選定根拠を明記し、
調査結果を簡潔に報告してください。
```

</div>

---

<!-- _class: content-2col-comparison -->

## ログデータはエラー周辺の該当行に絞り込んでから提示する

<div>

### ログ全体をそのまま入力した場合

```text
ビルドログ全体を会話に貼り付ける
```

無関係な出力行までコンテキストに含まれ、必要な情報が埋もれてしまいます。

</div>

<div>

### 対象範囲を抽出して入力した場合

```bash
rg -n -C 2 "ERROR|WARN" \
  build.log > relevant-lines.log
```

抽出した `relevant-lines.log` のみを渡します。`-C 2` で前後 2 行を含めて文脈を保持できます。

</div>

---

<!-- _class: content-3col -->

## 不要または重複しているツール定義を定期的に整理する

<div>

### 未使用のツール

登録後に一度も呼び出されていない MCP サーバーや拡張スキル。

</div>

<div>

### 機能の重複

同様の処理を行うツールが複数登録された状態。

</div>

<div>

### 接続エラーの残存

接続や認証に失敗した設定が残っている状態。

</div>

<p class="caption">対応環境において MCP のツール定義はオンデマンドで検索されるため、登録したすべての定義が即座にコンテキストを消費するわけではありません。<br>ただし不要な定義を残しておくと、モデルによる誤認識や意図しないツール呼び出しの原因となり得ます。</p>

---

<!-- _class: content-2col-comparison -->

## CLAUDE.md には全体で常に共有すべき情報のみを記載する

<div>

### 記載が不要な内容

```text
slides/ にスライドが入っている
themes/company.css がテーマ本体
```

コードベースを 1 ファイル確認すれば把握できる構造情報は、必要時に Claude Code 自身が検索して判別できます。

</div>

<div>

### 記載すべき内容

```text
テストは pnpm test で実行する
色は themes/ の CSS 変数以外に足さない
```

固有の開発ルール、テスト実行手順、コードからは読み取れない設計上の決定事項など。

</div>

<p class="caption">CLAUDE.md はセッション開始時に毎回読み込まれます。過剰に記載するとプロンプトが無駄に長くなり、必要な指示が認識されにくくなります。</p>

---

<!-- _class: content-dark-code -->

## 自動メモリ機能を無効化する

<div>

### 完了した作業の前提条件を持ち越さない

自動メモリ機能は初期状態で有効になっています。軽微な情報まで記録されたり、完了したタスクの前提条件が別の会話セッションへ混入したりするのを防ぐため、明示的にオフに設定します。

環境変数でも無効化できます。

`CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`

</div>

<div>

<div class="status-line">● ~/.claude/settings.json</div>

```json
{
  "promptCacheTtl": "1h",
  "subagentPromptCacheTtl": "1h",
  "autoMemoryEnabled": false
}
```

</div>

---

<!-- _class: content-image-right -->

## 仕様の確認には公式ドキュメントの対話機能を活用する

<div>

Claude Code の公式ドキュメントには、仕様や制約を対話形式で質問できる機能が備わっています。
該当ドキュメントへのリンクも提示されるため、詳細を暗記する必要はありません。

設定変更の前に、まず公式リファレンスの確認をお勧めします。

https://code.claude.com/docs/en/overview


</div>

![image](https://pub-151065dba8464e6982571edb9ce95445.r2.dev/images/ed0cae84fb43e66131583d4156f6c4fd.png)

</div>

---

<!-- _class: summary -->

## まとめ: 会話コンテキストの適切な管理

<div class="summary-items">

<div class="summary-item">

### 履歴の増大を前提とする

キャッシュは処理を高速化しますが、コンテキストの圧迫自体は防げません。

</div>

<div class="summary-item">

### タスクごとに分割する

決定事項をファイルに記録して `/clear` を実行し、次回指示文を用意します。

</div>

<div class="summary-item">

### 情報量を最適化する

指示文、選択モデル、ツール実行結果、CLAUDE.md、MCP 定義などを必要最小限に厳選します。

</div>

</div>

---

<!-- _class: summary -->

## まとめ: 出力の乖離（ずれ）を防ぐ手法

<div class="summary-items">

<div class="summary-item">

### 模範例を提示する

条件の羅列よりも、基準となる模範例を示すほうが意図どおりの精度を得られます。

</div>

<div class="summary-item">

### 事前計画と客観的検証

着手前に計画を立案し、サブエージェントによる検証で手戻りを防ぎます。

</div>

<div class="summary-item">

### 早期の巻き戻し

意図とずれたら指示を重ねず、`/rewind` 等で過去の地点へ巻き戻します。

</div>

</div>

---

## 参考資料

- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- [How Claude Code uses prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Create custom subagents](https://code.claude.com/docs/en/sub-agents)
- [Connect Claude Code to tools via MCP](https://code.claude.com/docs/en/mcp)
- [How Claude remembers your project](https://code.claude.com/docs/en/memory)
- [今更だけどよくやる Claude Code 活用法](https://suntory-n-water.com/blog/claude-code-context-management-tips)

<p class="caption">仕様は 2026 年 9 月 14 日時点の公式ドキュメントに基づきます。キャッシュ TTL の設定には Claude Code v2.1.242 以降が必要です。</p>
