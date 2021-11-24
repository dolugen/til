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
  - [Clone all repos of a GitHub user/organization](#clone-all-repos-of-a-github-userorganization)
- [Go](#go)
  - [install go](#install-go)
  - [go get installs where?](#go-get-installs-where)
- [Jupyter Lab](#jupyter-lab)
  - [Save cell contents as file](#save-cell-contents-as-file)
  - [Code formatting](#code-formatting)
- [Kubernetes](#kubernetes)
  - [Working with contexts](#working-with-contexts)
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
  - [cargo addons](#cargo-addons)
    - [cargo-edit](#cargo-edit)
  - [rustup](#rustup)
    - [install nightly toolchain](#install-nightly-toolchain)
    - [list installed toolchains](#list-installed-toolchains)
    - [set default toolchain](#set-default-toolchain)
    - [update toolchain](#update-toolchain)
    - [update rustup itself](#update-rustup-itself)
    - [open rust bookshelf](#open-rust-bookshelf)
- [shell](#shell)
  - [Access the most recent parameter](#access-the-most-recent-parameter)
  - [Get my computer's MAC addresses](#get-my-computers-mac-addresses)
  - [Operate on files with names starting with hyphen](#operate-on-files-with-names-starting-with-hyphen)
- [imagemagick](#imagemagick)
  - [Combine images side by side](#combine-images-side-by-side)
  - [Rotate an image 90 degrees](#rotate-an-image-90-degrees)
  - [Convert an image format](#convert-an-image-format)
  - [Convert format of multiple files](#convert-format-of-multiple-files)
- [exiftool](#exiftool)
  - [Remove EXIF metadata from photos](#remove-exif-metadata-from-photos)
- [VS Code](#vs-code)
  - [Useful navigation shortcuts](#useful-navigation-shortcuts)
  - [Enable `code-insiders` command for the Insiders version](#enable-code-insiders-command-for-the-insiders-version)
  - [List installed extensions](#list-installed-extensions)
  - [Run selection/current line in Python REPL](#run-selectioncurrent-line-in-python-repl)

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

### Clone all repos of a GitHub user/organization

Sometimes you want to clone all the repos from an organization. You can do this using `gh`, `jq` and `xargs`.

Here `<username>` can be a GitHub user or organization.

```bash
gh repo list <username> --json nameWithOwner --jq ".[] | .nameWithOwner" | xargs -I REPO gh repo clone REPO -- REPO
```

Be aware that this may copy hundreds of repos to your machine, so you might want to check how many repos the user have with `gh repo list <username>` first.

Use `-L` or `--limit` to specify the number of repos to clone.

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

## Kubernetes

### Working with contexts

List available contexts:
```
kubectl config get-contexts
```

Show current context:
```
kubectl config current-context
```

Change context:
```
kubectl config use-context <context name>
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

### cargo addons

#### [cargo-edit](https://github.com/killercup/cargo-edit)

Extends cargo to have commands such as `cargo add`, making `Cargo.toml` management a breeze.

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

#### open rust bookshelf
Opens local copy of Rust documentation, including _The Rust Programming Language_.
```
rustup doc
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

### Get my computer's MAC addresses

```bash
ifconfig | grep ether | awk '{print $2}'
```

### Operate on files with names starting with hyphen

When filenames start with a hyphen and commands may interpret it as argument.

For example, you have a file called `-test` and want to rename this to `test`. This won't work:

```bash
mv -test test
mv: illegal option -- t
usage: mv [-f | -i | -n] [-v] source target
       mv [-f | -i | -n] [-v] source ... directory
```

Use the `--` signal to end option processing before typing the filename.

```bash
mv -- -test test
```

This works with other commands as expected:

```bash
touch -- -myfile
rm -- -myfile
```


## imagemagick

Friendly reminder: imagemagick commands may contain hidden magic and can be destructive -- be sure to backup and work on the copy of your precious images.

### Combine images side by side

```
montage -geometry 500x500 [input-file1 input-file2...] output-file
```

### Rotate an image 90 degrees

```
convert -rotate "90" original.gif rotated-output.gif
```

### Convert an image format

This will create a copy of the image with a new format:
```
convert input.heic output.jpg
```

### Convert format of multiple files

If you have a bunch of HEIC files and want to convert all of them to JPG:

```
mogrify -format jpg *.heic
```

## exiftool

### Remove EXIF metadata from photos

This will remove all metadata from the given photo file.

```
exiftool -all= photo.jpg
```

Here the `photo.jpg` will be stripped of the metadata, and a copy of the original will be created. To overwrite the original without preserving a copy, use the `-overwrite_original` option.

## VS Code

### Useful navigation shortcuts

- Find file by name: <kbd>cmd</kbd> + <kbd>p</kbd>
- Go to symbol in the currently open file: <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>o</kbd>
- Go to symbol: <kbd>cmd</kbd> + <kbd>t</kbd>

### Enable `code-insiders` command for the [Insiders](https://code.visualstudio.com/insiders/) version

Within the editor, open command executor with <kbd>cmd</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> and type "Install code-insiders command in PATH" and execute it.

### List installed extensions
```
code --list-extensions
```

### Run selection/current line in Python REPL

Tap <kbd>shift</kbd>+<kbd>enter</kbd>. This will open a Python repl and execute the selected code.

This is also available via the command view (<kbd>cmd</kbd>+<kbd>shift</kbd>+<kbd>p</kbd>). Open the command view and type "python run line" (partial input works).
