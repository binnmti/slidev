---
theme: seriph
background: https://cover.sli.dev
title: AIとコードレビュー
info: |
  ## AIとコードレビュー
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
mdc: true
---

<style>
h1 {
  background-color: #f0fff0;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

# AIとコードレビュー

## 今のAIに合わせるか、未来のAIを見据えるか

松井 敏

---

# 自己紹介

- 👨 松井 敏(まつい びん)
- 👨‍💻 フリーランスプログラマ
- 👜 某大学でSRE50% + [HACARUS](https://hacarus.com/ja/)でC#エンジニア30% + [ZenTech](https://www.zen-tech.co.jp/company)でAIエージェントエンジニア30%
- 🏆 [Microsoft MVP for Developer Technologies 2012-2025](https://mvp.microsoft.com/en-US/MVP/profile/f8610ff3-3c9a-e411-93f2-9cb65495d3c4)
- 📚 [Unity5 3Dゲーム開発講座 ユニティちゃんで作る本格アクションゲーム](https://amzn.to/47YnopE)
- 💻 [C#読書会主催](https://cs-reading.connpass.com/)、[Greek Alphabet Software Academy TA](https://greek-academy.org/)
- ❤️ プログラム、マンガ、料理、睡眠、妻&子供

---

# 今日話すこと

1. **AIの完成度をどう捉えるか** — 哲学の話
2. **仕事でのコードレビュー** — 10-20%を人間が埋める
3. **ローカルコードレビューのテクニック** — 実践編
4. **個人開発でのコードレビュー** — 未来のAIに全振り

---
layout: center
class: text-center
---

<div class="text-6xl font-bold">
AIをどこまで信じるか
</div>

---


# 今のAIに投資 vs 未来のAIに投資

<div class="grid grid-cols-2 gap-8 mt-8">
<div class="bg-blue-50 p-6 rounded">

### 今のAIに投資
- AIの完成度は体感 **80-90%**
- 残り **10-20%** は人間が埋める必要がある
- 変更容易性・メンテナビリティを人間が担保
- **仕事ではこちらを採用**

</div>
<div class="bg-green-50 p-6 rounded">

### 未来のAIに投資
- 完成度は限りなく **100%** に近づく
- 10-20%の部分もスキップできる
- ひどいコードも未来のAIが自動リファクタリング
- **個人開発ではこちらを採用**

</div>
</div>

<div class="text-center text-2xl mt-8">

この選択によって、コードレビューへのアプローチが根本的に変わる

</div>

---
layout: center
class: text-center
---

<div class="text-6xl font-bold">
仕事でのコードレビュー
</div>
<div class="text-3xl mt-4">
今のAIに合わせる — 10-20%を埋める
</div>

---

# コードレビューは必須

- コミット前にコードを目で読む
- PRに関してもしっかり目を通す
- **自分が100%と思えるところまで修正を加える**
- 全体から見た最適なコードをAIはまだ100%では書けない
- PRレビューがなくなるという感覚は持っていない

<div class="mt-8">

## AIレビューもマスト
- PRレビューでは **CodeRabbit** が品質No.1（過去比較で圧倒的）
- GitHub Copilotのレビューはかなりレベルが低かった
- ただし現時点での最新比較はまだ未実施

</div>

---

# ローカルレビュー > PRレビュー

<div class="text-xl mt-4">

- PR上で回すのもローカルで回すのも、実質的にはほとんど差分がない
- であれば **他の人にも見えるPR上で直すよりも、ローカルで修正して都度レビュー** の方が効率的
- ローカルなら1人でも100人でも変わらない — **複数AIレビュー**が可能

</div>

<div class="grid grid-cols-2 gap-4 mt-8">
<div class="bg-blue-50 p-4 rounded">

### Claude Code
- コードを書いたAI自身にレビューも頼む
- 「書いた本人がレビューする意味あるか？」
- → 視点・立場の違いでクオリティは上がる

</div>
<div class="bg-green-50 p-4 rounded">

### Codex
- `-p`オプションでCLIから直接レビュー依頼
- 体感的にClaude Codeよりレビュー品質が高い
- インタラクティブモードではなくワンショット

</div>
</div>

---

# コードレビューの粒度：差分 vs ファイル vs プロジェクト

<div class="mt-4">

| 粒度 | クオリティ | トレードオフ |
|------|-----------|-------------|
| **差分単位** | やや低い | 見つけられない問題がある |
| **ファイル単位** | 高い（推奨） | バランスが良い |
| **プロジェクト全体** | 理論上は最高 | コンテキスト・パフォーマンス問題 |

</div>

<div class="text-xl mt-8">

- ファイル単位で見てもらうことで、差分では見つけられない指摘をちゃんとしてくれた
- **変更のあったファイル全体を渡す**のが現時点でのベストプラクティス

</div>

---

# プロンプトとタイミングの工夫

## `/review` だけでは足りない

- スラッシュレビューだけではほとんど有益な指摘は得られなかった
- 「こういう観点で見てほしい」という前提をしっかり入れる必要がある

## レビューのタイミング

- **コミット前レビュー** + **全コミットに対する最終レビュー** が個人的なフロー
- コミット単位の自動レビューでは深い指摘が得られにくい印象

## 自動 vs 手動（若干の敗北）

- スラッシュコマンドで完全自動化するより、手動でCodexを起動してプロンプトを打った方が指摘の質が高い
- 自分のコントロール下でやる方がクオリティは高いと感じている
- 完全自動化はまだ道半ば

---
layout: center
class: text-center
---

<div class="text-6xl font-bold">
個人開発でのコードレビュー
</div>
<div class="text-3xl mt-4">
未来のAIを見据える — 100%信じる
</div>

---

# 人間のコードレビューをなくす

<div class="text-xl">

- 未来のAIは自動リファクタリング、設計思想に基づく全書き換えもできるようになる
- 仮にひどいコードが上がっていても、後からAIが直してくれる前提
- 人間がAIの残り20%を埋める必要はない
- **やりたいことをどんどん伝えてクリアしていく方が正しい**

</div>

---

## 実際のフロー

- ブランチを切らない、PRもしない
- **mainにどんどんコミットを積んでいく**
- コミット・プッシュもほぼ全部AIに任せる
- ローカルレビューも自動化済み

<div class="text-xl mt-4 p-4 bg-yellow-100 rounded">

ただしUI/UXのレビューは自分の目でしっかりやっている。<br>
**特にUXレビューに関しては未来のAIにもそれほど期待していない。**<br>
この部分は自分自身のこだわりとして持ち続けるつもりでいる。

</div>

---

# ただし仕組みは必要

<div class="text-xl mt-4">

- コードレビューをスキップするとはいえ、**AIがしっかりレビューできる仕組み**は必要
- 今日話したテクニックは個人開発でも全部採用している

</div>

```mermaid
graph LR
    A[Claude Code<br>コード生成] --> B[Claude Code<br>レビュー]
    B --> C[Codex -p<br>レビュー]
    C --> D[Claude Code<br>コミット、mainにプッシュ]
```

<div class="text-xl mt-4">

- 違いは**人間が最終チェックで止めるかどうか**だけ
- 仕事：人間が止めて100%を担保する
- 個人開発：AIに任せて流す

</div>

---

# CodeRabbit CLIの課題

- CodeRabbitはPRレビューで最高品質だが、ローカルでも使いたい
- **Windows版CLIがない**（Mac/Linuxのみ）
- WSLで入れることは可能だが…
  - ログインの自動化が不安定
  - 再起動や時間経過で再ログインが必要になる
  - レビューフローに安定して組み込めなかった
- ローカルレビューのパイプラインに組み込めれば最強なのだが…

---

# まとめ

<div class="grid grid-cols-2 gap-8 mt-4">
<div class="bg-blue-100 p-6 rounded">

### 仕事（今のAIに合わせる）
- AIの完成度は80-90%
- コードレビューは必須、人間が10-20%を埋める
- ローカルで複数AIレビュー（Claude Code + Codex）
- ファイル単位でレビュー、プロンプトは丁寧に
- 最終チェックは人間の目で

</div>
<div class="bg-green-100 p-6 rounded">

### 個人開発（未来のAIを見据える）
- AIの完成度は100%に近づく前提
- 人間のコードレビューは不要
- mainに直接プッシュし続ける
- AIレビューの仕組みは自動化
- **ただしUI/UXレビューは自分の目で**

</div>
</div>

<div class="text-center text-2xl mt-8">

**AIをどこまで信じるかで、コードレビューのあり方は根本から変わる**

</div>

