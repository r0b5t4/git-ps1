# git-ps1
A simple bash snippet that shows git repo, branch or tag info where it belongs

```
# Custom git prompt

function gw {

    working_dir=$(pwd)
    parent=$working_dir
    prompt=""

    while [ "$parent" != "/" ]; do

        if [ -d "$parent/.git" ]; then
            branch=$(git symbolic-ref --short HEAD 2>/dev/null)
            if [ ! "$branch" ]; then
                branch=$(git describe --all 2>/dev/null)
                if [ $branch ]; then
                    branch=${branch#heads/}
                    branch=${branch#tags/refs/tags/}
                else
                    sha1=$(cat .git/HEAD)
                    branch=${sha1:0:7}
                fi
            fi
            prompt="[$branch]$prompt"
        fi

        base=$(basename $parent)
        prompt="/$base$prompt"

        parent=$(dirname $parent)
    done

    if [[ "$prompt" == "$HOME"* ]]; then
        prompt="~"${prompt#$HOME}
    fi

    echo $prompt
}

# plain prompt
#PS1="\u@\h:\$(gw)$ "

# debian colour prompt
PS1="${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\$(gw)\[\033[00m\]\$ "
```

The git repo info will be shown inline.

`robsta@ATLT0551:~/WSL2-Linux-Kernel[linux-msft-wsl-5.10.74.3]/arch/x86/boot$`
