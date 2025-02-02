fzf-git.sh
==========

bash and zsh key bindings for Git objects, powered by fzf.

Each binding will allow you to browse through Git objects of a certain type,
and select the objects you want to paste to your command-line.

Installation
------------

1. Install the latest version of [fzf][fzf]
   * (Optional) Install [bat](https://github.com/sharkdp/bat) or batdiff for
     syntax-highlighted file previews
2. Source [fzf-git.sh](https://raw.githubusercontent.com/junegunn/fzf-git.sh/main/fzf-git.sh) file from your .bashrc or .zshrc

Usage
-----

* List of bindings

  * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-F `</kbd>` for **F**iles
  * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-B `</kbd>` for **B**ranches
  * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-T `</kbd>` for **T**ags
  * ~~`<kbd>`CTRL-x `</kbd><kbd>`CTRL-R `</kbd>` for **R**emotes~~
  * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-H `</kbd>` for commit **H**ashes
  * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-S `</kbd>` for **S**tashes
  * ~~`<kbd>`CTRL-x `</kbd><kbd>`CTRL-E `</kbd>` for **E**ach ref (`git for-each-ref`)~~

  > ⚠️ You may have issues with these bindings in the following cases:
  >
  > * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-B `</kbd>` will not work if
  >   `<kbd>`CTRL-B `</kbd>` is used as the tmux prefix
  > * `<kbd>`CTRL-x `</kbd><kbd>`CTRL-S `</kbd>` will not work if flow control is enabled,
  >   `<kbd>`CTRL-S `</kbd>` will freeze the terminal instead
  >   * (`stty -ixon` will disable it)
  >
* Inside fzf

  * `<kbd>`TAB `</kbd>` or `<kbd>`SHIFT-TAB `</kbd>` to select multiple objects
  * `<kbd>`CTRL-/`</kbd>` to change preview window layout
  * `<kbd>`CTRL-O `</kbd>` to open the object in the web browser (in GitHub URL scheme)

## Showcase

* Git Commit

<img width="1680" alt="image" src="./images/gitcommits.png">

* Git Fils

<img width="1680" alt="image" src="./images/gitfiles.png">

* Git Branch

<img width="1680" alt="image" src="./images/gitbranchs.png">

* git stash

<img width="1680" alt="image" src="./images/gitstash.png">

Customization
-------------

```sh
# Redefine this function to change the options
_fzf_git_fzf() {
  fzf-tmux -p80%,60% -- \
    --layout=reverse --multi --height=50% --min-height=20 --border \
    --color='header:italic:underline' \
    --preview-window='right,50%,border-left' \
    --bind='ctrl-/:change-preview-window(down,50%,border-top|hidden|)' "$@"
}
```

Defining shortcut commands
--------------------------

Each binding is backed by `_fzf_git_*` function so you can do something like
this in your shell configuration file.

```sh
gco() {
  local selected=$(_fzf_git_each_ref --no-multi)
  [ -n "$selected" ] && git checkout "$selected"
}
```
