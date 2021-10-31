# Vim Plugins

Vim is an enormously configurable editor, but you don't need thousands of lines
of opaque VimScript copied from who-knows-where.  Here we'll introduce a common
system for managing Vim plugins, as well as a list of important plugins you
will likely wish to install.

## Plugin managers and vim-plug

Most software beyond a certain level of extensibility includes a package
manager for installing and updating packages. Vim 8 and NeoVim include package
managers natively, and you can learn about your editor's support with the
command `:h packages`.  However, vim users have other third party options.
Three of the most well-known are:

* [junegunn/vim-plug](https://github.com/junegunn/vim-plug)
* [tpope/pathogen](https://github.com/tpope/vim-pathogen)
* [VundleVim/vundle](https://github.com/VundleVim/Vundle.vim)

vim-plug is particularly easy to install and use.  The vim-plug wiki has [some
VimScript](https://github.com/junegunn/vim-plug/wiki/tips#automatic-installation)
for you to paste in your vimrc which will automatically download and install
the plugin.

## Plugin recommendations

* [*tpope/vim-sensible*](https://github.com/tpope/vim-sensible) is a collection
  of defaults that makes sense for almost all users.
* [*tpope/vim-rsi*](https://github.com/tpope/vim-rsi) binds readline-type
  commands (like `^A`, `^K`, etc) in insert and command modes.
* [*tpope/vim-commentary*](https://github.com/tpope/vim-commentary) defines
  handy bindings for commenting and uncommenting code.
* [*tpope/vim-unimpaired*](https://github.com/tpope/vim-unimpaired) defines
  handy toggles and "pairs" of commands, like `yod` to toggle `diffthis`, and
  `[<Space>` and `]<Space>` for inserting blank lines before or after the
  cursor.
* [*tpope/vim-surround*](https://github.com/tpope/vim-surround) defines handy
  bindings for inserting, changing, or deleting surrounding text: `ysiw"` to
  quote a word, `cs"'` to change to single quotes, and `ds'` to remove the
  quotes.
* [*sjl/gundo.vim*](https://github.com/sjl/gundo.vim) provides a visualization
  and navigation mode for the "undo tree" vim maintains.
* [*preservim/nerdtree*](https://github.com/preservim/nerdtree) is the standard
  "file tree" view.
* [*ctrlpvim/ctrlp.vim*](https://github.com/ctrlpvim/ctrlp.vim) provides a
  handy interface for searching among filenames and buffers.
* [*junegunn/fzf*](https://github.com/junegunn/fzf) is a general command-line
  fuzzy finder, but it provides vim functionality as well.
* [*sirver/ultisnips*](https://github.com/sirver/ultisnips) supports inserting
  (perhaps templated, language-aware) snippets.  There are many snippets
  libraries available, including
  [honza/vim-snippets](https://github.com/honza/vim-snippets)
