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

# テスト、CI/CD、レビュー

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

# アジェンダ

## TDD ハンズオン(2時間)
- TDDサイクル実践（Red-Green-Refactor）
- Pythonの基本的なフレームワークでの自動販売機のテスト

## CI/CDの講義(1時間)
- CI/CDの重要性、自分の実践など

## GitHub Actions ハンズオン (1時間)
- CodeRabbitによる自動レビュー
- PRに対してのアクション（時間がかからない、ビルドチェックやユニットテスト）
- マージに対してのアクション（デプロイや受け入れテスト）
- 1日1回のタイマーアクション（さらに時間がかかる、GUIテストやバッチ処理）

## テストやレビュー 講義 (1時間)
- 何故テストを書くべきか
- テストを書きやすく作るのに必要なのは設計。MVCの三層分離。開発手法。DIなどからめて。
- 何故コードレビューすべきか
- コード健康の保ち方

---

# TDDとは？

- **Test-Driven Development**（テスト駆動開発）
- テストを先に書いてから実装する開発手法
- 1. テストを書く（失敗する）
- 2. テストが通るコードを書く（最小限）
- 3. リファクタリングする
- **メリット**：高品質なコード、設計の向上、ドキュメント代わりになる

---

# Red-Green-Refactorサイクル

<div grid="~ cols-3 gap-4">
<div>
  <h3 style="color: red">Red</h3>
  <ul>
    <li>失敗するテストを書く</li>
    <li>何を実装するか明確に</li>
    <li>仕様を理解する</li>
  </ul>
</div>
<div>
  <h3 style="color: green">Green</h3>
  <ul>
    <li>テストが通る最小限の実装</li>
    <li>とりあえず動けばOK</li>
    <li>ハードコーディングもあり</li>
  </ul>
</div>
<div>
  <h3 style="color: blue">Refactor</h3>
  <ul>
    <li>コードを整理する</li>
    <li>重複を排除</li>
    <li>可読性向上</li>
  </ul>
</div>
</div>

---

# 自動販売機システムの要件

- お金を投入できる (100円, 500円, 1000円)
- 商品を選択できる (お茶:100円, コーヒー:150円, 水:80円)
- お釣りが返ってくる
- 投入金額が足りない場合は購入できない
- 在庫がない場合は購入できない

---

# pytestの基本

```python
# test_vending_machine.py
def test_insert_money():
    vm = VendingMachine()
    vm.insert(100)
    assert vm.balance == 100

def test_buy_tea():
    vm = VendingMachine()
    vm.insert(100)
    change, item = vm.buy("tea")
    assert change == 0
    assert item == "tea"
```

実行: `pytest test_vending_machine.py -v`

---

# ハンズオン: 最初のテスト（Red）

```python
# test_vending_machine.py
import pytest

def test_new_vending_machine_has_zero_balance():
    vm = VendingMachine()
    assert vm.balance == 0
```

このテストは失敗します！（VendingMachineクラスがないため）

---

# 実装してみよう（Green）

```python
# vending_machine.py
class VendingMachine:
    def __init__(self):
        self.balance = 0
```

これでテストが通ります。

---

# 次のテスト: お金の投入（Red）

```python
def test_insert_money_increases_balance():
    vm = VendingMachine()
    vm.insert(100)
    assert vm.balance == 100
    
    vm.insert(500)
    assert vm.balance == 600
```

---

# お金の投入機能の実装（Green）

```python
class VendingMachine:
    def __init__(self):
        self.balance = 0
        
    def insert(self, amount):
        self.balance += amount
```

---

# リファクタリング: 有効な金額のみ受け付ける（Refactor）

```python
class VendingMachine:
    def __init__(self):
        self.balance = 0
        self.valid_coins = [10, 50, 100, 500, 1000]
        
    def insert(self, amount):
        if amount in self.valid_coins:
            self.balance += amount
            return True
        return False
```

テストも更新:

```python
def test_insert_valid_money():
    vm = VendingMachine()
    assert vm.insert(100) == True
    assert vm.balance == 100
    
def test_insert_invalid_money():
    vm = VendingMachine()
    assert vm.insert(25) == False
    assert vm.balance == 0
```

---

# 商品購入のテスト（Red）

```python
def test_buy_product():
    vm = VendingMachine()
    vm.insert(100)
    change, product = vm.buy("tea")
    assert change == 0
    assert product == "tea"
```

---

# 商品購入の実装（Green）

```python
class VendingMachine:
    def __init__(self):
        self.balance = 0
        self.valid_coins = [10, 50, 100, 500, 1000]
        self.products = {
            "tea": {"price": 100, "stock": 5},
            "coffee": {"price": 150, "stock": 5},
            "water": {"price": 80, "stock": 5}
        }
        
    def insert(self, amount):
        if amount in self.valid_coins:
            self.balance += amount
            return True
        return False
        
    def buy(self, product_name):
        if product_name not in self.products:
            return self.balance, None
            
        product = self.products[product_name]
        if product["stock"] <= 0:
            return self.balance, None
            
        if self.balance < product["price"]:
            return self.balance, None
            
        change = self.balance - product["price"]
        self.balance = 0
        product["stock"] -= 1
        return change, product_name
```

---

# 在庫切れのテスト（Red）

```python
def test_buy_out_of_stock():
    vm = VendingMachine()
    vm.products["tea"]["stock"] = 0
    vm.insert(100)
    change, product = vm.buy("tea")
    assert change == 100  # お金は返ってくる
    assert product == None  # 商品はない
```

---

# まとめ: TDDの流れ

1. **明確な要件の理解**
2. **小さな単位で実装**
   - テスト→実装→リファクタリング
3. **常にテストが通る状態を保つ**
4. **設計が自然に改善される**   

---
