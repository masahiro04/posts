```metadata
{
    "title": "Next.jsのrouterでundefinedが出る問題",
    "date": "2022-01-30T15:23:13",
    "categories": "Next.js, React",
}
```

Next.jsでrouter経由でparamsを取得しようとした際に、undefinedが出る問題が発生しましたが、下記のようにreadyしたタイミングに変更したところ、問題なく動きました

```typescript
 const router = useRouter();
useEffect(()=>{
    if(!router.isReady) return;
}, [router.isReady]);
```

## 参考記事

[useRouter/withRouter receive undefined on query in first render](https://stackoverflow.com/questions/61040790/userouter-withrouter-receive-undefined-on-query-in-first-render)
