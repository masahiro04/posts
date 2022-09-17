```metadata
{
    "title": "[dein] Vim(lua):E5107: Error loading lua [string &#8220;:lua&#8221;]:3: &#8216;=&#8217; expected near &#8216;nnoremap&#8217;",
    "date": "2022-08-08T16:00:37",
    "categories": "Vim",
}```

NeoVimでLuaを導入したところタイトルのエラーが発生しました

どうやらこのエラーは、luaの前方にスペースやタブが入っていてもエラーが出るようなので、

```vim
 [[plugins]]
repo = 'nvim-telescope/telescope.nvim'
depends = ['plenary.nvim']
hook_add = '''
  lua << EOF
    require('telescope').setup()
  EOF
  nnoremap <leader>ff <cmd>lua require('telescope.builtin').find_files()<cr>
'''
```

こちらに修正したところ解決できました！

```vim
 [[plugins]]
repo = 'nvim-telescope/telescope.nvim'
depends = ['plenary.nvim']
hook_add = '''
lua << EOF
  require('telescope').setup()
EOF
  nnoremap <leader>ff <cmd>lua require('telescope.builtin').find_files()<cr>
'''
```

## 参考記事

[Unable to use lua inside a `.vim` file](https://vi.stackexchange.com/questions/31093/unable-to-use-lua-inside-a-vim-file)
