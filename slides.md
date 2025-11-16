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

# Visual StudioとVS Codeと A(I)

## 松井 敏

---

# 自己紹介

- 👨 松井 敏(まつい びん)
- 👨‍💻 フリーランスプログラマ
- 👜 某大学でSRE50% + HACARUSでC#エンジニア30% + ZenTechでAIエージェント30%
- 🏆 Microsoft MVP for Developer Technologies 2012-2025
- 📚 Unity5 3Dゲーム開発講座 ユニティちゃんで作る本格アクションゲーム
- 💻 C#読書会主催、Greek Alphabet Software Academy TA
- ❤️ プログラム、マンガ、料理、睡眠、妻&子供

---

# I(私)のVisual StudioへのAi(愛)

- 大体2006年頃から使用開始
- つまり約20年共に歩んできた
- 揺るぎないNo.1 IDE
- 「死んだら棺に入れて」と思っているｗ

---

# IとAI

## 2024年

- 9月頃では生成名前付けの役に立つ程度
- プロンプトを頑張ってアプリを作らせてみるも正直まだまだ

## 2025年初頭

- AIが生成するコードの質が徐々に向上
- 個人開発でAIとペアプログラミングを開始
- **Askモード**で出力されたコードをペーストする機会が増加

---

# AI Agent

## 2025年5月
- 2〜3日程度のタスクをAIと取り組むことに
- Askモードで出力されたコードでは何度か失敗が続く
- フラストレーションがたまってきた

## AI Agent

- 世の中で**Agentモード**が話題になり始めていた
- しかしVisual Studioにはなかなか実装されない
- VS Codeは既に搭載済みだった
- そこでVS Code の GitHub CopilotのAgent モードを試すことに

---

# AI Shock

- いきなりタスクの全コードを数分で出力
- それがほぼ想定通りだった
  - （今思えばたまたま？）
- 凄いというよりも強い憤りを感じた

<div class="text-center text-3xl my-8">
「ここまでやられたら、ちっとも面白くない」
</div>
<div class="text-center text-3xl my-8">
「むしろコードを書く楽しみを奪われる」
</div>

---

# Visual Studio’s Agent

- 徐々にVS Code + Agentモードにスライドしていく
- 待望のVisual Studioにも Agentモードが実装される
- しかし、期待外れの出来栄え...

---

# 二刀流

## Visual Studio と VS CodeのW開発

<div class="grid grid-cols-2 gap-4 mt-4">
<div class="bg-blue-50 p-4 rounded">

### VS Code + GitHub Copilot
- **Askモード**で仕様を固める
- **Agentモード**で実装　→　
- **Agentモード**でコード修正　→　

</div>
<div class="bg-green-50 p-4 rounded">

### Visual Studio
- コードリーディング
- ← 実行と確認
- ↓ コミット・PR作成

</div>
</div>


```mermaid
graph LR
    A[VS Code<br>Askモード] --> C[VS Code<br>Agentモード]
    C --> D[Visual Studio<br>コードチェック・実行]
    D --> E{完成?}
    E -->|No| C
    E -->|Yes| F[Visual Studio<br>Push・PR]
    F --> G[CodeRabbit&Copilot<br>レビュー]
    G -->|No| C
    G -->|Yes| Merge
```


---

# AI Model

## Anthropic is No.1

- 初期のAgenticコーディングはGPTを使用
- Copilotで選べるようになった**Claude Sonnet 3.5**がかなり良かった
- その後様々なモデルを試すも、個人的にはずっとClaude Sonnetが最強
- Visual Studio Code InsidersでCodexも試したが、個人的にはやっぱりClaude Sonnet

## Claude Code

- 使ってみたが日本語入力時の場所移動が嫌すぎる
- Visual Studio Codeの公式ExtentionでGUI対応
- 徐々にGitHub CopilotとClaude Codeの二刀流に

---

# Claude Code vs GitHub Copilot
- Claude Code vs GitHub CopilotでほぼどちらもClaude Sonnet4.5
- プロンプトを繰り返すと大差ない印象
- ただClaude Codeの方が**ファーストプロンプトの精度が顕著に高い**
- Claude Code Proだったが、10回ぐらいリミットが来たのでいよいよMaxに課金した

---

# ファーストプロンプト

- AI開発で最初の１回はとても重要
- Planモードでプロンプトを作ってAgentモードにそのプロンプトを投げるが王道
- この精度をさらに上げるのに、各AIに渡すClaude.md, copilot-instructions.mdは重要
- またMCPも初回が一番把握してくれている気がする
- 個人的にはこの内容はプロンプトを繰り返すごとにAIが忘れていくように感じる

---

# 並列開発

<div class="text-center text-2xl my-4">
きっかけ：<strong>Agentモード中の隙間時間</strong>
</div>

- Agent中は別の事した方が効率的ではある。が、自分はマルチタスクがとても苦手
- 仕事中に、ふと他も出来たら効率的だなと思った瞬間があった
- ちょうどCursor2.0で並列開発の機能が乗った
- ある勉強会でAnthropic社員の基調講演でも並列開発の話になった
- git worktree + tmuxがよさげ

---

# git worktree

- 並列開発するのであればブランチでは衝突するので複数リポジトリが想定される
- 実際過去よく複数リポジトリ作って切り替えていたこともある
- ただ数分だけSSDも圧迫するし、管理も間違えやすい
- もっといい機能がgitにあるんじゃない？
- それがgit worktree。
- 好きな別フォルダに全コピーされ、.gitは元と共有。
- commit pushは可能。git worktree removeで削除

---

# 実験その１　Visual Studio Code Insiders

- Codex, Claude Code, GitHub Copilotに全く同じ作業を振ってみた
- フォルダだけ作ってその下に作ってもらうように指示
- 3つのチャットを読んでいくだけでも結構脳の負荷が高い

---

# 実験その2　Cursor並列開発マルチモデル

- CursorでClaude Code, GPT5-codex, Composeで並列開発
- UI未確定時にたまにやるようになった
- 各worktreeの比較が難しい
- Visual Studioで一つずつ確認
- CursorでClaude Code×4は正直殆ど変わらないのであんまり。
  - でもこの並列数を上げると突然変異も生まれるらしい
- なるべく自動実行の仕組みが欲しい


---


# まとめ：開発スタイルの進化

## 3つのフェーズ

<div class="grid grid-cols-3 gap-2 mt-4 text-sm">
<div class="bg-gray-100 p-4 rounded">

### Phase 1
**Visual Studio一筋**
- 2006-2024
- IDEこそ正義

</div>
<div class="bg-blue-100 p-4 rounded">

### Phase 2
**Visual Studio+VS CodeのW体制**
- 2025年前半
- VS Code Agent + Visual Studio
- AIとのペアプロ

</div>
<div class="bg-green-100 p-4 rounded">

### Phase 3
**並列開発**

- 2025年11月〜
- 複数モデル活用
- VS Code, VsCode Insider, Cursor
- まだスタイルは確定していない

</div>
</div>


---

# 今後はまだ迷い中

- 並列開発は重要なキーワードだがスタイルは模索中
  - KAMUIが面白そうで100並列も検討中
  - 今後は1000並列の使い方も
  - Github Actionsの並列も
- ファーストプロンプトは、SpecDDが流行っている
  - まださわってないけれどKiro
  - GitHubもSpec Kitを作っている
- LLMをLLMで評価するLLM-as-a-Judge
  - 結局良し悪しは個人の好みになりがちなのでAIに判断してもらう

---

# 最重要補足

- 今までのAgentモード
- Claude Code > VS Code >>>>>>>> Visual Studio（論外）
- Visual Studio2022で検証、数カ月に１回程度試しても進化はせず
- 先週Visual Studio2026でのAgentモード再検証
- Claude Code > VS Code > Visual Studio
- また検討に帰ってきた我が相棒

---
