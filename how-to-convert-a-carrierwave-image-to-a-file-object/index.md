```metadata
{
    "title": "Carrierwaveの画像をFile Objectに変換する方法",
    "date": "2022-04-26T20:11:10",
    "categories": "Ruby on Rails",
}```

Carrierwaveに登録した画像を別のモデルに移行するために変換試みたのですが、意外に時間を使ってしまったので次回のために記事にしました

こちら結論

```ruby
 File.binwrite(Rails.root.join("public/#{file.filename}"), file.read)
p.image = File.open(Rails.root.join("public/#{file.filename}"))
```

1行で一発変換を探していたのですが、無理そうだったのでまずは登録している画像をfileとして保存してます

その際にpathだと正確に画像を取得できない場合もあった(local/prodの違い等)ので、Carrierwaveで保存したファイルをバイナリ経由でFile Objectとして作成、その後にファイルをpublic配下に作成してます

```ruby
 File.binwrite(Rails.root.join("public/#{file.filename}"), file.read)
```

ファイル作成後に、ファイルのpathからFileをnewして新たにCarrierwave経由で新しい場所(任意のカラム等)に保存するようにしてみました

```ruby
 p.image = File.open(Rails.root.join("public/#{filename}"))
```

この作成方法だと該当するレコード分だけ画像が作成されてしまうので、loopの最後に画像を削除することが必要なのでその点注意が必要です🙌

また、pathもベタガキなので長期的なことを考えると下記のようにまとめるとより運用楽になるかと

```ruby
 # 共通のpathを設定
path = Rails.root.join("public/#{file.filename}")
# バイナリからファイルを生成、保存
File.binwrite(path, file.read)
# pというインスタンスのimageカラムにFile Object追加
p.image = File.open(path)
# 保存完了後にファイルを削除
File.delete(path)
```

##  参考記事

自己解決
