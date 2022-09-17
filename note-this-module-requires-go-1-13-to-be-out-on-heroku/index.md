```metadata
{
    "title": "note: module requires Go 1.13がHerokuで出る",
    "date": "2022-03-26T21:16:51",
    "categories": "Golang, Heroku",
}```

go.modにversionを指定しているのですが、なぜかタイトルのエラーが発生しました

```go
  !!    The go.mod file for this project does not specify a Go version
 !!    
 !!    Defaulting to go1.12.17
 !!    
 !!    For more details see: https://devcenter.heroku.com/articles/go-apps-with-modules#build-configuration

golang.org/x/text/secure/precis
# github.com/jackc/pgtype
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/aclitem_array.go:89:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/bool_array.go:92:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/bpchar_array.go:92:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/bytea_array.go:73:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/cidr_array.go:112:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/date_array.go:93:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/enum_array.go:89:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/float4_array.go:92:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/float8_array.go:92:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/hstore_array.go:73:49: reflectedValue.IsZero undefined (type reflect.Value has no field or method IsZero)
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgtype@v1.10.0/hstore_array.go:73:49: too many errors
note: module requires Go 1.13
github.com/jackc/pgx/v4/internal/sanitize
github.com/jackc/pgconn
# github.com/jackc/pgconn
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgconn@v1.11.0/errors.go:25:9: undefined: errors.As
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgconn@v1.11.0/pgconn.go:236:6: undefined: errors.As
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgconn@v1.11.0/pgconn.go:485:6: undefined: errors.As
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgconn@v1.11.0/pgconn.go:495:15: undefined: errors.As
../codon/tmp/cache/go-path/pkg/mod/github.com/jackc/pgconn@v1.11.0/pgconn.go:513:15: undefined: errors.As
 !     Push rejected, failed to compile Go app.
 !     Push failed
```

Goのバージョンは下記のように固定しているのですが、なぜかHerokuではタイトルのように、1.12.17が適用されてしまいます

```go
 module go-api

go 1.18
```

HerokuでGoのバージョンを指定するためには、コメントで下記のように追加する、もしくはHerokuのdashboardの変数管理の場所で、追加しても動くそうです(私は未確認)

```go
 // +heroku goVersion go1.18
go 1.18
```

![](./heroku_go-644x42.png)

## 参考記事

[go.mod に書かれた Go のバージョンが Heroku に認識されない](https://www.utakata.work/entry/herroku/go-gomod-version)

[Defaults to v1.11.x version, despite “go 1.12” in go.mod file #301](https://github.com/heroku/heroku-buildpack-go/issues/301)
