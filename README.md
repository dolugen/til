# TIL <!-- omit in toc -->

Today I Learned

Inspired by [jbranchaud](https://github.com/jbranchaud/til/) and [the other til repo](/).

Will keep the notes here until there are enough to organize.

- [AWS](#aws)
  - [List Stacks](#list-stacks)
  - [Delete Stack](#delete-stack)
- [Git](#git)
  - [List TODOs in the current branch](#list-todos-in-the-current-branch)
- [Go](#go)
  - [install go](#install-go)
  - [go get installs where?](#go-get-installs-where)

## AWS

### List Stacks

```
aws cloudformation list-stacks --profile <profile-name-optional> | jq '.StackSummaries | .[] |  {name: .StackName, status: .StackStatus}'
```

### Delete Stack

```
aws cloudformation delete-stack --stack-name place-indexer --profile <profile-name-optional>
```

## Git

### List TODOs in the current branch

```
git diff master..$(git branch --show-current) | grep TODO
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
