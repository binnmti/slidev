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

# C#でC#コンパイラを作っている話

## 松井 敏

---

# 自己紹介

- 👨 松井 敏(まつい びん)
- 👜 HACARUS C#&CI/CD メンター(副業)
- 🏆 Microsoft MVP for Developer Technologies 2012-2024
- 📚 Unity5 3Dゲーム開発講座 ユニティちゃんで作る本格アクションゲーム
- 💻 C#読書会主催、Greek Alphabet Software Academy TA
- ❤️ プログラム、マンガ、料理、睡眠、妻&子供

---

# キャリアブレイク

- 松井は現在働いていない
- 少し長めの休みを取って日々勉強をしている
- 勉強している内容
  - アルゴリズム
  - コンピューターサイエンス
  - **コンパイラ作成**
  - 英語
  - 宗教
  - 栄養学
- その中でも今日はコンパイラ作成をピックアップ

---

# 何故、コンパイラを作ろうと思ったか

- 以前は低レイヤープログラミングを重要視していなかった
- Greek Alphabet Software Academyは低レイヤーの内容をかなり重要視している
- アルゴリズムは完全に、Alphaがきっかけではじめた
- それ以外でも少しずつ低レイヤーに興味を持つようになっていた

---

# コンパイラ作成の話を聞いて出来るかもと感じた

- たまたまポッドキャストでコンパイラの話を聞いた
  - [30. セルフホストできるCコンパイラの作り方を夏休みの特別授業で教えた話](https://turingcomplete.fm/30)
- これを聞いたきっかけは自分でもちょっと謎で、、知らない人だし、1話でもないし、その時はコンパイラも興味がなかった
- なんとなく自分でも作れるかもと思ってサイトを見に行った
  - [低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)
- 流し見して、これはキャリアブレイク中の勉強課題にしようとすぐに思った

---

# 実際に読んでみる

- 最初の２ケ月ぐらいはただ1日1時間、1ヶ月15日ぐらいのペースで読んでいた
- 写経はしつつだが、必ずしもコードを把握していたわけではない
- 一応wslでLinux環境も構築して、書かれている事はプログラムにしたりもした
- 再帰下降構文解析の話ぐらいから理解があやふやになった
  - 木構造
  - BNF（Backus–Naur form）と、それを拡張したEBNF（Extended BNF）
  - スタックマシン
- 特にEBNFをCに落としていくところはかなり怪しかった。。

```
expr    = mul ("+" mul | "-" mul)*
mul     = primary ("*" primary | "/" primary)*
primary = num | "(" expr ")"
```

---

# 読むだけでは理解出来ない

- 再帰下降構文解析から先は実際にEBNFをCに変換していくのが基本作業
- ベースの理解が怪しいので、当然どんどん難しくなっていく
- 高校数の授業で躓くのは、実は中学の基礎から躓いていましたみたいな話？
- ステップ12に「ここからの章は正直まだ公開するレベルには達していないと思います。」と書いてある
- 一旦読むのはここまでとした
- 正直読んだだけではコンパイラが作れるレベルでの理解は全く出来てない

---

# Cコンパイラにモチベーションが上がらない

- またCコンパイラを作るという課題にもwsl+Visual Studio Codeの環境にも全くモチベーションが上がってなかった
- そこで、どうせなら楽しい方が良いだろうと思い、当初からC#コンパイラに出来ないかと思っていたので、そちらに舵を切ることにした
- この際、wslも全部止めてC# + Visual Studioで開発するようにしようと思った
- とはいえC#をアセンブリに変換するのに何をすらばよいのか全く分からない

---

# 自分のアセンブラ経験

- 自分はゲームでもアセンブラはほぼほぼ未経験。
- ファーストキャリアからC言語だし、専門学校はBASIC→Cだった。
- ちょうどC++になりだした黎明期。GBAで当時使っているタイトルはほぼなかった記憶。
- なのでアセンブラはほぼ読めない
- C#では確かILというキーワードを聞くなぐらいの把握度

---

# C#コンパイラを作りたい

- 先ずはILの言語仕様を確認しようとした
- ECMA-335に[CLIの仕様](https://www.ecma-international.org/wp-content/uploads/ECMA-335_6th_edition_june_2012.pdf)が書いてあるので読んでみて大きく挫けた
- SharpLabで```Console.WriteLine(42);```だけ書いたのが、[65行のIL](https://sharplab.io/#v2:C4LglgNgNAJiDUAfAAgJgIwFgBQOwDtgBTAJ3wEMIACNKgQRwG8crWb0A2GgFioFlyBABQBKFm2bY209gE4h3VCIDc41gF8cm7EA)になったのもちょっと引いた
- でもChatGPTが簡単な加算プログラムを教えてくれた。これがなかったら多分やってなかった。
- 因みに同様の情報はググっても見つけられず。本当にChatGPT様様。。
- この辺生成系AIってどうなっているんだろう。。内容によってはググってダメな情報はでないことも多いので。。

```
.assembly AddExample {}
.method static void Main() cil managed {
    .entrypoint
    ldc.i4 10         // スタックに10をプッシュ
    ldc.i4 20         // スタックに20をプッシュ
    add               // スタック上の2つの値を加算
    call void [mscorlib]System.Console::WriteLine(int32)
    ret
}
```
- 先ずはこれを出力するところから始めた
---

# テスト

- というわけで数字を書いたらそれがILにする最初の仕組みを作った
- ただし、これだけだとこれが正しくILかをチェックする術がなかった。
- このILを実行できれば30と出力されるはずだが、実行する方法が分からない
- ILをexeにするのってどうしたら良いんだ？？ってなった
- Ilasm.exe (IL アセンブラー)を使えばILが実行ファイルになることが分かった
- 実行したら数字が出るので、これでテストが書けるようになった

---

# 楽しい

- このあたりからめちゃ楽しくなってきた
- 勉強の中でもコンパイラは、かなりモチベーションは低めだったがいきなり一番楽しくなった
- 今まで読んでいただけの理解度がうなぎ上りに上がっていった
- 本当に楽しくてしょうがなくて1時間で止めるのもちょっとずつ伸びていった

---

# 再び再帰下降構文解析

- やっぱり再帰下降構文解析が一番難しかった
- 多分誰がやってもここが最初の山。
- ギターのFコードみたいなもので、きっとココが死屍累々
- ココを超えると結構あとは延長線上に思える


---

# ILは中間

- 再帰下降構文解析が出来てアセンブラを出力して徐々にCコンパイラから差分も出てくる
- この辺でILって実はとても簡単に使えるアセンブラなんじゃないかなと思いだす
- ILは中間アセンブラなので、実際に制限が少なく、汎用的に使いやすくなっている（と思う）
- なので、思っていたよりシンプルに書けることが増えてきた

---

# エラーチェック

- シンプルに書けるとは言え、実装が増えてくると当然つまることもある
- テストが失敗した場合エラーを追うのが少しずつ難しくなった
- 構文解析のエラーは難しくてもVisual Studioでデバッグ出来る
- アセンブラとしては出力出来ているが、それが間違っている時が問題
- Ilasm.exeでエラーは分かるが、Visual Studioのようにエラー箇所を教えてくれない


---

# 3種の神器

- これは３つの道具がとても役に立った。
- 一つは自分で書いたコードをリアルタイムにアセンブラで表示してくれる[sharplab](https://sharplab.io/)
  - C#もILも選べるのはマジで便利だった。
- ChatGPT。コードをアセンブラにした結果を投げて、あっているか聞くと教えてくれる
  - 対話式ビルドエラーみたいな感じで重宝した
- 最後の一つは自分で書いたコードをリアルタイムにアセンブラで表示してくれる仕組み
  - 最初はコマンドラインツールとテストだけだった
  - アセンブラにするところ迄をDLLにして、Blazorからも呼べるようにした
  - 書いたコードがリアルタイムでアセンブラになるのは超便利
  - 人にも見せやすいというメリットもあった
- この文章を書いている時に[peverify](https://learn.microsoft.com/ja-jp/dotnet/framework/tools/peverify-exe-peverify-tool) なるものを知った。

---

# Blazor DEMO

- Github:[ChibiCSharp](https://github.com/binnmti/ChibiCSharp)
- サイト:[BlazorChibiCSharp](https://blazorchibicsharp.azurewebsites.net/)
- 現状出来ること
- 比較演算、変数、if,while,for,関数が実装済み
- 少しだけUI/UXも拘って誰でも扱えるものとしていった

---

# で、今

- ポインタや&記号などもC#は使わない。
- sizeofも仕様が違う。
- 本の内容とも大分ズレが出てきた。
- 型定義がしたくなった
- 型定義は結構大変で、特に型不一致などのエラー対応が必要になってきた
- そもそもmainで返すが出来ない。。
- ちょっと次の方向性を迷い中。。

---

# IL → JIT Asm → exe

- C#がILにはなったけれど、実行ファイルはiLasm.exe任せ。ココの仕組みはもうちょい知りたい。
- 最初はILをJIT Asmにするには？
- 今までの必要最低限ILだとJIT Asm にはならない。
- C#のルールであるclassで囲って、やれば一応出来た。

```
.assembly TestAssembly {}
.module TestModule.dll

.class public auto ansi beforefieldinit Program
       extends [System.Runtime]System.Object
{
    .method public static 
            int32 Main() cil managed 
    {
        .entrypoint
        .maxstack 1
        .locals init (
            [0] int32
        )

        IL_0000: nop
        IL_0001: ldc.i4.1
        IL_0002: stloc.0
        IL_0003: br.s IL_0005

        IL_0005: ldloc.0
        IL_0006: ret
    }
}
```

# IL → JIT Asm ×→ exe
- ただ、JIT Asmからexeには出来ない。
- AOTコンパイルするとC#のコードがネイティブコードになる。
- ILをilasmでexeには出来る or test.asmを書いて、nasmを使ってlink.exeすればexeになる。
- ここが勘違い、そもそもVisual StudioもC#をJIT Asmにはしていない。

---

# IL → exe

- System.Reflectionを使う(これは特にしたい事ではなかった)
```cs
using System;
using System.Reflection;
using System.Reflection.Emit;

class Program
{
    static void Main()
    {
        AssemblyName asmName = new AssemblyName("TestExe");
        AssemblyBuilder asmBuilder = AssemblyBuilder.DefineDynamicAssembly(asmName, AssemblyBuilderAccess.Save);
        ModuleBuilder modBuilder = asmBuilder.DefineDynamicModule("TestExe", "TestExe.exe");
        TypeBuilder typeBuilder = modBuilder.DefineType("Program", TypeAttributes.Public);
        MethodBuilder methodBuilder = typeBuilder.DefineMethod("Main", MethodAttributes.Public | MethodAttributes.Static, typeof(void), Type.EmptyTypes);

        ILGenerator il = methodBuilder.GetILGenerator();
        il.Emit(OpCodes.Ldc_I4_1);
        il.Emit(OpCodes.Call, typeof(Console).GetMethod("WriteLine", new Type[] { typeof(int) }));
        il.Emit(OpCodes.Ret);

        typeBuilder.CreateType();
        asmBuilder.SetEntryPoint(methodBuilder);
        asmBuilder.Save("TestExe.exe");
    }
}
```

---

# IL → exe

- バイナリを出力する(これか。。)
```cs
using System;
using System.IO;

class Program
{
    static void Main()
    {
        byte[] peBytes = new byte[]
        {
            0x4D, 0x5A, 0x90, 0x00, 0x03, 0x00, 0x00, 0x00,  // MZ ヘッダー
            0x04, 0x00, 0x00, 0x00, 0xFF, 0xFF, 0x00, 0x00,
            0xB8, 0x01, 0x00, 0x00, 0x00, 0xC3              // return 1; に相当する x86 アセンブリ
        };

        File.WriteAllBytes("SimpleReturn.exe", peBytes);

        Console.WriteLine("SimpleReturn.exe を作成しました。");
    }
}
```

- 対応表[List of CIL instructions](https://en.wikipedia.org/wiki/List_of_CIL_instructions)は、オペコードからやればできるはず。
- ちっとも食指が動かなかった

---

# 今後の目標

- セルフホスト
  - 既に完璧な環境があるのに？
- 独自言語
  - 既に完璧な言語があるのに？
- デバッグ環境構築
  - これは若干だけ興味なくもない
- ま、他のことに移るかな。。