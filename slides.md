---
theme: default
background: https://source.unsplash.com/1920x1080/?programming,code
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## AIと一緒にC#を書く時代のワークフロー
  
  GitHub CopilotとVS Code/Visual Studioを使った効率的な開発スタイル
drawings:
  persist: false
transition: slide-left
title: AIと一緒にC#を書く時代のワークフロー
mdc: true

---

# AIと一緒にC#を書く時代のワークフロー

最近の自分の開発スタイル  
「AIと一緒にコードを書く」ことが前提

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/binnmti" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---

# 自己紹介

- 👨 松井 敏(まつい びん)
- 👨‍💻 フリーランスプログラマ
  - C#を中心に、ゲーム・Windowsアプリ・Webなど幅広く開発
  - 自動化、CI/CD、SREにも携わり、開発から運用まで得意。
  - 講師や技術書執筆、技術顧問なども。
  - モノづくりが大好きなので、誰かの作りたいをサポートします！
- 🏆 Microsoft MVP for Developer Technologies 2012-2025
- 📚 Unity5 3Dゲーム開発講座 ユニティちゃんで作る本格アクションゲーム
- 💻 C#読書会主催、Greek Alphabet Software Academy TA
- ❤️ プログラム、マンガ、料理、睡眠、妻&子供

---

# 開発ツールの役割分担

<div class="grid grid-cols-2 gap-6 mt-6">

<div>

## Visual Studio 🔧
- **メインの開発環境**
- 実行・デバッグ・確認
- 最終的なコード整形
- GitHub Copilot Agentは使わない
  - 応答不能になりやすい
  - 最新モデルが使えない

</div>

<div>

## VS Code 💭  
- **AIとのやりとり**
- GitHub Copilot Ask/Agent
- 思考整理と壁打ち
- コード生成・修正
- 圧倒的に安定している

</div>

</div>

<div class="mt-6 text-center">
<v-click>

**つい最近まで Visual Studio 一択 → 今は二本柱**

</v-click>
</div>

---

# Askモードで思考整理

## 🤔 AIとの壁打ち
- いきなりAgentで書かせるのはギャンブル（成功率50%以下）
- まずはAskで「やりたいこと」を**乱文でもOK**で書き出す
- モデルは**Sonnet 4**中心（Opusとの行き来が面倒）

## 📝 ワークフロー
1. **思考を言語化** → Askに投げる
2. **コード付きの回答** → 何ターンか壁打ち
3. **方向性がずれたら** → コメントで軌道修正  
4. **話がまとまらない** → ゼロから書き直し
5. **方向性が見えたら** → 「今までの流れをふまえてコードにして」

<v-click class="mt-6">

💡 **長くなったら**: 「これをゼロベースでAgentに投げるならどうプロンプト書く？」

</v-click>

---

# Agentモードでのコーディング

<div class="grid grid-cols-2 gap-6">

<div>

## ⚙️ 設定・注意点
- **Auto Approve ON** 
- でも「次のプロンプト使いますか？」で止まりやすい
- コマンド実行後に進まない → **同じコマンド再実行**
- **プロンプト間違い** → 止めて打ち直し

</div>

<div>

## 🎯 運用のコツ
- 出力が変な方向 → **止めてコメント指摘**
- 出力中は他作業と並行もアリ
- 複数セッション同時は試したことなし
- **Opusから Sonnetへの切り替えで精度低下**（主観）

</div>

</div>

<div class="mt-8 text-center bg-blue-100 p-4 rounded">
<v-click>

**ポイント**: 無理やり続けずに、おかしいと思ったら止める

</v-click>
</div>

---

# Visual Studioでの確認

<div class="grid grid-cols-2 gap-6">

<div>

## 🔍 確認フロー
1. **差分をざっと見て変更箇所把握**  
   （多すぎるときは詳細追わず）

2. **実行して動作確認**  
   → 想定外なら再びAgentへ

3. **想定通りなら、コードをじっくり見る**

</div>

<div>

## ✋ スタイルの受け入れ

**以前**: 自分のコードスタイルに整形

**今**: ある程度そのまま受け入れる  
明らかに変な部分だけAgentに指摘

</div>

</div>

---

# 最終調整の現実

## 😤 手修正あるある

<div class="space-y-4">

<div class="bg-red-100 p-4 rounded-lg">
<h3 class="text-lg mb-2">改行問題</h3>
<p><strong>改行は高確率でおかしい</strong> → 「クソが」と思いつつ手修正</p>
</div>

<div class="bg-blue-100 p-4 rounded-lg">
<h3 class="text-lg mb-2">対処法</h3>
<p>全体コピペで直ることも → <strong>まずはそれを試す</strong></p>
</div>

<div class="bg-green-100 p-4 rounded-lg">
<h3 class="text-lg mb-2">重要</h3>
<p>コード整ったら<strong>コミット必須</strong></p>
</div>

</div>

---

# AIリファクタとPRレビュー

<div class="grid grid-cols-2 gap-6">

<div>

## 🔄 リファクタ依頼
- 「もっとシンプルにできる？」
- **わりと壊されることも**
- だからコミットは必須
- 気に入らなければロールバック

</div>

<div>

## 👀 PRレビュー活用
1. **自分でざっと確認**
2. **Copilot + Code Rabbit**のAIレビュー
3. **気になった指摘だけ**をAgentに投げる
4. 部分的に修正 → 再コミット
5. PRが通ったらマージ

</div>

</div>

<div class="mt-8 text-center">
<v-click>

**全部は投げない** - 選別が重要

</v-click>
</div>

---
layout: center
class: text-center
---

# まとめ

## 🎯 キーポイント

**Ask** → **Agent** → **Visual Studio** → **AI Review**

<v-click>

### 思考整理 → コード生成 → 確認・実行 → 最終調整

</v-click>

<v-click>

### AIと協業する時代のワークフロー

**完璧を求めず、適度に受け入れる**

</v-click>

<div class="pt-8">
  <span class="text-sm opacity-75">
    あくまで個人的なスタイル - 参考程度に 🙂
  </span>
</div>

---

# Thank you!

<div class="pt-12 text-center">
  <div class="text-2xl mb-4">質問・ディスカッション歓迎</div>
  <div class="text-lg opacity-75">AIとのペアプログラミング体験談</div>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/binnmti" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>
