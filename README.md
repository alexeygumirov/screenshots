# Bash functions and scripts with screenshots

This is the project containing screenshots of my scripts and some examples of them.

For all functions with the `fzf` preview I have standard key bindings:
- **F1**: preview page down
- **F2**: preview scroll down
- **F3**: preview scroll up
- **F4**: preview page up
- **Ctrl + A**: select all
- **Ctrl + Space**: deselect all

## Generic bash functions

### `cd` command enhancement with [`fzf`][fzf-link]

This functions relies on [`fzf`][fzf-link], [`fd`][fd-link] and [`bat`][bat-link].

```sh
function cd-fuzzy(){
    local FZF_header="Find folder to go to."
    local DIRECTORY_FZF="$(fd --follow --type d --hidden --exclude .git . | \
        fzf +m --header="$FZF_header" --reverse --height 60% \
        --bind=change:up,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up \
        --border --preview 'tree --du -h -L 1 -a -C --dirsfirst {}' --preview-window=right:40%)"
    cd "$DIRECTORY_FZF"
}
```

![cd-fuzzy() preview](img/cdf_with_fzf.png)

### Preview of files in terminal (non-binary)

This functions relies on [`fzf`][fzf-link], [`fd`][fd-link] and [`bat`][bat-link].

```sh
function preview(){
    if [[ ! -z $1 ]];then
        local FZF_header="Preview of \`$1\` files."
        fd --type f -e $1 --hidden --follow --exclude .git . | \
            fzf +m --header="$FZF_header" --reverse \
            --bind=change:up,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up \
            --preview 'bat --style=numbers --color=always {}' --preview-window=right:75%:wrap
    else
        local FZF_header="\`preview <ext>\` shows files of defined extension."
        fd --type f --hidden --follow --exclude .git . | \
            fzf +m --header="$FZF_header" --reverse \
            --bind=change:up,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up \
            --preview 'bat --style=numbers --color=always {}' --preview-window=right:75%:wrap
    fi
}
```

![preview() screenshot](img/preview_with_fzf.png "preview function screenshot")

## Git functions

### Git-add with `fzf`

`git-add` function improved with [`fzf`][fzf-link]. It provides preview of the file modifications using `git-diff`.

```sh
function gitadd-fuzzy(){
    local FZF_header="\`git-add\`. Select files."
    git add $(git ls-files --modified --others --exclude-standard | \
        fzf -m --header="$FZF_header" --reverse --border \
        --bind=change:up,ctrl-a:select-all,ctrl-space:deselect-all,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up \
        --preview 'git diff --color=always {} | bat -l"Git Commit" --color=always' --preview-window=right:75%)
}
```

![gitadd-fuzzy screenshot](img/git-add_with_fzf.png)

### Git-diff with `fzf`

`git-diff` function improved with [`fzf`][fzf-link].

function gitdiff-fuzzy(){
    local FZF_header="\`git-diff\`. Select files."
    git status -s | grep "^ M" | cut -d" " -f3 | \
        fzf +m --header="$FZF_header" --reverse --border \
        --bind=change:up,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up \
        --preview 'git diff --color=always {} | bat -l"Git Commit" --color=always' --preview-window=right:75%:wrap
}

![gitdiff-fuzzy screenshot](./img/git-diff_with_fzf.png)

<!-- Links -->
[fzf-link]: https://github.com/junegunn/fzf
[fd-link]: https://github.com/sharkdp/fd 
[bat-link]: https://github.com/sharkdp/bat
