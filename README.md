# TIL<!-- omit in toc -->

Today I Learned

Inspired by [jbranchaud/til](https://github.com/jbranchaud/til/).

- [AWS](#aws)
  - [List Stacks](#list-stacks)
  - [Delete Stack](#delete-stack)
- [conda](#conda)
- [fzf](#fzf)
  - [List recent Git branches and switch to one](#list-recent-git-branches-and-switch-to-one)
- [Git](#git)
  - [List TODOs in the current branch](#list-todos-in-the-current-branch)
  - [Switch to `main` as the default branch](#switch-to-main-as-the-default-branch)
  - [Add only tracked files](#add-only-tracked-files)
- [gh](#gh)
  - [List my repos](#list-my-repos)
  - [Create PR with a title](#create-pr-with-a-title)
  - [Generate PR title from the branch name](#generate-pr-title-from-the-branch-name)
- [Go](#go)
  - [install go](#install-go)
  - [go get installs where?](#go-get-installs-where)
- [Jupyter Lab](#jupyter-lab)
  - [Save cell contents as file](#save-cell-contents-as-file)
  - [Code formatting](#code-formatting)
- [pandas](#pandas)
  - [Read from newline delimited JSON file](#read-from-newline-delimited-json-file)
- [pyenv](#pyenv)
  - [List installable Python versions](#list-installable-python-versions)
  - [List installed Python versions](#list-installed-python-versions)
  - [Set Python version](#set-python-version)
  - [Set local Python version](#set-local-python-version)
- [Rust](#rust)
  - [cargo](#cargo)
    - [open docs locally](#open-docs-locally)
  - [rustup](#rustup)
    - [install nightly toolchain](#install-nightly-toolchain)
    - [list installed toolchains](#list-installed-toolchains)
    - [set default toolchain](#set-default-toolchain)
    - [update toolchain](#update-toolchain)
    - [update rustup itself](#update-rustup-itself)
- [shell](#shell)
  - [Access the most recent parameter](#access-the-most-recent-parameter)

## AWS

### List Stacks

```bash
aws cloudformation list-stacks --profile <profile-name-optional> | jq '.StackSummaries | .[] |  {name: .StackName, status: .StackStatus}'
```

### Delete Stack

```bash
aws cloudformation delete-stack --stack-name place-indexer --profile <profile-name-optional>
```

## conda

Quick howto on installing and using the conda package manager.

```
brew install miniconda
```

Add to your shell:
```
conda init zsh
```

Create a new environment with a specific Python version:
```
conda create -n notebooks python=3.9
```

List available environments:
```
conda env list
```

Remove an environment:
```
conda env remove --name notebooks
```

Install a package to the current environment:
```
conda install jupyterlab=3
```

## fzf

Pipe any list output to `fzf` and get an interactive fuzzy selector.

### List recent Git branches and switch to one

This works as a shell function. Add it to your `.zshrc` file.

```shell
fbr() {
  local branches branch
  branches=$(git branch --all | grep -v HEAD) &&
  branch=$(echo "$branches" |
           fzf-tmux -d $(( 2 + $(wc -l <<< "$branches") )) +m) &&
  git checkout $(echo "$branch" | sed "s/.* //" | sed "s#remotes/[^/]*/##")
}
```
As seen in [fzf/wiki](https://github.com/junegunn/fzf/wiki/Examples#git)

## Git

### List TODOs in the current branch

```
git diff master..$(git branch --show-current) | grep TODO
```

### Switch to `main` as the default branch

Set `main` as default branch:
```
git config --global init.defaultBranch main
```

Continue reading if you have older repos that need updating.

For older repos, update default branch:
```
git checkout -b main
git push -u origin main
```

On GitHub, you need to update the default too:

1. Go to the repo settings
2. Go to Branches
3. In the Default branch section, click the Switch to another branch button
4. Choose `main` and update

### Add only tracked files

```
git add --update
```
Shorthand:
```
git add -u
```

## gh

### List my repos

```
gh repo list
```

### Create PR with a title

```
gh pr create --title "The PR title"
```

### Generate PR title from the branch name

If you use `PROJ-XXX-slug` pattern for your branch names, and use `PROJ-XXX: slug` for your PR title, you can do a little string munging with `$(git branch --show-current)` and have the PR title generated from the branch name.

```
# insert bash one liner here
```

## Go

### install go

```
brew install go
```

### go get installs where?

I can "get" a module like this:

```
go get github.com/user/repo
```

But where is it installed?

It's in `~/go/bin/`.

## Jupyter Lab

### Save cell contents as file
Using `%%file` magic function, the cell contents will be saved in the specified file. For example:
```
%%file my_script.py
print('hello.py')
```
After executing the cell, you should see `Writing my_script.py` as the cell output, and a new file is created with the contents `print('hello.py')\n`

### Code formatting

Install [jupyter_code_formatter](https://github.com/ryantam626/jupyterlab_code_formatter) extension. This adds a code format button in the notebook toolbar, and two new commands in the Edit menu.

Make sure you have `black` and `isort` installed.

```
pip install jupyterlab_code_formatter
pip install black isort
```

## pandas

### Read from newline delimited JSON file

```python
pd.read_json(filepath, lines=True)
```

## pyenv

### List installable Python versions

```
pyenv install --list
```

### List installed Python versions
```
pyenv versions
```

### Set Python version
```
pyenv global 3.9.0
```

### Set local Python version
When you want to set a Python version for a single workspace.
```
pyenv local 3.7.9
```

## Rust

### cargo

#### open docs locally

This will generate HTML docs of the current crate and its dependencies, and open it in the browser.

```
cargo doc --open
```

### rustup
#### install nightly toolchain
```
rustup toolchain install nightly
```

#### list installed toolchains
```bash
rustup toolchain list
```

#### set default toolchain
```bash
rustup default <toolchain name>

# use nightly
rustup default nightly

# back to stable
rustup default stable
```

#### update toolchain
```
rustup update
```
This will update the everything included in the toolchain, such as `rustc`, `cargo`, `rustfmt`.

#### update rustup itself
```
rustup self update
```

## shell

### Access the most recent parameter
```bash
$_
```
For example, create a directory and change into it:
```bash
mkdir /tmp/mydir && cd $_
```
