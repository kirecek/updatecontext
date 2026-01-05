# kubectl updatecontext

[![GitHub Release](https://img.shields.io/github/release/borisputerka/updatecontext.svg?style=flat)](https://github.com/borisputerka/updatecontext/releases)
[![Go Report Card](https://goreportcard.com/badge/github.com/borisputerka/updatecontext)](https://goreportcard.com/report/github.com/borisputerka/updatecontext)

[Kubectl](https://github.com/kubernetes/kubectl) plugin that manages kubernetes contexts. It will create context in `<namespace/<cluster>` format. Contexts for non-existent namespaces will be deleted (e.g., after you delete namespace with created context).


To switch contexts you can use [kubectx](https://github.com/ahmetb/kubectx) or [fzf](https://github.com/junegunn/fzf) (see instructions below). Once you have contexts created you will no longer need to use `kubectx` with `kubens` when accessing namespace in different cluster.

# Installation and usage

> **Info**: This plugin is not in [krew-index](https://github.com/kubernetes-sigs/krew-index) but for convenience I added krew manifest as installation option..

1. Install plugin remotely from git
    ```bash
    kubectl krew install --manifest-url=https://raw.githubusercontent.com/borisputerka/updatecontext/main/deploy/krew/plugin.yaml
    ```

2. Install plugin locally from cloned git repository
    ```bash
    git clone https://github.com/borisputerka/updatecontext.git
    cd updatecontext
    kubectl krew install --manifest=deploy/krew/plugin.yaml
    ```

3. Install plugin from local repository using Makefile
    ```bash
    git clone https://github.com/borisputerka/updatecontext.git
    cd updatecontext
    make bin
    ```
    Then add the binary to your PATH:
    ```bash
    export PATH=$PATH:$(pwd)/bin
    ```

## Usage

```bash
kubectl updatecontext
```

# Use fzf to switch contexts

Add these lines into your `.bashrc` or `.zshrc`

```bash
#fzf inline alias
alias _inline_fzf="fzf --multi --ansi -i -1 --height=50% --reverse -0 --header-lines=1 --inline-info --border"

#kubernetes contexts switcher
kcs() {
    local context="$(kubectl config get-contexts | _inline_fzf | awk '{print $1}')"
    eval kubectl config set current-context "${context}"
}
```

Now use `kcs` within your terminal.

```bash
kcs
```

