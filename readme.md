# Git Notes

## Global setup

`git config --global user.name "Luismi Sánchez"`

`git config --global user.email "my@email.com"`

Check global config per user:

`git config --global -e`

Global config is opened with vi, so we can modify (a) and save changes (Esc :wq!)

# Init local repository

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

# Repository status

Report the status of the followed files and commits:

`git status`

# Track (add) files to the repository

- Add files to track changes:

`git add filename1 filename2` or `git add wildcards` (e.g. `git add .`)

- Track a whole folder:

`git add folderPath\`

(*) In order to track an empty directory to the project (e.g. an empty `uploads` folder) we should create an empty file inside it named as `.gitkeep`

# Untrack files

`git reset filename` (or wildcard)

# Working with commits

## Take a snapshot (commit) of the current state of the project

`git commit -m "Initial commit"`

- If we have been working only on tracked files we can commit them directly (without adding them in the previous step):

`git commit -am "Commit changes on tracked files"`

- Fix title of the latest commit:

`git commit --amend -m "fixed commit title"`

(*) without `-m` would enter in interactive `vi` mode

## Undo latest commit maintaining file changes

`git reset --soft HEAD^`

(*) we could undo previous commits using `HEAD^2|3|4...` or the desired commit hash instead of `HEAD^`:

`git reset --soft 6bff2f0`

## Undo commit **and untrack files** maintaining file changes

`git reset --mixed 6bff2f0`

(*) `--mixed` is the default option for `reset`

## Undo commit and file changes

`git reset --hard 6bff2f0`

## Recover reset commits

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

## Restoring project to previous commit

`git checkout -- .`

## Restoring a single file to the previous commit:

`git checkout -- file-name`

# Logs

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

# Notes about CRLF

Sometimes git warns about line endings on different OS (maybe a partner is *still working on Windows*). To avoid this messages:

`git config core.autocrlf true`

# Aliases

## Git status short

`git config --global alias.s "status -sb"`

``` bash
luismi@MacBook-Air-de-Luis GIT % git s
## main
 M readme.md
 ```

 ## Git log one line decorated

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

# Check differences

`git diff` or `git diff --staged` (to see changes on previously staged --added-- files).

# Working with filesystem

> Using this method prevent losing history of moved/renamed/deleted files

- Rename/Move files

`git mv destruirmundo.md salvar-mundo.md`

- Delete files

`git rm salvar-mundo.md`

# Ignoring files

A `.gitignore` file should exist on the project root and be committed. This file will include paths or wildcards to be ignored by git (IDE configurations, node_modules, passwords, etc).

```bash
luismi@MacBook-Air-de-Luis 04-material-heroes % cat .gitignore 
/.idea
/dist
/node_modules
*.log
/uploads/*.log
```

To ignore a file from console we could

`printf "\n/path" >> .gitignore`

(*) Note the `\n` to append the new path/wildcard to a new line.

# Branches

> ALWAYS WORK IN BRANCHES (DIFFERENT COMMITS TIMELINES)

- Show current branch and commit:

`git branch -av`

- Rename branch:

`git branch -m old-name new-name`

- Create a new branch:

`git branch branch-name`

- Move to branch:

`git checkout branch-name`

- Create and move to the new branch:
  
`git checkout -b new-branch`

## Merges

When doing merges we must be on the target branch, so to merge new-branch to main branch we should:

``` bash
git checkout main
git merge new-branch

luismi@MacBook-Air-de-Luis demo-06 % git merge rama-villanos 
Actualizando 62c8a10..eadbf79
Fast-forward <-- Merge type detected and executed by git. See bellow descriptions.
 villanos.md | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 villanos.md
```

If there are no conflicts, git will add the commits of new-branch to main, moving `HEAD` (*) to the latest commit. 

Previous state:

``` bash
luismi@MacBook-Air-de-Luis demo-06 % git lg             
* eadbf79 - (hace 45 segundos) Villanos.md: añadido lolo - Luismi Sánchez (rama-villanos)
* d3e6593 - (hace 2 minutos) added villanos - Luismi Sánchez
* 62c8a10 - (4 años, y 6 meses atrás) Agregando el gitignore - Strider (HEAD -> master)
* ac0d374 - (4 años, y 6 meses atrás) Borramos la historia de batman - Strider
* b4c748c - (4 años, y 6 meses atrás) Cambiamos el nombre de la historia de superman - Strider
```

After the merge:

``` bash
luismi@MacBook-Air-de-Luis demo-06 % git lg    
* eadbf79 - (hace 10 minutos) Villanos.md: añadido lolo - Luismi Sánchez (HEAD -> main, rama-villanos)
* d3e6593 - (hace 11 minutos) added villanos - Luismi Sánchez
* 62c8a10 - (4 años, y 6 meses atrás) Agregando el gitignore - Strider
* ac0d374 - (4 años, y 6 meses atrás) Borramos la historia de batman - Strider
* b4c748c - (4 años, y 6 meses atrás) Cambiamos el nombre de la historia de superman - Strider
```
- Usually after a merge we **delete the new branch** if no needed anymore:

`git branch -d new-branch`

(*) Adding `-f` parameter forces the deletion without checking if there are not committed changes on the branch.

## Merge strategies

- **Fast-forward**: git detects there is no changes on the target branch (usually main/master). Commits of the new branch are stored on target as we have been working directly on this target branch.
- **Automatic unions (ort/recursive strategy)**: git detects changes on the target branch not included on the new branch and git **DO NOT DETECT CONFLICTS** (e.g. We are working on new-branch, commit changes in one file, then switch to main branch and commit changes on a different file):
  ```
  luismi@MacBook-Air-de-Luis demo-06 % git lg                                        
    * b9e1831 - (hace 27 segundos) borramos lolo - Luismi Sánchez (HEAD -> main)
    | * c9591fb - (hace 76 segundos) añadido notasssss en villanos - Luismi Sánchez (rama-villanos)
    | * 3402088 - (hace 3 minutos) añadido notas en villanos - Luismi Sánchez
    |/  <- Here we have a branch bifurcation
    * eadbf79 - (hace 10 horas) Villanos.md: añadido lolo - Luismi Sánchez
    * d3e6593 - (hace 10 horas) added villanos - Luismi Sánchez
    * 62c8a10 - (4 años, y 6 meses atrás) Agregando el gitignore - Strider
    ```
    In this case git will merge branches on the target adding the commits from the new branch and creating a new commit for this merge:
    ```
    git merge new-branch

    Merge branch 'new-branch' <- Merge commit message (editable)
    # Por favor ingresa un mensaje de commit que explique por qué es necesaria esta fusión,
    # especialmente si esto fusiona un upstream actualizado en una rama de tópico.
    #
    # Líneas comenzando con '#' serán ignoradas, y un mensaje vacío aborta
    # el commit.
    ```

    To save message and exit (vi) -> `ESC :wq! ENTER`:

    ```
    luismi@MacBook-Air-de-Luis demo-06 % git merge rama-villanos 
    Merge made by the 'ort' strategy.
    villanos.md | 4 +---
    1 file changed, 1 insertion(+), 3 deletions(-)
    ```

- **Handmade unions**: There are code conflicts between new and target branches. Git will ask to correct the conflict and create a new *merge commit* (including this correction). This happens when we modify and commit the same file on different branches:

    ```
    luismi@MacBook-Air-de-Luis demo-06 % git merge conflicto                             
    Auto-fusionando misiones.md
    CONFLICTO (contenido): Conflicto de fusión en misiones.md
    Fusión automática falló; arregle los conflictos y luego realice un commit con el resultado.
    ```

    Git will add conflict notes to the file:

    ```
    luismi@MacBook-Air-de-Luis demo-06 % cat misiones.md 
    # Misiones

    <<<<<<< HEAD
    1. Acabar con el plan de Lex Luthor.
    2. Crear la liga de la justicia.
    3. Buscar nuevos miembros antiheroes.
    4. Buscar comida.
    =======
    1. Acabar con el plan de Lex Luthor
    2. Crear la liga de la justicia
    3. Buscar nuevos miembros superheroes
    4. 
    >>>>>>> conflicto
    ```

    After fixing the conflict (manually in the editor/IDE) we should commit the fixed file on the target branch:

    `git commit -am file-name`

    (*) Files without conflict will merge normally.

# Tags

- Use `tags` to name/number releases:

`git tag -a "1.0.0" -m "Release version 1.0.0"`

- Check existing tags:

`git tag`

- Remove a tag:

`git tag -d "1.0.0"`

- Add a tag to a past commit:

`git tag -a "alpha-1.0.0" commitHash -m "Alpha version 1.0.0"`

- Extended info of a tag:

`git show "1.0.0"`
```
tag 1.0.0
Tagger: Luismi Sánchez
Date:   Wed Dec 29 20:12:45 2021 +0100

Release version 1.0.0

commit 9a387e66dd72188bd79f052888d79b823cc4be3d (HEAD -> main, tag: 1.0.0)
Merge: 22c78a1 c0c2c1e
...
```

# Stash

Git Stash save actual changes (TEMPORARILY as **Work In Progress**) and returns repository to last `HEAD` __in the branch we are working__. This way you can work with your latest commit without losing changes made after it. Then you can recover changes from the Stash and continue working on them:

```
luismi@MacBook-Air-de-Luis 07-demo-stash % git stash save "Changes made to stash"
Directorio de trabajo y estado de índice On master: Changes made to stash guardados

luismi@MacBook-Air-de-Luis 07-demo-stash % git lg
*   3587dde - (hace 71 segundos) On master: Changes made to stash - Luismi Sánchez (refs/stash)
|\
| * f25d401 - (hace 71 segundos) index on master: 5440fe5 Resolviendo conflictos - Luismi Sánchez
|/
*   5440fe5 - (4 años, y 6 meses atrás) Resolviendo conflictos - Strider (HEAD -> master, tag: v1.0.0) <- After the Stash we are HERE.
|\
| * 52c9666 - (4 años, y 6 meses atrás) Modificamos las misiones - Strider
...
```

- See stashed references (list stashes)

```
luismi@MacBook-Air-de-Luis 07-demo-stash % git stash list --stat
stash@{0}: On master: Changes made to stash

 README.md | 5 +----
 1 file changed, 1 insertion(+), 4 deletions(-)
stash@{1}: WIP on master: 78fb744 README.md: merged with stash

 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

**RECOMMENDED**: Work only with one Stash if needed. Preferibly work with branches.

After commiting we will see the Stash under our latest commits:

```
luismi@MacBook-Air-de-Luis 07-demo-stash % git lg
* 702f170 - (hace 3 segundos) README.md: Changed to Notas - Luismi Sánchez (HEAD -> master) <- Our latest commit
| * 3587dde - (hace 9 minutos) WIP on master: 5440fe5 Resolviendo conflictos - Luismi Sánchez (refs/stash) <- Changes stashed
|/|
| * f25d401 - (hace 9 minutos) index on master: 5440fe5 Resolviendo conflictos - Luismi Sánchez
|/
*   5440fe5 - (4 años, y 6 meses atrás) Resolviendo conflictos - Strider (tag: v1.0.0)
```

- Recover latest stash changes:  `git stash pop`

    - If there are no conflicts we will see our stashed changes ready to be commited:

    ```
    luismi@MacBook-Air-de-Luis 07-demo-stash % git stash pop
    En la rama master
    Cambios no rastreados para el commit:
    (usa "git add <archivo>..." para actualizar lo que será confirmado)
    (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
        modificados:     misiones.md

    sin cambios agregados al commit (usa "git add" y/o "git commit -a")
    Descartado refs/stash@{0} (3587ddec9fadec94f40acb39b99a99a6670ae786)
    ```

    - If git encounter changes on the same file it will try to **Auto-merge** them:
    
    ```
    luismi@MacBook-Air-de-Luis 07-demo-stash % git stash pop
    Auto-fusionando README.md <- Auto merge (e.g: changes on different lines).
    En la rama master
    Cambios no rastreados para el commit:
    (usa "git add <archivo>..." para actualizar lo que será confirmado)
    (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
        modificados:     README.md

    sin cambios agregados al commit (usa "git add" y/o "git commit -a")
    Descartado refs/stash@{0} (ad937bd4124b9fc6cbe167608e283db427a359ad)
    ```

    - If there are conflicts git couldn't manage it will behave the same way as with merge branches conflicts (you must fix conflicst by hand). The stashed changes remains until conflict fixed:

    ```
    luismi@MacBook-Air-de-Luis 07-demo-stash % git stash pop
    Auto-fusionando README.md
    CONFLICTO (contenido): Conflicto de fusión en README.md
    La entrada de stash se guardó en caso de ser necesario nuevamente.


    luismi@MacBook-Air-de-Luis 07-demo-stash % cat README.md
    <<<<<<< Updated upstream
    # NOTAS. Second conflict changes. 4th conflict.
    =======
    # NOTAS. Second conflict changes. Third conflict.
    >>>>>>> Stashed changes

    Este repositorio sirve para probar cosas

    Changes to create stash conflict.


    luismi@MacBook-Air-de-Luis 07-demo-stash % git lg
    * 971784d - (hace 6 minutos) README.md: conflict in the same line with Stash - Luismi Sánchez (HEAD -> master)
    | * c925f34 - (hace 7 minutos) WIP on master: 78fb744 README.md: merged with stash - Luismi Sánchez (refs/stash)
    |/|
    | * 23e1f64 - (hace 7 minutos) index on master: 78fb744 README.md: merged with stash - Luismi Sánchez
    |/
    * 78fb744 - (hace 8 minutos) README.md: merged with stash - Luismi Sánchez
    ```   
    
    
    If there were no conflicts, the stash is discarded and changes made out of the stash *merged*:

    ```
    luismi@MacBook-Air-de-Luis 07-demo-stash % git lg
    * 702f170 - (hace 5 minutos) README.md: Changed to Notas - Luismi Sánchez (HEAD -> master)
    *   5440fe5 - (4 años, y 6 meses atrás) Resolviendo conflictos - Strider (tag: v1.0.0)
    ```

    If there were conflics, stash remains after fixing conflicts:

    ```
    luismi@MacBook-Air-de-Luis 07-demo-stash % git lg
    * d5a0b6b - (hace 10 segundos) README.md: conflict fixed - Luismi Sánchez (HEAD -> master)
    * 971784d - (hace 10 minutos) README.md: conflict in the same line with Stash - Luismi Sánchez
    | * c925f34 - (hace 11 minutos) WIP on master: 78fb744 README.md: merged with stash - Luismi Sánchez (refs/stash)
    |/|
    | * 23e1f64 - (hace 11 minutos) index on master: 78fb744 README.md: merged with stash - Luismi Sánchez
    |/
    * 78fb744 - (hace 12 minutos) README.md: merged with stash - Luismi Sánchez
    ```

    At this point we should finish the work and commit changes recovered from the stash. 

- Remove all stashes. Recommended after each stash work: `git stash clear` 

    (*) You could recover after removing using `reflog`. (See above Recover reset commit method)

- Apply stash (not the latest with `pop`): 

```
luismi@MacBook-Air-de-Luis 07-demo-stash % git stash list
stash@{0}: On master: Changes made to stash
stash@{1}: WIP on master: 78fb744 README.md: merged with stash

luismi@MacBook-Air-de-Luis 07-demo-stash % git stash apply stash@{1}
Auto-fusionando README.md
```

- Remove single stash:

```
luismi@MacBook-Air-de-Luis 07-demo-stash % git stash drop stash@{1}
Descartado stash@{1} (c925f34d98b25a8e1137eb03765c3fafa79e8d9b)
```

- Show info about a stash:

```
luismi@MacBook-Air-de-Luis 07-demo-stash % git stash show stash@{1}
 README.md | 5 +----
 1 file changed, 1 insertion(+), 4 deletions(-)
```



# Rebase

Rebase allow us to reorganize the work done. It's recommended not to change git history, so rebase should only be used before we deploy or push our code. From the git reference book: **Do not rebase commits that exist outside your repository and that people may have based work on.**

It allows to move the branch pointers, squash (fusion) commits, rename, edit, etc. We also should rebase if there ara conflicts pushing/pulling from remote repositories (see bellow).

https://git-scm.com/docs/git-rebase

https://git-scm.com/book/en/v2/Git-Branching-Rebasing (https://git-scm.com/book/es/v2/Ramificaciones-en-Git-Reorganizar-el-Trabajo-Realizado)

## Common Rebase: 
E.g.: There are commits applied on main branch after the commits on our working branch. Our working branch is not merged yet with main and we need main commits to be included in our working branch. This will have a similar effect than merging our working branch from main:

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git lg
    * 158ba9e - (4 años, y 6 meses atrás) Se agrego a la liga: Volcán Negro - Strider (HEAD -> master)
    * 300c014 - (4 años, y 6 meses atrás) Misiones nuevas agregadas - Strider
    | * 8e755a3 - (4 años, y 6 meses atrás) Actualizamos dos misiones completadas al momento - Strider (rama-misiones-completadas)
    | * cc55aaf - (4 años, y 6 meses atrás) Agregamos el archivo de las misiones completadas - Strider
    |/
    * acea380 - (4 años, y 6 meses atrás) Actualización de las misiones - Strider

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git checkout rama-misiones-completadas
    Cambiado a rama 'rama-misiones-completadas'

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git rebase master
    Rebase aplicado satisfactoriamente y actualizado refs/heads/rama-misiones-completadas.

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git lg
    * 6bb9edb - (4 años, y 6 meses atrás) Actualizamos dos misiones completadas al momento - Strider (HEAD -> rama-misiones-completadas) <- HEAD is moved to our working branch, including main commits
    * 55ae59a - (4 años, y 6 meses atrás) Agregamos el archivo de las misiones completadas - Strider
    * 158ba9e - (4 años, y 6 meses atrás) Se agrego a la liga: Volcán Negro - Strider (master)
    * 300c014 - (4 años, y 6 meses atrás) Misiones nuevas agregadas - Strider
    * acea380 - (4 años, y 6 meses atrás) Actualización de las misiones - Strider

Now we could fast-forward merge branches:

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git checkout master
    Cambiado a rama 'master'

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git merge rama-misiones-completadas
    Actualizando 158ba9e..6bb9edb
    Fast-forward
    misiones-completadas.md | 4 ++++
    1 file changed, 4 insertions(+)
    create mode 100644 misiones-completadas.md
    
    luismi@MacBook-Air-de-Luis 08-demo-rebase % git lg
    * 6bb9edb - (4 años, y 6 meses atrás) Actualizamos dos misiones completadas al momento - Strider (HEAD -> master, rama-misiones-completadas)
    * 55ae59a - (4 años, y 6 meses atrás) Agregamos el archivo de las misiones completadas - Strider
    * 158ba9e - (4 años, y 6 meses atrás) Se agrego a la liga: Volcán Negro - Strider
    * 300c014 - (4 años, y 6 meses atrás) Misiones nuevas agregadas - Strider
    * acea380 - (4 años, y 6 meses atrás) Actualización de las misiones - Strider

## Rebase Squash
Sometimes we need fusion commits, e.g.: we commit changes and then realize these changes are related to the previous commit.


    luismi@MacBook-Air-de-Luis 08-demo-rebase % git rebase -i HEAD~4

    pick acea380 Actualización de las misiones
    pick 300c014 Misiones nuevas agregadas
    pick 158ba9e Se agrego a la liga: Volcán Negro
    s bb0f045 Agregamos y actualizamos el archivo de las misiones completadas <- Squash this commit to the previous one. We are in -i (interactive) vi mode, so ESC :wq! INTRO to write and quit.

    # Rebase 31efae8..bb0f045 en 31efae8 (4 comandos)
    #
    # Comandos:
    # p, pick <commit> = usar commit
    # r, reword <commit> = usar commit, pero editar el mensaje de commit
    # e, edit <commit> = usar commit, pero parar para un amend
    # s, squash <commit> = usar commit, pero fusionarlo en el commit previo
    ...

Next step: new squashed commit message (ESC :wq! INTRO to write and quit):

    # Esta es una combinación de 2 commits.
    # Este es el mensaje del 1er commit:

    New message

    # Este es el mensaje del commit #2:

    # Agregamos ectualizamos el archivo de las misiones completadas

    # Por favor ingresa el mensaje del commit para tus cambios. Las
    #  líneas que comiencen con '#' serán ignoradas, y un mensaje
    #  vacío aborta el commit.

Git will show the rebase message at completion:

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git rebase -i HEAD~4
    [HEAD desacoplado a844504] New message
    Date: Fri Jun 23 15:47:22 2017 -0600
    2 files changed, 5 insertions(+)
    create mode 100644 misiones-completadas.md
    Rebase aplicado satisfactoriamente y actualizado refs/heads/master.
 
    luismi@MacBook-Air-de-Luis 08-demo-rebase % git lg
    * a844504 - (4 años, y 6 meses atrás) New message - Strider (HEAD -> master) <- Rebased commit
    * 300c014 - (4 años, y 6 meses atrás) Misiones nuevas agregadas - Strider
    * acea380 - (4 años, y 6 meses atrás) Actualización de las misiones - Strider
    * 31efae8 - (4 años, y 6 meses atrás) Retomando el trabajo que guarde en el stash - Strider
    * 8239e26 - (4 años, y 6 meses atrás) Cambios de emergencia en el README - Strider

## - Rebase Reword

Allows change commit messages.

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git lg
    * a844504 - (4 años, y 6 meses atrás) New message - Strider (HEAD -> master)
    * 300c014 - (4 años, y 6 meses atrás) Misiones nuevas agregadas - Strider
    * acea380 - (4 años, y 6 meses atrás) Actualización de las misiones - Strider
    * 31efae8 - (4 años, y 6 meses atrás) Retomando el trabajo que guarde en el stash - Strider

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git rebase -i HEAD~4

It will enter on interactive modo, letting us change the commit messages for each commit marked with r(reword):

    pick 31efae8 Retomando el trabajo que guarde en el stash
    pick acea380 Actualización de las misiones
    r 300c014 Misiones nuevas agregadas
    r a844504 New message

    # Rebase 8239e26..a844504 en 8239e26 (4 comandos)
    #
    # Comandos:
    # p, pick <commit> = usar commit
    # r, reword <commit> = usar commit, pero editar el mensaje de commit

Then we could change each commit message (ESV :wq! INTRO) to write and quit to the next:

    Reword: Misiones nuevas agregadas

    # Por favor ingresa el mensaje del commit para tus cambios. Las
    #  líneas que comiencen con '#' serán ignoradas, y un mensaje
    #  vacío aborta el commit.

Finally git shows what happened:

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git rebase -i HEAD~4
    [HEAD desacoplado 1df102e] Reword: Misiones nuevas agregadas
    Author: Strider 
    Date: Fri Jun 23 15:47:03 2017 -0600
    1 file changed, 1 insertion(+)
    [HEAD desacoplado f0814f4] Reword 2: New message
    Author: Strider 
    Date: Fri Jun 23 15:47:22 2017 -0600
    2 files changed, 5 insertions(+)
    create mode 100644 misiones-completadas.md
    Rebase aplicado satisfactoriamente y actualizado refs/heads/master.

    luismi@MacBook-Air-de-Luis 08-demo-rebase % git lg
    * f0814f4 - (4 años, y 6 meses atrás) Reword 2: New message - Strider (HEAD -> master)
    * 1df102e - (4 años, y 6 meses atrás) Reword: Misiones nuevas agregadas - Strider
    * acea380 - (4 años, y 6 meses atrás) Actualización de las misiones - Strider

# Working with GitHub

## Caching GitHub credentials: 
https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git

# See repository remotes

    git remote -v

## Create a new repository on the command line
    echo "# test" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git branch -M main
    git remote add origin https://github.com/luismisanchez/test.git
    git push -u origin main

## Push an existing repository from the command line
    git remote add origin https://github.com/luismisanchez/test.git
    git branch -M main
    git push -u origin main

## Pushing local tags

    git push --tags

## Pulling remote repositories

If we pull from repositories without setting a strategy git will fail warning about it:

    luismi@MacBook-Air-de-Luis GIT % git pull
    remote: Enumerating objects: 5, done.
    remote: Counting objects: 100% (5/5), done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
    Desempaquetando objetos: 100% (3/3), 679 bytes | 8.00 KiB/s, listo.
    Desde https://github.com/luismisanchez/test
    09b3dc0..6a9c982  main       -> origin/main
    ayuda: Hacer un pull sin especificar cómo reconciliar las ramas es poco
    ayuda: recomendable. Puedes eliminar este mensaje usando uno de los
    ayuda: siguientes comandos antes de tu siguiente pull:
    ayuda:
    ayuda:   git config pull.rebase false  # hacer merge (estrategia por defecto)
    ayuda:   git config pull.rebase true   # aplicar rebase
    ayuda:   git config pull.ff only       # aplicar solo fast-forward
    ayuda:
    ayuda: Puedes reemplazar "git config" con "git config --global" para aplicar
    ayuda: la preferencia en todos los repositorios. Puedes también pasar --rebase,
    ayuda: --no-rebase, o --ff-only en el comando para sobrescribir la configuración
    ayuda: por defecto en cada invocación.
    fatal: Necesita especificar cómo reconciliar las ramas divergentes.

## Allow only Fast-forward pulls (set globaly):

    git config --global pull.ff only

## Allow rebase pulls (set globaly). RECOMMENDED OPTION:

    git config --global pull.rebase true

This way git allows to rebase (see above) if conflicts found in origin on pull.

## Fixing conflicts with origin on push/pull:

    luismi@MacBook-Air-de-Luis GIT % git push
    To https://github.com/luismisanchez/test.git
    ! [rejected]        main -> main (fetch first)
    error: falló el push de algunas referencias a 'https://github.com/luismisanchez/test.git'
    ayuda: Actualizaciones fueron rechazadas porque el remoto contiene trabajo que
    ayuda: no existe localmente. Esto es causado usualmente por otro repositorio
    ayuda: realizando push a la misma ref. Quizás quieras integrar primero los cambios
    ayuda: remotos (ej. 'git pull ...') antes de volver a hacer push.
    ayuda: Mira 'Notes about fast-forwards' en 'git push --help' para detalles.

So we try a pull as noted by git warning, but:

    luismi@MacBook-Air-de-Luis GIT % git pull
    remote: Enumerating objects: 5, done.
    remote: Counting objects: 100% (5/5), done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
    Desempaquetando objetos: 100% (3/3), 677 bytes | 8.00 KiB/s, listo.
    Desde https://github.com/luismisanchez/test
    6dc4645..7d05d65  main       -> origin/main
    Auto-fusionando readme.md
    CONFLICTO (contenido): Conflicto de fusión en readme.md
    error: no se pudo aplicar abbe42a... readme updated
    ayuda: Resolve all conflicts manually, mark them as resolved with
    ayuda: "git add/rm <conflicted_files>", then run "git rebase --continue".
    ayuda: You can instead skip this commit: run "git rebase --skip".
    ayuda: To abort and get back to the state before "git rebase", run "git rebase --abort".
    No se pudo aplicar abbe42a... readme updated

After fixing the conflicts by hand in the editor/IDE we should:


    luismi@MacBook-Air-de-Luis GIT % git add readme.md

    luismi@MacBook-Air-de-Luis GIT % git rebase --continue

    fixed rebased readme

    # Por favor ingresa el mensaje del commit para tus cambios. Las
    #  líneas que comiencen con '#' serán ignoradas, y un mensaje
    #  vacío aborta el commit.
    #
    # rebase interactivo en progreso; sobre 7d05d65
    # El último comando realizado (1 comando realizado):
    #    pick abbe42a readme updated
    # No quedan más comandos.
    # Estás aplicando un rebase de la rama 'main' sobre '7d05d65.
    #
    # Cambios a ser confirmados:
    #       modificados:     readme.md
    #

    luismi@MacBook-Air-de-Luis GIT % git rebase --continue
    [HEAD desacoplado cecf0ba] fixed rebased readme
    1 file changed, 25 insertions(+), 1 deletion(-)
    Rebase aplicado satisfactoriamente y actualizado refs/heads/main.

Now we could push:

    luismi@MacBook-Air-de-Luis GIT % git push
    Enumerando objetos: 5, listo.
    Contando objetos: 100% (5/5), listo.
    Compresión delta usando hasta 4 hilos
    Comprimiendo objetos: 100% (3/3), listo.
    Escribiendo objetos: 100% (3/3), 656 bytes | 656.00 KiB/s, listo.
    Total 3 (delta 2), reusados 0 (delta 0), pack-reusados 0
    remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
    To https://github.com/luismisanchez/test.git
    7d05d65..cecf0ba  main -> main