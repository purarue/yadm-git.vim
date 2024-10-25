## yadm-git.vim

Integrates [`yadm`](https://github.com/TheLocehiliosan/yadm) with [`vim-fugitive`](https://github.com/tpope/vim-fugitive) and [`vim-gitgutter`](https://github.com/airblade/vim-gitgutter) so you can manage your dotfiles without leaving vim.

Whenever a buffer is loaded, uses `yadm ls-files` to detect if the current file is tracked by `yadm`. If it is, this:

- runs `FugitiveDetect` from [`vim-fugitive`](https://github.com/tpope/vim-fugitive), so fugitive can act on the `yadm` git repo. All fugitive bindings stay the same
- sets `g:gitgutter_git_executable` to `yadm`, which causes [`vim-gitgutter`](https://github.com/airblade/vim-gitgutter) to show changed git hunks using `yadm`

When you stop editing the dotfile (switch to a file which `yadm` isn't tracking), it resets them back to the defaults (Fugitive does that automatically).

For [gitsigns](https://github.com/lewis6991/gitsigns.nvim) yadm support, see [gitsigns-yadm.nvim](https://github.com/purarue/gitsigns-yadm.nvim)

## Install

This uses [`jobstart`](<https://neovim.io/doc/user/builtin.html#jobstart()>), so it requires `neovim`. Am quite new to writing plugins, so would appreciate feedback and/or contributions for compatibility with vim

Should work with all vim plugin managers -- load the file in `plugin`

For example, using [`lazy`](https://github.com/folke/lazy.nvim):

```lua
{
    'purarue/yadm-git.vim',
    event = "BufWinEnter",
    config = function()
        vim.g.yadm_git_fugitive_enabled = 0
    end
}
```

Configuration:

```vim
let g:yadm_git_enabled = 1 " setting this to 0 disables the plugin
let g:yadm_git_verbose = 0 " prints when it yadm ls-files detects a file

let g:yadm_git_fugitive_enabled = 1
let g:yadm_git_gitgutter_enabled = 1

" the location of your yadm repo (this is the default for yadm)
" used with FugitiveDetect when it detects a dotfile
let g:yadm_git_repo_path = "~/.local/share/yadm/repo.git"
" to 'reset' gitgutter git path after switching buffers
" to a non-dotfile. you probably dont need to set this
let g:yadm_git_default_git_path = "git"
```

### Known Issues

Sometimes when switching back to a git repository from a dotfile, GitGutter doesn't redraw the hunks. This can be fixed by running `:GitGutter` to force a redraw
