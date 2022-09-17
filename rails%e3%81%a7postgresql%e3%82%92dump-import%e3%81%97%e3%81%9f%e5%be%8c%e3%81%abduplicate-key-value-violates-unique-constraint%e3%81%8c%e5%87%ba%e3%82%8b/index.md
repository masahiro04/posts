```metadata
{
    "title": "RailsでPostgreSQLをdump importした後にduplicate key value violates unique constraintが出る",
    "date": "2022-05-11T21:20:48",
    "categories": "PostgreSQL, Ruby on Rails",
}
```

RailsのプロジェクトでStagingのDBをdevelopmentに入れたところタイトルのエラーが発生しました

## 解決策

こちらが解決するためのコード

```ruby
 $ rails c

ActiveRecord::Base.connection.tables.each do |table_name| 
  ActiveRecord::Base.connection.reset_pk_sequence!(table_name)
end
```

上記実行することで、特定のtableだけではなく全てのtableに対してprimary keyリセットを実行することが可能になります

## そもそもの問題点

エラー文に書いてある通り、key valueがダブっています、が問題です

```ruby
 duplicate key value
```

PostgreSQLでは連番となる数値はシーケンスと呼ばれる、データベースのオブジェクトで、transactionやcommit/rollback関係なくインクリメントされます

通常は勝手にインクリメントされるわけですが、db dump等でimportを行うと、<strong>新しいデータベース</strong>にschemaをコピーし、シーケンスを1から始めるようです

※importする際は空っぽの新しいデータベースに入れることになるので、sequenceは1から始まる

[The Sequence ‘issue’ that in the new database, is probably because the GUI tool used to copy Data over, is simply copying the Schema over to the new Database (which resets the Sequence state). This causes the new Database Sequence to start counting from 1 (instead of what it currently is, in the Old database)https://stackoverflow.com/questions/32043431/sequence-issue-while-importing-db](https://stackoverflow.com/questions/32043431/sequence-issue-while-importing-db)

## 参考記事

[why PG::UniqueViolation: ERROR: duplicate key value violates unique constraint?](https://stackoverflow.com/questions/47577532/why-pguniqueviolation-error-duplicate-key-value-violates-unique-constraint)

[新人にPostgreSQLのシーケンスを理解してもらう](https://qiita.com/5zm/items/da82cec73e097d2a97d0)
