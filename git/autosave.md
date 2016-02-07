# Git Autosave

I've recently discovered useful Git configuration snippet which I now use daily
as a means temporal backup before pulls & merges.

Snippet credits goes to [jusinmk](https://github.com/justinmk).

Here it goes.

```
[alias]
    autosave = "!git stash save autosave-$(date +%Y%m%d_%H%M%S)"
```

*How to use:*

```console

λ ./til/ master* git autosave
Saved working directory and index state On master: autosave-20160207_191131
HEAD is now at 08efc4b Initisal version of README

λ ./til/ master git stash apply

```

