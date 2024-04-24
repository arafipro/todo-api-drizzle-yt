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


## APIに接続を許可するURLを指定(CORS)

`src/index.ts`のcorsのoriginに接続を許可するURLを指定する  
配列で複数指定することも可能  
接続を許可するURLを指定する場合は最後のスラッシュ（/）は不要  
例）`http://localhost:8000（/は不要）`    

### /todos

idを指定しないURLの場合
GETとPOSTでしか使わない

```ts:src/index.ts
app.use(
  "/todos",
  cors({
    origin: ["{許可するURL1}", "{許可するURL2}"],
    allowHeaders: [
    "X-Custom-Header",
      "Upgrade-Insecure-Requests",
      "Content-Type",
    ],
    allowMethods: ["GET", "POST", "OPTIONS"],
		...
  })
);
```

### /todos/*

idを指定したURLの場合
GETとPUTとDELETEでしか使わない

```ts:src/index.ts
app.use(
  "/todos/*",
  cors({
    origin: ["{許可するURL1}", "{許可するURL2}"],
    allowHeaders: [
      "X-Custom-Header",
      "Upgrade-Insecure-Requests",
      "Content-Type",
    ],
    allowMethods: ["GET", "PUT", "DELETE", "OPTIONS"],
		...
  })
);
```

### 参考資料

Honoドキュメント：
https://hono.dev/middleware/builtin/cors

参考サイト：
https://zenn.dev/camomile_cafe/articles/b11a3f9b8f2f1d
      
## テーブル

### テーブル名 todos

| No. | カラム名 | データ型 | 主キー | 初期値 | 備考         |
| --- | -------- | -------- | :----: | ------ | ------------ |
| 1   | id       | integer  |   ○    |        | ID           |
| 2   | todo     | text     |        |        | TODO の内容  |
| 3   | score    | integer  |   　   | 0      | 点数(達成率) |
| 4   | isDone   | boolean  |   　   | false  | 完了フラグ   |
| 5   | createAt | text     |   　   |        | 登録日時     |
