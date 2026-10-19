# APIとJSON

## 背景

アプリはいま、アプリの中のモック（`MockProductAPI`）の商品を表示しています。Next.js版の会員向けAPI（`GET /api/products`）につないで、本物の商品を表示できるようにします。サーバーが落ちているときや圏外のときも、理由に合ったメッセージを出せるようにしたい、と言われています。

## やること

`SakuraMartKit/Sources/SakuraMartKit/HTTPProductAPI.swift` を作り、`ProductAPI` に準拠した `HTTPProductAPI` を実装する。通信には `HTTPClient`（`HTTPClient.swift`。本物は `URLSessionHTTPClient`）を使う。

### `HTTPProductAPI`

- `init(baseURL: URL, client: HTTPClient = URLSessionHTTPClient())`
- `fetchProducts()`: `GET {baseURL}/api/products`

  ```json
  { "products": [ { "id": 1, "name": "国産こしひかり 5kg", "description": "…", "price": 2980, "category": "食品" } ] }
  ```

- `fetchProduct(id:)`: `GET {baseURL}/api/products/{id}`

  ```json
  { "product": { "id": 1, "name": "国産こしひかり 5kg", "description": "…", "price": 2980, "category": "食品" } }
  ```

### エラー（`APIError`）

| 状況 | 投げるエラー |
| --- | --- |
| 通信できない（`client.get` が例外を投げた） | `.network` |
| ステータスが 404 | `.notFound` |
| そのほかの 200番台以外 | `.server(status: ステータスコード)` |
| 200番台だが、JSON の形が違う | `.decoding` |

### アプリの設定

`SakuraMart/AppConfig.swift` で、Xcode の Scheme の環境変数 `SAKURA_API_BASE_URL`（例: `http://localhost:3000`）があれば `HTTPProductAPI` を、なければいままでどおり `MockProductAPI` を使うようにする（画面はAIレビューで確認するので、自動のテストはありません）。

## 受け入れ条件

- [ ] 商品一覧と商品の詳細を、APIの JSON から取得できる
- [ ] 失敗の理由ごとに、上の表のエラーを投げる
- [ ] `HTTPProductAPI` は `HTTPClient` を外から受け取り、テストでスタブに差し替えられる
- [ ] 既存のテストがすべて通る

## 参考

- テスト: `SakuraMartKit/Tests/SakuraMartKitTests/Tasks/HTTPProductAPITaskTests.swift`
- Next.js版のさくらマート（JavaScript / TypeScript 中級の課題リポジトリ）を手元で動かすと、本物のAPIで試せます
- Apple: Encoding and Decoding Custom Types https://developer.apple.com/documentation/foundation/encoding-and-decoding-custom-types

---

課題で用意したテストと `.github/` は変更しないでください。変更したPRは判定の対象外になります。
