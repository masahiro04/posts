```metadata
{
    "title": "Active Adminで同じModelを複数使う方法",
    "date": "2021-12-08T14:38:08",
    "categories": "Ruby on Rails",
}```

User.roleに応じて実装する方法を探していたところ、継承で実装できそうなので試したところうまくいきました

```ruby
 # models/partner.rbで作成
class Partner < User
end

ActiveAdmin.register Partner do
end
```

この方法使えば、いくらでも分けて実装することが可能となります🙏

## 参考記事

[Two pages for the same resource – ActiveAdmin](https://stackoverflow.com/questions/16546502/two-pages-for-the-same-resource-activeadmin)
