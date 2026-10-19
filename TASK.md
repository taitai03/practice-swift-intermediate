# テストを書く

## 背景

CSチームに「アプリのカートに表示されるポイントと、実際に付与されたポイントが1ポイント合わないことがある」という問い合わせが何件か来ています。ポイント計算（`PointCalculator`）にはテストがありません。**テストを書いて仕様どおりかを確かめ、見つかった不具合を直してください。**

## ポイント計算の仕様

`PointCalculator.points(taxIncluded:rank:)`

- `taxIncluded` は、割引と消費税を反映した金額（送料を含まない）
- 一般（`.regular`）・シルバー（`.silver`）会員は1%、ゴールド（`.gold`）会員は2%
- 端数は **切り捨て**
- 金額が0以下のときは0

## やること

1. `SakuraMartKit/Tests/SakuraMartKitTests/PointCalculatorTests.swift` を作り、`PointCalculatorTests` という名前の Suite（struct）に、上の仕様を確かめるテストを書く（正常系・境界値・異常系）
2. テストで見つかった不具合を `SakuraMartKit/Sources/SakuraMartKit/PointCalculator.swift` で直す

## 受け入れ条件

- [ ] `PointCalculatorTests.swift` に、仕様を確かめるテストがある
- [ ] あなたのテストが、こちらで用意した **バグ入りのポイント計算（5種類）をすべて見つけられる**（PRのチェックで、バグを入れた実装にあなたのテストを当てて、失敗することを確かめます。`--filter PointCalculatorTests` で実行するので、Suite の名前を変えないこと）
- [ ] ポイント計算の不具合が直っている
- [ ] 既存のテストがすべて通る

## 参考

- Swift Testing: https://developer.apple.com/documentation/testing
- 手元で「テストのテスト」をするには `bash .task/check.sh` を実行します

---

課題で用意したテストと `.github/` は変更しないでください。変更したPRは判定の対象外になります。
