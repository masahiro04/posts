```metadata
{
    "title": "margin 0 auto とmargin-topを共存させる方法",
    "date": "2022-02-11T15:05:14",
    "categories": "CSS",
}
```

```css
 // だめ
margin-top: 20px;
margin: 0 auto;

// いける
margin: 20px auto 0;

```

## 参考記事

[CSS margin auto center with top margin – VALID?](https://stackoverflow.com/questions/9165369/css-margin-auto-center-with-top-margin-valid)
