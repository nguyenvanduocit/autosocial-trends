---
source: hackernews
title: Archiving Git branches as tags
url: https://etc.octavore.com/2025/12/archiving-git-branches-as-tags/
date: 2025-12-26
---

[et cetera](https://etc.octavore.com)

* [Home](https://etc.octavore.com/)
* 🌓

# Archiving git branches as tags

22 December 2025

I have a spicy git alias that allows me to "archive" old git branches by converting them to tags, making them less visible across git tooling.

```
# ~/.gitconfig
[alias]
archive-branch = "!f() { \
    : git switch; \
    local git_branch=${1:-$(git branch --show-current)}; \
    git co main && \
      git tag archive/$git_branch $git_branch && \
      git branch -D $git_branch; \
  }; f"
```

The magic of this alias is that aside from tagging and deleting the branch as desired, it also adds support for shell completion so I can tab complete the branch I want to archive (if not the current branch I'm on).

This is accomplished by the strange magic of wrapping the alias in an immediately executed bash function (`!f() { ... }; f`) and having the first line be `: git switch`. `git switch` isn't actually run; it's a hint to git to use the same completion for this alias as `git switch` ([via](https://stackoverflow.com/questions/57417378/how-can-i-make-tab-completion-for-my-git-alias-behave-like-an-existing-command?ref=etc.octavore.com) and [source](https://github.com/git/git/blob/45547b60aca32b45d2f1ef93462cf9df28637c13/contrib/completion/git-completion.bash?ref=etc.octavore.com#L26-L32)).

**Important note**: the strange magic requires the official git completion script. `zsh` comes with its own [git completion](https://github.com/zsh-users/zsh/blob/master/Completion/Unix/Command/_git?ref=etc.octavore.com) which does not support specifying the completion style, so you'll also have to make sure `zsh` loads the official completion. On macOS, if using the Xcode Developer Tools version of git, you can link it into `/usr/local/share/zsh/site-functions` like so:

```
sudo mkdir -p /usr/local/share/zsh/site-functions
sudo ln -s /Applications/Xcode.app/Contents/Developer/usr/share/git-core/git-completion.zsh \
  /usr/local/share/zsh/site-functions/_git
```

And then adding the following to `.zshrc`, because `git-completion.zsh` relies on `git-completion.bash`.

```
# ~/.zshrc
# you may already have this line
autoload -Uz compinit && compinit

zstyle ':completion:*:*:git:*' script /Applications/Xcode.app/Contents/Developer/usr/share/git-core/git-completion.bash
```

Credit for original idea goes to this [reddit thread](https://www.reddit.com/r/git/comments/1gi6idg/whats_good_practice_for_archiving_old_branches/?ref=etc.octavore.com).

octavore © 2025

[Sign in](#/portal/signin)
[Subscribe](#/portal/signup)
