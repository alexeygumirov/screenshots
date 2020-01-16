# Screenshots

This is the project containing screenshots of my scripts and some examples of them.

## Preview in bash

This functions relies on `fzf`, `fd` and `bat`.

```sh
function preview(){
    if [[ ! -z $1 ]];then
        local FZF_header="Preview of \`$1\` files."
        fd --type f -e $1 --hidden --follow --exclude .git . | fzf +m --header="$FZF_header" --reverse --bind=change:up,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up --preview 'bat --style=numbers --color=always {}' --preview-window=right:75%:wrap
    else
        local FZF_header="\`preview <ext>\` shows files of defined extension."
        fd --type f --hidden --follow --exclude .git . | fzf +m --header="$FZF_header" --reverse --bind=change:up,f1:preview-page-down,f2:preview-down,f3:preview-up,f4:preview-page-up --preview 'bat --style=numbers --color=always {}' --preview-window=right:75%:wrap
    fi
}
```

![preview() screenshot](img/preview_with_fzf.png "preview function screenshot")


