# Git

## Favorite command

```
git add . && git commit --amend --no-edit && git push --force-with-lease
```

## Sign commits on Github

Main [article](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification).

### Step 1 - Produce a GPG key

Either an existing one, check with:
```
gpg --list-secret-keys --keyid-format=long
```

Or create a new one:
1. Run `gpg --full-generate-key`
1. Choose your key type, default `RSA and RSA` is fine, so press `Enter`.
1. Choose the key size, must be at least 4096 bits.
1. Choose the length of time the key should be valid. Default doesn't expire, so press `Enter`.
1. Choose `y` if everything is correct.

#### Enter `user ID` information:

1. Name, use same as Github.
1. Email, use one that you use to commit.
1. Comment, can be empty.
1. Choose `o` for okay.

Now Linux will prompty you to enter a password for the new key.

### Step 1a - Add extra emails

Usually there is more than one email used to commit, so to enter a second email.

#### Get your GPG key ID:
```
gpg --list-secret-keys --keyid-format=long
```
Returns something like:
```
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```
The key ID is the line with `sec` so `3AA5C34371567BD2`.

#### With the key ID do:

1. Run `gpg --edit-key <GPG key ID>`
1. At the `gpg` prompt type `adduid` to add a new `user ID`.
1. Enter `user ID` information as above.
1. At the `gpg` prompt type `save` to store the new `user ID`.

### Step 2 - Export key to Github importable format

To add the GPG key in Github it needs to be in ASCII armor format.

To do that run:
```
gpg --armor --export <GPG key ID>
```
The key will look like this:
```
-----BEGIN PGP PUBLIC KEY BLOCK-----

  ... key content ...

-----END PGP PUBLIC KEY BLOCK-----
```
Copy everything, and enter in Github -> Settings -> SSH and GPG keys -> New GPG key

**Note!** If user ID's are added or remove, the key needs to be exported and imported on Github again.

### Step 3 - Set your GPG signing key in Git

Run:
```
git config --global user.signingkey <GPG key ID>
```

### Step 4 - Sign your commits

Either on a commit by commit basis:
```
git commit -S -m *your commit message*
```
Or configure a single repository to allways sign:
```
git config commit.gpgsign true
```
Or configure git globally to sign:
```
git config --global commit.gpgsign true
```