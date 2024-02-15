# _degit_

Usage:

`degit <src>[#ref] [<dest>] [options]`

Fetches the `src` repo, and extracts it to `dest` (or the current directory).

The `src` argument can be any of the following:

## GitHub repos

user/repo
github:user/repo
https://github.com/user/repo

## GitLab repos

gitlab:user/repo
https://gitlab.com/user/repo

## BitBucket repos

bitbucket:user/repo
https://bitbucket.com/user/repo

## Sourcehut repos

git.sr.ht/user/repo
git@git.sr.ht:user/repo
https://git.sr.ht/user/repo

You can append a #ref to any of the above:

## Branches (defaults to master)

user/repo#dev

## Tags

user/repo#v1.2.3

## Commit hashes

user/repo#abcd1234

The `dest` directory (or the current directory, if unspecified) must be empty
unless the `--force` option is used.

Options:

  `--help`,    `-h`  Show this message
  `--cache`,   `-c`  Only use local cache
  `--force`,   `-f`  Allow non-empty destination directory
  `--verbose`, `-v`  Extra logging
  `--mode=`,   `-m=` Force the mode by which degit clones the repo
                     Valid options are `tar` or `git` (uses SSH)
  `--github`,  `-g` Initialize a private GitHub repository within the current working directory.
  `--public`,  `-p` Switch to create a public GitHub repository. This must be paired with the `--github` option.
  `--git`           A flag to execute `git init` within the current working directory.
  `--subdir=`, `-s` Retrieve a specific subdirectory in GitHub CLI mode.


See https://github.com/xiangnanscu/degit for more information
