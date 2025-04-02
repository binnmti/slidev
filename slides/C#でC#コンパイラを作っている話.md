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
- 👨‍💻 元ゲームプログラマ & 元Windowsソフトウェアプログラマ
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

- 最初の２ケ月ぐらいは1日1時間、1ヶ月15日ぐらいのペースでただ読んでいた
- 当たり前だけど、読んだだけでは把握出来ないことも沢山ある
- 一応wslでLinux環境も構築して、書かれている事はプログラムにしたりもした。写経。
- 特に再帰下降構文解析の話ぐらいから理解があやふやになってきた
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

# 理解出来ないことを積んでもますます理解出来ない悪循環

- 再帰下降構文解析から先は実際にEBNFをCに変換していくのが基本作業
- ベースの理解が怪しいので、当然どんどん難しくなっていく
- 高校の数学や英語の授業で躓くのは、実は中学の基礎から躓いていましたみたいな話？
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

- 自分はゲーム開発でもアセンブラはほぼほぼ未経験。
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
- 先ずはこれを出力するところから始めた


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

---

# 生成系AIってどうなっているのかな？

- 因みに同様の情報はググっても見つけられず。本当にChatGPT様様。。
- この辺生成系AIってどうなっているんだろう。。
- 内容によってはググっても全然情報が出ない時は、ChatGPTも全然見当違いのことを言うことがあるので。。

---

# テスト

- というわけで数字を書いたらそれがILにする最初の仕組みを作った
- ただし、これだけだとこれが正しくILかをチェックする術がなかった。
- つまりILをビルドしてコンパイルエラー？する仕組みってないの？
- あとILをexeにするのってどうしたら良いんだ？？ってなった
- Ilasm.exe (IL アセンブラー)を使えばILが実行ファイルになることが分かった
- 実行したら数字が出るので、これでテストが書けるようになった
- 自分で書いたC#（のような）プログラム("42")→自分で書いたIL→実行した結果("42")
```
.assembly AddExample { }
.method static int32 main() cil managed {
    .entrypoint
    ldc.i4 42
    ret
}
```
- というわけでC#でC#コンパイラを作ろうとした

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
- 現状出来ること:比較演算、変数、if,while,for,関数が実装済み
- 少しだけUI/UXも拘って誰でも扱えるものとしていった

---

# で、現状

- ポインタや&記号などもC#は使わない。
- sizeofも仕様が違う。
- 本の内容とも大分ズレが出てきた。
- せめて型定義がしたくなった
- と思ったら型定義は結構大変で、特に型不一致などのエラー対応が必要になってきた
- そもそもmainでstringを返すことが出来ない。。
- ちょっと次の方向性を迷い中。。

---

# で、今後

- IL→exe自作
  - ま、ILasm.exeがあるし。。
- セルフホスト
  - 既に完璧な環境があるのに？
- 独自言語
  - 既に完璧な言語があるのに？
- デバッグ環境構築
  - これは若干だけ興味なくもない
- ま、他のことやるかな。。

---

# まとめ

- コンパイラ作りは最初は簡単だが、徐々に理解が難しくなった
- 再帰下降構文解析がハードル
- 自分にあった環境でやらないとモチベーションは保てない
- 実際に作りだしたらめちゃ楽しくなった
- 車輪の再発明も悪くない
- プログラムの理解度はワンステップ上がった感じ
- 色々発展していく方向性はありそう

---

# オマケ

---

# IL → JIT Asm → exe

- C#がILにはなったけれど、実行ファイルはiLasm.exe任せ。ココの仕組みはもうちょい知りたい。
- 最初はILをJIT Asmにするには？
- 今までの必要最低限ILだとJIT Asm にはならない。
- C#のルールであるclassで囲って、やれば一応出来た。
- ただ、JIT Asmからexeには出来ないと気づいた。
- ILをilasmでexeには出来る(IL→exe)
- そもそもVisual StudioもC#をJIT Asmにはしていないじゃんと。(C#→(IL)→exe)
- test.asmを書いて、nasmを使ってlink.exeすればexeになるらしいが。。。(ASM→exe)

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

- バイナリを出力する(これかな。。)
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

---

- 対応表[List of CIL instructions](https://en.wikipedia.org/wiki/List_of_CIL_instructions)は、オペコードからやればできるはず。

| Opcode | Instruction | Description | Type of instruction |
|--------|-------------|-------------|---------------------|
| 0x58 | add | Add two values, returning a new value. | Base instruction |
| 0x38 | br <int32 (target)> | Branch to target. | Base instruction |
| 0x2B | br.s <int8 (target)> | Branch to target, short form. | Base instruction |
| 0x20 | ldc.i4 <int32 (num)> | Push num of type int32 onto the stack as int32. | Base instruction |
| 0x16 | ldc.i4.0 | Push 0 onto the stack as int32. | Base instruction |

- でも、実行してみると、、、

```
---------------------------
サポートされていない 16 ビット アプリケーション
---------------------------
64 ビット バージョンの Windows での非互換性のため、プログラムまたは機能である "\??\C:\Users\BinMa\source\repos\test\ConsoleApp1\ConsoleApp1\bin\Debug\net9.0\SimpleReturn.exe" を開始または実行できません。ソフトウェア製造元に問い合わせて 64 ビット Windows 互換バージョンが利用可能であるかどうか確認してください。

```

---

- 16 ビット アプリケーションではなくなったが、実行出来ない。
- このアプリはお使いのPCでは起動できません。

```
using var fs = new FileStream("SimpleReturn.exe", FileMode.Create, FileAccess.Write);
using var bw = new BinaryWriter(fs);

// DOSヘッダー
byte[] dosHeader = {
            0x4D, 0x5A, 0x90, 0x00, 0x03, 0x00, 0x00, 0x00, 0x04, 0x00, 0x00, 0x00, 0xFF, 0xFF, 0x00, 0x00,
            0xB8, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x40, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
            0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
            0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0x00, 0x00, 0x00
        };
bw.Write(dosHeader);

// DOSスタブ メッセージ
byte[] dosStub = {
            0x0E, 0x1F, 0xBA, 0x0E, 0x00, 0xB4, 0x09, 0xCD, 0x21, 0xB8, 0x01, 0x4C, 0xCD, 0x21, 0x54, 0x68,
            0x69, 0x73, 0x20, 0x70, 0x72, 0x6F, 0x67, 0x72, 0x61, 0x6D, 0x20, 0x63, 0x61, 0x6E, 0x6E, 0x6F,
            0x74, 0x20, 0x62, 0x65, 0x20, 0x72, 0x75, 0x6E, 0x20, 0x69, 0x6E, 0x20, 0x44, 0x4F, 0x53, 0x20,
            0x6D, 0x6F, 0x64, 0x65, 0x2E, 0x0D, 0x0D, 0x0A, 0x24, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
        };
bw.Write(dosStub);
bw.Write(new byte[16]); // パディング

// PEシグネチャ (DOSヘッダーから0x80バイト目)
bw.Seek(0x80, SeekOrigin.Begin);
bw.Write(Encoding.ASCII.GetBytes("PE\0\0"));

// COFF File Header
uint timestamp = (uint)(DateTimeOffset.UtcNow.ToUnixTimeSeconds() & 0xFFFFFFFF);

bw.Write((ushort)0x8664);  // Machine: x64
bw.Write((ushort)1);       // NumberOfSections
bw.Write(timestamp);       // TimeDateStamp (現在時刻)
bw.Write((uint)0);         // PointerToSymbolTable
bw.Write((uint)0);         // NumberOfSymbols
bw.Write((ushort)0xF0);    // SizeOfOptionalHeader
bw.Write((ushort)0x22);    // Characteristics: 実行可能

// Optional Header - Standard Fields
bw.Write((ushort)0x20B);   // Magic: PE32+ (64ビット)
bw.Write((byte)14);        // MajorLinkerVersion
bw.Write((byte)31);        // MinorLinkerVersion
bw.Write((uint)0x200);     // SizeOfCode
bw.Write((uint)0x400);     // SizeOfInitializedData
bw.Write((uint)0);         // SizeOfUninitializedData
bw.Write((uint)0x1000);    // AddressOfEntryPoint
bw.Write((uint)0x1000);    // BaseOfCode

// Optional Header - Windows-Specific Fields
bw.Write((ulong)0x140000000); // ImageBase
bw.Write((uint)0x1000);    // SectionAlignment
bw.Write((uint)0x200);     // FileAlignment
bw.Write((ushort)10);      // MajorOperatingSystemVersion (Windows 10)
bw.Write((ushort)0);       // MinorOperatingSystemVersion
bw.Write((ushort)0);       // MajorImageVersion
bw.Write((ushort)0);       // MinorImageVersion
bw.Write((ushort)10);      // MajorSubsystemVersion (Windows 10)
bw.Write((ushort)0);       // MinorSubsystemVersion
bw.Write((uint)0);         // Win32VersionValue
bw.Write((uint)0x3000);    // SizeOfImage
bw.Write((uint)0x400);     // SizeOfHeaders
bw.Write((uint)0);         // CheckSum
bw.Write((ushort)3);       // Subsystem: コンソール
bw.Write((ushort)0x8140);  // DllCharacteristics: NX互換, ハイエントロピーVA, ターミナルサーバ対応
bw.Write((ulong)0x100000); // SizeOfStackReserve
bw.Write((ulong)0x1000);   // SizeOfStackCommit
bw.Write((ulong)0x100000); // SizeOfHeapReserve
bw.Write((ulong)0x1000);   // SizeOfHeapCommit
bw.Write((uint)0);         // LoaderFlags
bw.Write((uint)16);        // NumberOfRvaAndSizes

// データディレクトリ (16個)
for (int i = 0; i < 16; i++)
{
    bw.Write((uint)0); // VirtualAddress
    bw.Write((uint)0); // Size
}

// セクションテーブル
// .text セクション
bw.Write(Encoding.ASCII.GetBytes(".text\0\0\0")); // Name
bw.Write((uint)0x10);      // VirtualSize
bw.Write((uint)0x1000);    // VirtualAddress
bw.Write((uint)0x200);     // SizeOfRawData
bw.Write((uint)0x400);     // PointerToRawData
bw.Write((uint)0);         // PointerToRelocations
bw.Write((uint)0);         // PointerToLinenumbers
bw.Write((ushort)0);       // NumberOfRelocations
bw.Write((ushort)0);       // NumberOfLinenumbers
bw.Write((uint)0x60000020); // Characteristics: 実行可能コード

// .text セクションのコード
bw.Seek(0x400, SeekOrigin.Begin);

// 単純なハローワールドプログラム相当 (コンソールウィンドウを一瞬表示）
// 64ビットWindows calling convention: 
// RCX, RDX, R8, R9が最初の4つの引数
// RAXは戻り値

// mov rcx, 0 (ExitCode = 0)
// call ExitProcess
byte[] code = {
            0x48, 0xC7, 0xC1, 0x00, 0x00, 0x00, 0x00,  // mov rcx, 0
            0x48, 0x31, 0xC0,                          // xor rax, rax
            0x48, 0xFF, 0xC0,                          // inc rax
            0xC3                                        // ret
        };

bw.Write(code);

// ファイルサイズを調整
bw.Seek(0x600, SeekOrigin.Begin);

```

