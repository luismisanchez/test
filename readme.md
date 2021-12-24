# Git Notes

## Global setup

`git config --global user.name "Luismi Sánchez"`

`git config --global user.email "my@email.com"`

Check global config per user:

`git config --global -e`

Global config is opened with vi, so we can modify (a) and save changes (Esc :wq!)

## Init local repository

Inside the project directory:

`git init`

This will create the `.git` directory where all the control versioning is stored.

Git is changing the convention from `master` branches to `main` so at the moment of writing this notes we'll see this warning (in the system language):

``` bash
ayuda: Usando 'master' como el nombre de la rama inicial. Este nombre de rama predeterminado
ayuda: está sujeto a cambios. Para configurar el nombre de la rama inicial para usar en todos
ayuda: de sus nuevos repositorios, reprimiendo esta advertencia, llama a:
ayuda:
ayuda: 	git config --global init.defaultBranch <nombre>
ayuda:
ayuda: Los nombres comúnmente elegidos en lugar de 'master' son 'main', 'trunk' y
ayuda: 'development'. Se puede cambiar el nombre de la rama recién creada mediante este comando:
ayuda:
ayuda: 	git branch -m <nombre>
```
Change the `master` to the `main` branch globaly:

`git config --global init.defaultBranch main`

## Repository status

Report the status of the followed files and commits:

´git status´

## Add files to the repository

Add files to track changes

`git add filename1 filename2` or `git add wildcards` (e.g. `git add .`)

Track a whole folder

`git add folderPath\`

(*) In order to track an empty directory to the project (e.g. an empty `uploads` folder) we should create an empty file inside it named as `.gitkeep`

## Untrack files

`git reset filename` (or wildcard)

## Commit to the repository

- Take a snapshot of the current state of the project

`git commit -m "Initial commit"`

- If we have been working only on tracked files we can commit them directly (without adding them in the previous step):

`git commit -am "Commit changes on tracked files"`

- Fix title of the latest commit:

`git commit --amend -m "fixed commit title"`

(*) without `-m` would enter in interactive `vi` mode

- Undo latest commit maintaining file changes

`git reset --soft HEAD^`

(*) we could undo previous commits using `HEAD^2|3|4...` or the desired commit hash instead of `HEAD^`:

`git reset --soft 6bff2f0`

- Undo commit **and untrack files** maintaining file changes

`git reset --mixed 6bff2f0`

(*) `--mixed` is the default option for `reset`

- Undo commit and file changes

`git reset --hard 6bff2f0`

- **Recover reseted commits**

Check logs: `git reflog`

```bash
luismi@MacBook-Air-de-Luis 04-material-heroes % git reflog
311c8f3 (HEAD -> main) HEAD@{0}: reset: moving to HEAD^
0685467 HEAD@{1}: reset: moving to 0685467
0685467 HEAD@{2}: reset: moving to 0685467
0685467 HEAD@{3}: reset: moving to 0685467
b48bb0b HEAD@{4}: commit: GL & Robin added
6bff2f0 HEAD@{5}: reset: moving to 6bff2f0
806e26c HEAD@{6}: commit: GL added
6bff2f0 HEAD@{7}: commit (amend): historia added
b595331 HEAD@{8}: commit: historia added
cb0a715 HEAD@{9}: commit: ciudades added
0685467 HEAD@{10}: commit: heroes added
311c8f3 (HEAD -> main) HEAD@{11}: commit: misiones added
57533c0 HEAD@{12}: commit (initial): readme added
```

Reset to the desired commit: `git reset --hard b48bb0b`


## Logs

`git log`

``` bash
luismi@MacBook-Air-de-Luis GIT % git log
commit 09edb8ffce84007cf21069bb8c65b178058dc302 (HEAD -> main)
Author: Luismi Sánchez
Date:   Fri Dec 24 08:29:16 2021 +0100

    readme updated

commit 8092029c073575f45c7bba604566a5db893377c3
Author: Luismi Sánchez
Date:   Fri Dec 24 08:07:56 2021 +0100

    First main commit
:
```

## Notes about CRLF

Sometimes git warns about line endings on different OS (maybe a partner is *still working on Windows*). To avoid this messages:

`git config core.autocrlf true`

## Restoring project to previous commit

`git checkout -- .`

## Branches

> ALWAYS WORK IN BRANCHES

Show current branch and commit:

`git branch -av`

Rename branch:

`git branch -m master main`

## Aliases examples

Git status short

`git config --global alias.s "status -sb"`

``` bash
luismi@MacBook-Air-de-Luis GIT % git s
## main
 M readme.md
 ```

 Git log oneline decorated

 `git config --global alias.lg "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"`

``` bash
luismi@MacBook-Air-de-Luis GIT % git lg
* 547072a - (hace 15 minutos) updated readme and project files - Luismi Sánchez (HEAD -> main)
* 1187e31 - (hace 15 minutos) added uploads folder - Luismi Sánchez
* 73eb00c - (hace 30 minutos) readme updated from VSCode - Luismi Sánchez
* 09edb8f - (hace 40 minutos) readme updated - Luismi Sánchez
* 995341f - (hace 47 minutos) readme updated - Luismi Sánchez
* 8092029 - (hace 61 minutos) First main commit - Luismi Sánchez
```

Useful aliases: https://opensource.com/article/20/11/git-aliases

## Check differences

`git diff` or `git diff --staged` (to see changes on prefiosly staged --added-- files).

## Working with filesystem

> Using these method we prevent losing history of moved/renamed/deleted files

- Rename/Move files

`git mv destruirmundo.md salvar-mundo.md`

- Delete files

`git rm salvar-mundo.md`

## Ignoring files

A `.gitignore` file should exist on the project root and be commited. This file will include paths or wildcards to be ignored by git (IDE configurations, node_modules, etc).

```bash
luismi@MacBook-Air-de-Luis 04-material-heroes % cat .gitignore 
/.idea
/dist
/node_modules
*.log
/uploads/*.log
```

