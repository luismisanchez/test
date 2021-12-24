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

`git add filename1 filename2` or `git add regex` (e.g. `git add .`)

## Untrack files

`git reset filename` (or regex)

## Commit to the repository

Take a snapshot of the current state of the project

`git commit -m "Initial commit"`

If we have been working only on tracked files we can commit them directly (without adding them in the previous step):

`git commit -am "Commit changes on tracked files"`

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


