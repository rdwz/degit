# xtt — straightforward project scaffolding
[![install size](https://badgen.net/packagephobia/install/xtt)](https://packagephobia.now.sh/result?p=xtt)
[![npm package version](https://badgen.net/npm/v/xtt)](https://npm.im/xtt)

## Enhancement from the origin degit

- Use `degit some-repo` as a shortcut for `degit some-user/some-repo`

- Alias `xxt` for `degit`

If you're already logged into [GitHub CLI](https://cli.github.com/) (for example, as `some-user`), executing `degit some-repo` will implicitly act as if you entered `degit some-user/some-repo`.

- `--github` option to initialize a GitHub repository in your current directory

- `--public` flag to create a public GitHub repository. This must be used in conjunction with the `--github` flag.

- `--git` command to perform a `git init` in your current working directory

- `--subdir=` parameter to define a subdirectory within a GitHub repository when operating in GitHub CLI mode

## Introduction

**degit** makes copies of git repositories. When you run `degit some-user/some-repo`, it will find the latest commit on https://github.com/some-user/some-repo and download the associated tar file to `~/.degit/some-user/some-repo/commithash.tar.gz` if it doesn't already exist locally. (This is much quicker than using `git clone`, because you're not downloading the entire git history.)

_Requires Node 8 or above, because `async` and `await` are the cat's pyjamas_


## Installation

```bash
npm install -g xtt
```

## Usage

### GitHub CLI mode
Assuming you're already logged into [GitHub CLI](https://cli.github.com/) (for example, as `user`)
```bash
# same as `degit user/repo`
degit repo

# initialize a private github repo `user/somedir` from `user/repo` (commit and push immediately)
cd somedir && degit repo --github

# or like this
degit repo somedir --github

# create a public github repo `user/somedir`
degit repo somedir --github --public

# or initialize `user/somedir` from `user/templates-repo/templateA`
degit templates-repo somedir --github --subdir=templateA
```

### Basics

The simplest use of degit is to download the master branch of a repo from GitHub to the current working directory:

```bash
degit user/repo

# these commands are equivalent
degit github:user/repo
degit git@github.com:user/repo
degit https://github.com/user/repo
```

Or you can download from GitLab and BitBucket:

```bash
# download from GitLab
degit gitlab:user/repo
degit git@gitlab.com:user/repo
degit https://gitlab.com/user/repo

# download from BitBucket
degit bitbucket:user/repo
degit git@bitbucket.org:user/repo
degit https://bitbucket.org/user/repo

# download from Sourcehut
degit git.sr.ht/user/repo
degit git@git.sr.ht:user/repo
degit https://git.sr.ht/user/repo
```

### Specify a tag, branch or commit

The default branch is `master`.

```bash
degit user/repo#dev       # branch
degit user/repo#v1.2.3    # release tag
degit user/repo#1234abcd  # commit hash
````

### Create a new folder for the project

If the second argument is omitted, the repo will be cloned to the current directory.

```bash
degit user/repo my-new-project
```

### Specify a subdirectory

To clone a specific subdirectory instead of the entire repo, just add it to the argument:

```bash
degit user/repo/subdirectory
```

### HTTPS proxying

If you have an `https_proxy` environment variable, Degit will use it.

### Private repositories

Private repos can be cloned by specifying `--mode=git` (the default is `tar`). In this mode, Degit will use `git` under the hood. It's much slower than fetching a tarball, which is why it's not the default.

Note: this clones over SSH, not HTTPS.

### See all options

```bash
degit --help
```

## Wait, isn't this just `git clone --depth 1`?

A few salient differences:

- If you `git clone`, you get a `.git` folder that pertains to the project template, rather than your project. You can easily forget to re-init the repository, and end up confusing yourself
- Caching and offline support (if you already have a `.tar.gz` file for a specific commit, you don't need to fetch it again).
- Less to type (`degit user/repo` instead of `git clone --depth 1 git@github.com:user/repo`)
- Composability via [actions](#actions)
- Future capabilities — [interactive mode](https://github.com/Rich-Harris/degit/issues/4), [friendly onboarding and postinstall scripts](https://github.com/Rich-Harris/degit/issues/6)

## JavaScript API

You can also use degit inside a Node script:

```js
const degit = require('degit');

const emitter = degit('user/repo', {
	cache: true,
	force: true,
	verbose: true,
});

emitter.on('info', info => {
	console.log(info.message);
});

emitter.clone('path/to/dest').then(() => {
	console.log('done');
});
```

## Actions

You can manipulate repositories after they have been cloned with _actions_, specified in a `degit.json` file that lives at the top level of the working directory. Currently, there are two actions — `clone` and `remove`. Additional actions may be added in future.

### clone

```json
// degit.json
[
	{
		"action": "clone",
		"src": "user/another-repo"
	}
]
```

This will clone `user/another-repo`, preserving the contents of the existing working directory. This allows you to, say, add a new README.md or starter file to a repo that you do not control. The cloned repo can contain its own `degit.json` actions.

### remove

```json
// degit.json
[
	{
		"action": "remove",
		"files": ["LICENSE"]
	}
]
```

Remove a file at the specified path.

## See also

- [zel](https://github.com/vutran/zel) by [Vu Tran](https://twitter.com/tranvu)
- [gittar](https://github.com/lukeed/gittar) by [Luke Edwards](https://twitter.com/lukeed05)

## License

[MIT](LICENSE.md).
