# Sign Your Commit

The information presented here is mostly from
[Git Tools Signing Your Work](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work).

* Let's start by creating PGP key first.

```console
$ gpg --gen-key
```

* Lets list the PGP keys

```console
$ gpg --list-keys
/Users/schacon/.gnupg/pubring.gpg
---------------------------------
pub   2048R/0A46826A 2016-02-07 [expires: 2017-02-06]
uid                  Leonids Maslovs (leonardinius Git-Sign-Key) <leonids.maslovs@xyz.com>
sub   2048R/874529A9 2016-02-07 [expires: 2017-02-06]
```

Let's make a notion of public part of the key.

```text
       pub   2048R/0A46826A
                   _^
    --------------/
```

* Let's configure Git to use this key by default

```console
$ git config --global user.signingkey 0A46826A
```

----

* Let's sign some Git commit

Please pay attention to `-S` argument.

```console
$ git commit -a -S -m 'signed commit'

You need a passphrase to unlock the secret key for
user: "Leonids Maslovs (leonardinius Git-Sign-Key) <leonids.maslovs@xyz.com>"
4096-bit RSA key, ID 0A46826A, created 2016-02-07

[master 5c3386c] signed commit
  ...
```

* Let's view git logs

```console
$ git log --show-signature -1
commit 66edf883a7c444076b20b5a0ff07c749fd5b98bb
gpg: Signature made Sun 07 Feb 2016 06:32:48 PM EET using RSA key ID 0A46826A
gpg: Good signature from "Leonids Maslovs (leonardinius Git-Sign-Key) <leonids.maslovs@xyz.com>"
Author: Leonids Maslovs <leonids.maslovs@xyz.com>
Date:   Sun Feb 7 18:32:48 2016 +0200

    signed commit

# -----------

$ git log --pretty="format:%h %G? %aN  %s"

bb1464f N Leonids Maslovs  signed commit
aa56c76 N Leonids Maslovs  jekyll_3.0: URLs fix by permalinks (Fixes disquss comments)
f5000a0 N Leonids Maslovs  Revert "jekyll_3.0: url fix"
```

* Merges, verify signatures

```console
$ git merge --verify-signatures -S  signed-branch
Commit 13ad65e has a good GPG signature by Leonids Maslovs (leonardinius Git-Sign-Key) <leonids.maslovs@xyz.com>

You need a passphrase to unlock the secret key for
user: "Leonids Maslovs (leonardinius Git-Sign-Key) <leonids.maslovs@xyz.com>"
4096-bit RSA key, ID 0A46826A, created 2016-02-07

Merge made by the 'recursive' strategy.
 README | 2 ++
 1 file changed, 2 insertions(+)
```
