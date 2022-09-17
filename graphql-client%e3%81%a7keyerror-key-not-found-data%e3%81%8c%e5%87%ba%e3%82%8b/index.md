```metadata
{
    "title": "graphql-clientでKeyError: key not found: &#8220;data&#8221;が出る",
    "date": "2022-01-29T20:08:01",
    "categories": "GraphQL, Ruby on Rails",
}
```

Railsでwordpressからgraphql経由で記事取得しようとしたら、タイトルのエラーが発生しました

該当箇所は以下の部分で、おそらくschemaを読み込んでいると思うのですが、

```ruby
 Schema = GraphQL::Client.load_schema(HTTP)
```

なぜかdataがないです、というエラーが発生してます

queryを投げてdataがない、というのであれば理解できるのですが、query以前の問題だったので、<br>途中で解決は諦めて下記のようにhttpで対応する方針にした結果、すぐ解決できました🙌

```graphql
 query = <<-'GRAPHQL'
      query Posts {
        posts(first: 5) {
          edges {
            node {
              title
              excerpt
              slug
              date
              featuredImage {
                node {
                  sourceUrl
                }
              }
              categories {
                edges {
                  node {
                    name
                  }
                }
              }
            }
          }
        }
      }
    GRAPHQL
    uri = URI('https://hoge/graphql')
    res = Net::HTTP.start(uri.host, uri.port, use_ssl: true) do |http|
      req = Net::HTTP::Post.new(uri)
      req['Content-Type'] = 'application/json'
      # req['Authorization'] = "Bearer #{accessToken}"
      # The body needs to be a JSON string.
      req.body = JSON[{'query' => query}]
      puts(req.body)
      http.request(req)
    end
    puts(res.body)
```

## 参考記事

[`fetch’: key not found: “data” (KeyError) : graphql-client error](https://stackoverflow.com/questions/56540787/fetch-key-not-found-data-keyerror-graphql-client-error)

[KeyError: key not found: “data” #1956](https://github.com/rmosolgo/graphql-ruby/issues/1956)

[GraphQL via HTTP in five ways: cURL, Python, JavaScript, Ruby and PHP](https://www.contentful.com/blog/2021/01/14/GraphQL-via-HTTP-in-five-ways/)
