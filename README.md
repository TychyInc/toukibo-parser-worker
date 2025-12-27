# toukibo-parser-worker

[商業登記簿パーサー](https://github.com/tychy/toukibo-parser)を組み込んだCloudflare Workers API

商業登記簿PDFをPOSTすると、法人情報をJSON形式で返します。

[デモページ](https://toukibo-parser-demo.tychy.jp/?index)で試すことができます。

## 技術スタック

- Go 1.23 + WebAssembly
- Cloudflare Workers
- [syumai/workers](https://github.com/syumai/workers) - Go Wasm Workers ランタイム

## API

### POST /parse

商業登記簿PDFを解析し、法人情報を返します。

**リクエスト**

```bash
curl -X POST \
  -H "Content-Type: application/pdf" \
  --data-binary "@登記簿.pdf" \
  https://go-worker.a2sin2a2ko1115.workers.dev/parse
```

**レスポンス例**

```json
{
  "登記簿作成時刻": "2024-01-01T00:00:00Z",
  "法人名": "株式会社サンプル",
  "法人格": "株式会社",
  "住所": "東京都千代田区...",
  "資本金": 10000000,
  "発行済み株式数": 100,
  "各種の株式の数": [],
  "役員": [
    { "氏名": "山田太郎", "役職": "代表取締役" }
  ],
  "役員氏名": ["山田太郎"],
  "代表者氏名": ["山田太郎"],
  "破産日": "",
  "解散日": "",
  "会社継続日": ""
}
```

### GET /hello

ヘルスチェック用エンドポイント。`Hello!` を返します。

## 開発

### 必要環境

- Go 1.23以上
- Node.js（wranglerのため）
- [wrangler CLI](https://developers.cloudflare.com/workers/wrangler/)

### コマンド

```bash
make dev      # 開発サーバー起動 (localhost:8787)
make build    # Go Wasm バイナリをビルド
make deploy   # Cloudflare Workers にデプロイ
```

### ローカルでのAPI呼び出し

```bash
curl -X POST \
  -H "Content-Type: application/pdf" \
  --data-binary "@sample.pdf" \
  http://localhost:8787/parse
```

## デプロイ

mainブランチへのpush時にGitHub Actionsで自動デプロイされます。

手動デプロイする場合:

```bash
make deploy
```

## ライセンス

[toukibo-parser](https://github.com/tychy/toukibo-parser)のライセンスに従います。
