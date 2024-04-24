# 【Cloudflare Workers】Hono ＋ Cloudflare D1にDrizzle ORMを追加して30分でREST APIを作成

## YouTube

[!["【Cloudflare Workers】Hono ＋ Cloudflare D1にDrizzle ORMを追加して30分でREST APIを作成"](https://i.ytimg.com/vi/kCwHEXqoPsE/maxresdefault.jpg)](https://youtu.be/kCwHEXqoPsE)

## 技術選定

- TypeScript
- Hono
- Drizzle
- Cloudflare D1
- Cloudflare Workers

## 初期設定

### NodeModule をインストール

```bash
npm install
```

### データベースを作成

```bash
npx wrangler d1 create todo-api-drizzle
```

### wrangler.toml に追記

```toml
[[d1_databases]]
binding = "DB"
database_name = "todo-api-drizzle"
database_id = "<unique-ID-for-your-database>"
```

`<unique-ID-for-your-database>`はデータベースを作成したときに出力されるID

### テーブルのスキーマを作成

```bash
npm run generate
```

### ローカルデータベースにテーブルを作成

```bash
npm run migrate:local
```

### リモートデータベースにテーブルを作成

```bash
npm run migrate:remote
```

### ローカルデータベースのテーブルにダミーデータを登録

```bash
npm run migrate:local
```

### リモートデータベースのテーブルにダミーデータを登録

```bash
npm run migrate:remote
```

## テーブル

### テーブル名 todos

| No. | カラム名 | データ型 | 主キー | 初期値 | 備考         |
| --- | -------- | -------- | :----: | ------ | ------------ |
| 1   | id       | integer  |   ○    |        | ID           |
| 2   | todo     | text     |        |        | TODO の内容  |
| 3   | score    | integer  |   　   | 0      | 点数(達成率) |
| 4   | isDone   | boolean  |   　   | false  | 完了フラグ   |
| 5   | createAt | text     |   　   |        | 登録日時     |
