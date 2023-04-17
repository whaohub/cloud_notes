[toc]

## Linux add user

```shell
useradd -s /bin/bash -m username
passwd username
sermod -aG sudo username  # su
```

## linux command with space

命令别名时使用alias 命名带空格的命令无法生效

```shell
alias git status='git status --ignore-submodules' # not work

# replace
git() {
    if [[ $@ == "status" ]]; then
        command git status --ignore-submodules
    else
        command git "$@"
    fi
}
```

