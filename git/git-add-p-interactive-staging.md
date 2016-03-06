# Git interactive staging (add -p)

Interactive staging (or `git add -p`) is one of a few Git interactive commands
I became so fond of recently (another one I use quite frequently over the years
is `git rebase -i`).

In few words it displays the diff hunks and provides options either to stage
all hunks, edit each one etc..

## Example

**Initial repo**
```console
λ /tmp/tmp.KCvUH5bMFv/repo/ master* git st
## master
 M bash-check-port--dev-tcp.md
```

**Let's choose to dit the hunk'**
```console
λ /tmp/tmp.KCvUH5bMFv/repo/ master* git add -p
diff --git a/bash-check-port--dev-tcp.md b/bash-check-port--dev-tcp.md
index d9c5634..2400f05 100644
--- a/bash-check-port--dev-tcp.md
+++ b/bash-check-port--dev-tcp.md
@@ -1,10 +1,8 @@
 # Shell: check tcp port

-Periodically I find myself in th need to check for TCP port availability; and
+Periodically I find myself in the need to check for TCP port availability; and
 no convenient tools available and not possible to install one.

-Usually I use `netcat -z -v cidrc port` or telnet.
-
 Lately I've found quite useful some under-appreciated feature of `bash` shell.
 See [/dev/tcp][dev-tcp].

Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]? e
```
Please note, we have chosen `e` (`e` stands for `edit`).

**Hunk edit mode**
Please pay attention to the notes below.
```diff
# Manual hunk edit mode -- see bottom for a quick guide
@@ -1,10 +1,8 @@
 # Shell: check tcp port

-Periodically I find myself in th need to check for TCP port availability; and
+Periodically I find myself in the need to check for TCP port availability; and
 no convenient tools available and not possible to install one.

-Usually I use `netcat -z -v cidrc port` or telnet.
-
 Lately I've found quite useful some under-appreciated feature of `bash` shell.
 See [/dev/tcp][dev-tcp].

# ---
# To remove '-' lines, make them ' ' lines (context).
# To remove '+' lines, delete them.
# Lines starting with # will be removed.
#
# If the patch applies cleanly, the edited hunk will immediately be
# marked for staging. If it does not apply cleanly, you will be given
# an opportunity to edit again. If all lines of the hunk are removed,
# then the edit is aborted and the hunk is left unchanged.
```

## More information

It's not very easy describe the feature _literally_ in few words, so will leave
the link to git scm book here
https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging
