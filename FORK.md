# About this fork

This is a personal fork of [storytold/wordcraft](https://github.com/storytold/wordcraft). Its
`main` is the official `main` plus this fork's own commits, such as the fix that lets Paste fill
several selected table cells.

| Remote | Points to | Used for |
|---|---|---|
| `origin` | `facufrastas/wordcraft` (this fork) | `git push` / `git pull` |
| `upstream` | `storytold/wordcraft` (official) | getting the latest official changes |

## Getting the latest from the official repository

1. Start from a clean `main`:

   ```sh
   cd ~/dev/General/WordCraft
   git switch main
   git status
   ```

   `git status` should say "nothing to commit". If not, commit your work or run `git stash` first.

2. Fetch the official changes and replay this fork's commits on top of them:

   ```sh
   git fetch upstream
   git rebase upstream/main
   ```

3. Push the updated `main` to the fork:

   ```sh
   git push --force-with-lease
   ```

   A rebase rewrites this fork's commits, so a plain push is refused and the force is needed. With
   `--force-with-lease`, git still refuses if the fork has changed since your last fetch.

To check what the fork adds on top of the official repository:

```sh
git log --oneline upstream/main..main
```

### If the rebase stops with a conflict

This happens when the official code changed the same lines as one of the fork's commits. Git
lists the files with conflicts.

1. Edit each file listed and resolve the parts between `<<<<<<<` and `>>>>>>>`.
2. Mark it resolved with `git add <file>`.
3. Run `git rebase --continue`.

To give up and go back to how things were, run `git rebase --abort`.

### If a fork commit is now in the official repository

If the official project merged one of this fork's changes, the rebase usually drops the fork's copy
by itself. If it stops on that commit instead, run `git rebase --skip`.

## Translations and fonts

Official builds embed the translations and fonts from two other repositories. Here they are checked
out next to this folder, in `~/dev/General/craft-i18n` and `~/dev/General/craft-fonts`.

`craft-i18n.rev` holds the craft-i18n commit to build with. After an update, move the checkout to it:

```sh
git -C ../craft-i18n fetch
git -C ../craft-i18n checkout "$(cat craft-i18n.rev)"
```

The craft-fonts commit is the `craft-fonts-ref` in `.github/workflows/release.yml`.
