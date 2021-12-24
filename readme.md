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

```
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

## Logs

`git log`

```
luismi@MacBook-Air-de-Luis 01-bases % git log
commit 24701eb52636ce4b2251dacd6a343b00de0609a7 (HEAD -> master)
Author: Luismi Sánchez
Date:   Fri Dec 24 07:12:38 2021 +0100

    First commit
```

## Notes about CRLF

Sometimes git warns about line endings on different OS (maybe a partner is *still working on Windows*). To avoid this messages:

`git config core.autocrlf true`

## Restoring project to previous commit

`git checkout -- .`