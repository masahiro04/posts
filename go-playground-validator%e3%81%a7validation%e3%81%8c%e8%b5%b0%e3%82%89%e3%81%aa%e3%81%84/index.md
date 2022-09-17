```metadata
{
    "title": "go-playground/validatorでvalidationが走らない",
    "date": "2022-01-27T23:44:27",
    "categories": "Golang",
}```

structにvalidationを実装していて、なぜかvalidationが発火しない事件が発生しました

```go
 type Body struct {
	value string `validate:"required" ja:"内容"`
}
```

個人的には全く問題ないと思っていたのですが、どうやらGoではlower caseはprivateな扱いとなり、エンコード/デコードされないようです

なので、小文字で変換が発生しないことは正常であり、大文字に直すことで上記は対応することができました🙌

## 参考記事

[【Golang】structのField名で気をつけるところ](https://qiita.com/Yarimizu14/items/e93097c4f4cfd5468259)

[Public、Private【Go】](https://tech-up.hatenablog.com/entry/2018/12/04/000101)
