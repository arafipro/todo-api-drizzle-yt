# 【Cloudflare Workers】REST APIを作成しながらDrizzle ORMの使い方を学ぶ

## YouTube

[!["【Cloudflare Workers】REST APIを作成しながらDrizzle ORMの使い方を学ぶ"](https://i.ytimg.com/vi/kCwHEXqoPsE/maxresdefault.jpg)](https://youtu.be/kCwHEXqoPsE)

## 技術選定

- TypeScript
- Hono
- Drizzle
- Cloudflare D1
- Cloudflare Workers

## 初期設定

### NodeModuleをインストール

```bash
npm install
```

### データベースを作成

```bash
npx wrangler d1 create todo-api-drizzle
```

### wrangler.tomlに追記

```toml
[[d1_databases]]
binding = "DB"
database_name = "todo-api-drizzle"
database_id = "<unique-ID-for-your-database>"
```

`<unique-ID-for-your-database>`はデータベースを作成したときに出力されるID

### データベースにテーブルを作成

```bash
npx wrangler d1 execute todo-api-drizzle --remote --file=./db/schema.sql
```
