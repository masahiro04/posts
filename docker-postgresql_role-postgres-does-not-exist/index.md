```metadata
{
    "title": "Docker + postgresqlでrole &#8220;postgres&#8221; does not existエラー",
    "date": "2022-04-13T14:29:03",
    "categories": "Docker, PostgreSQL",
}
```

Dockerのpostgresqlを実行したところタイトルのエラーが発生しました

こちらが一番簡単に解決するための手段

```docker
 version: "3.8"
services:
  db:
    image: postgres:12-alpine
    ports:
      - '5432:5432'
    volumes:
      - postgres:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: hoge_development # ここ！！！！！
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_INITDB_ARGS: "--encoding=UTF-8"
      TZ: "Asia/Tokyo"
      POSTGRES_HOST_AUTH_METHOD: trust

```

どうやら2015/7/8以降は、

- User
- Database

を作成するだけなら、POSTGRES_DBという環境変数を入れるだけで良くなったらしいので、上記で対応しました🙌

<hr class="wp-block-separator">

[But since July 8th, 2015, if all you need is to create a user and database, it is easier to just make use to the POSTGRES_USER, POSTGRES_PASSWORD and POSTGRES_DB environment variables:https://stackoverflow.com/questions/26598738/how-to-create-user-database-in-script-for-docker-postgres](https://stackoverflow.com/questions/26598738/how-to-create-user-database-in-script-for-docker-postgres)

ちなみにですが、こちらを適用するために

- Containers
- Images
- Volume

を該当箇所全て削除し、dockerのcacheもclearしてから実行しました

削除せずにおこなったところ動かない状態が続いていたので、ご注意ください

## 参考記事

[How to create User/Database in script for Docker Postgres](https://stackoverflow.com/questions/26598738/how-to-create-user-database-in-script-for-docker-postgres)
