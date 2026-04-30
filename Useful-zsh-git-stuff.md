# Useful zsh & git things

## Prompt showing working directory and git branch
`.zshrc`
```shell
function parse_git_branch() {
git branch --show-current 2>/dev/null
}

setopt PROMPT_SUBST
#PROMPT='%F{cyan}%n@%m %F{yellow}%~ [%F{green}$(parse_git_branch)]%f %# '
PROMPT='%F{cyan}%~ %F{green}[$(parse_git_branch)]%f %# '
```

### Change to add modified indicator prompt

```shell
 function parse_git_branch() {
    local branch=$(git branch --show-current 2>/dev/null)
    local gstatus=$(git status --porcelain 2>/dev/null)
    [[ -n $gstatus ]] && branch="${branch}*"
    echo $branch
}
```



