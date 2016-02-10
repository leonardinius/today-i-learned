I've recently discovered a useful Git operations/work mode: [Git Worktrees][1].

Now, during the writeup I've performed quick web-search and found following
interesting fabulous manuals:
- [Github take on worktrees][1] (Git 2.5 release announcement)
- [Atlassian take on worktrees][2] (Git 2.x release announcements)

**tl;dr;**

It allows you to maintain multiple checked out work trees of the same root
repository.

What's it in for me?

Before this command existed my usual hotfix or similar endeavors would require
me to either have multiple clones of the same repo; OR stash the current
uncommitted changes and checkout a different branch, apply changes there
commit; switch back, unstash etc etc ... Boring.

So, I would either need separate clones (sync and storage issue) or use complex
workflow procedure.

With worktrees I can have local checkouts (not full clones) in parallel to my
main repository clone and work there as in multiple parallel clones aware each
of other. Slick.

E.g. let's consider we have the following repo

```console
λ /test-repo/ develop git branch -a
* develop
  master
```

And now we want to checkout latest PROD version (`master`) and check some stuff
there w/o messing up with the local develop stuff. Easy.

```console
λ /test-repo/ develop git worktree add master master
  Preparing master (identifier master)
  HEAD is now at c0db3b1 Add README.txt
```

We found our usual suspect and now we need to work on hotfix on top of latest
wip branch (`develop`). Let's start clean w/o messing with the local changes.

```console
λ /test-repo/ develop* git worktree add -b old-develop ../old-develop
  Preparing ../old-develop (identifier old-develop)
  HEAD is now at c0db3b1 Add README.txt
```

_Ough_, I forgot there I was. Please remind me.

```console
λ /test-repo/ develop* git worktree list
  /test-repo         0cf1f61 [develop]
  /test-repo/master  4c77cea [master]
  /old-develop       c0db3b1 [old-develop]
```

**Please pay attention**: `master` is a subfolder and `old-develop` is on the
same level as `develop` folder itself. Cool.

And now I don't need all this crap lying around.

```console
λ /test-repo/ develop* rm -rf master
λ /test-repo/ develop rm -rf ../old-develop
λ /test-repo/ develop git worktree prune
λ /test-repo/ develop git worktree list
 /test-repo  0cf1f61 [develop]
```

_Vuala_
<!-- References -->

[1]: https://git-scm.com/docs/git-worktree "Git Worktrees"
[2]: https://github.com/blog/2042-git-2-5-including-multiple-worktrees-and-triangular-workflows#git-worktree-one-git-repository-with-multiple-working-trees "Github take on worktrees"
[3]: https://developer.atlassian.com/blog/2015/10/cool-features-git-2.x/#better-handling-of-multiple-clones-git-worktree "Atlassian take on worktrees"
