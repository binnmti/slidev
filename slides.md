---
theme: seriph
background: https://cover.sli.dev
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
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

# Alpha-Dojo

## 現状共有

### 1.5期生 松井 敏

---

# 自己紹介

- 👨 松井 敏(まつい びん)
- 👜 Codeer プログラマ(本業) & HACARUS C#&CI/CD メンター(副業)
- 🏆 Microsoft MVP for Developer Technologies 2012-2024
- 📚 Unity5 3Dゲーム開発講座 ユニティちゃんで作る本格アクションゲーム
- 💻 C#読書会主催、Greek Alphabet Software Academy TA
- ❤️ プログラム、マンガ、料理、睡眠、妻&子供

---

# 本日のプレゼン資料

- 全く本質的でない話ですが、今回のプレゼン資料は[Slidev](https://sli.dev/)で作っています。
- マークダウンで資料を作るのはとても良いなと思い、昨日初めて使ってみました。
- [Github Pages](https://binnmti.github.io/slidev)でホスティング出来るのも良さそうだと思いました(Thanks Yabuta)。

---

# プロジェクトの概要説明

- アルゴリズム問題を解いてレビューが出来るサービス。
  - LeetCodeやAtCodeは問題を解くだけだが、解いた後にレビュー出来るのが特徴
  - 計算量を入力することも出来る
  - 教える人と教えられる人とではUIが違う
- 1期生(テザーさん、安田さん、すみれさん、本庄さん)が作り始めた。
- 僕は、概要知らない時は「教える人と教えられる人のためのアルゴリズム問題サービスかな？」と思った

---

# 現状の説明

- 人員リソースの問題で2週間前ぐらいに急遽1.5期生の3人(田中さん、陶山さん、松井)がヘルプが入ることになった。
- 初日さわってみた感じ、世の中に使ってもらうサービスとしてはまだまだ課題が多いと感じた。
- ただ、3日ぐらい見ているうちに、まだそのフェーズではないと思い、ゴールを見直した
- 一旦ドッグフーディングすることを目標にした。それなら8割以上には来ていると感じた。
- 現時点ではスコープを狭めるため「アルファプロジェクトが使うためのもの」としている
  - 教える人：勝又さん、教えられる人：2期生

---

# デモ

---

# フィッシュフーディング

- 一通りみてもらった感じ、なんか使えそうという印象を持ってもらえたかも？
- 正直、機能面では運用でカバーしないと使えない箇所もある（DB書き換えとか）。
- また細かく見れば出来ないことも、入力したことが消えてしまうようなバグ残っていると思う
- そういう前提だと分かった上で、フィッシュフーディングとしてお試ししたもらうことは可能かなと
- 問題は運用で回避して使ってもらいつつ、都度バグ、要望は記述してもらえると助かります！

---

# 今後の目標

- とりあえず8月のお盆休み明けぐらいにドッグフーディング可能な状態を目標にしていた
- ただ、自分の中では今回のフィッシュフーディング=予定としていたドッグフーディングなので、細かい予定はまだ未定
- とりあえずやることは一杯あるので、要望を出してもらってトリアージュしながら対応していく予定

---

# プロジェクト進行についての所感

- 個人的には運用でカバーしてもフィッシュフーディングするのは良い方法だと思う
- 1.5期生のプロジェクトでそれぞれにロールをつけるのは個人的にはとても良かった。
- プロジェクトの進め方に正解はなし。チーム開発では常に相手に対するリスペクトは忘れないように。
