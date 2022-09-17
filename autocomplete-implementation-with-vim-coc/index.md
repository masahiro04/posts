```metadata
{
    "title": "vim + cocでaut ocomplete実装",
    "date": "2021-12-23T23:44:45",
    "categories": "Vim",
}
```

こちら参考に実装しました

```vim
 inoremap <silent><expr> <tab> pumvisible() ? coc#_select_confirm() : "\<C-g>u\<TAB>"
inoremap <silent><expr> <cr> "\<c-g>u\<CR>"
```

## 参考記事

[How to remap coc.nvim autocomplete key?](https://stackoverflow.com/questions/67370086/how-to-remap-coc-nvim-autocomplete-key)
