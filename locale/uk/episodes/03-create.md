---
title: Створення репозиторію
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Створити локальний репозиторій Git.
- Пояснити призначення каталогу `.git`.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Де Git зберігає інформацію?

::::::::::::::::::::::::::::::::::::::::::::::::::

Після налаштування Git ми можемо почати його використання.

We will help Alfredo with his new project, create a repository with all his recipes.

First, let's create a new directory in the `Desktop` folder for our work and then change the current working directory to the newly created one:

```bash
$ cd ~/Desktop
$ mkdir recipes
$ cd recipes
```

Then we tell Git to make `recipes` a [repository](../learners/reference.md#repository)
\-- a place where Git can store versions of our files:

```bash
$ git init
```

It is important to note that `git init` will create a repository that
can include subdirectories and their files---there is no need to create
separate repositories nested within the `recipes` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `recipes` directory and its initialization as a
repository are completely separate processes.

Якщо ми використаємо `ls`, щоб передивитись зміст каталогу, то здається, що нічого не змінилося:

```bash
$ ls
```

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `recipes` called `.git`:

```bash
$ ls -a
```

```output
.	..	.git
```

Git uses this special subdirectory to store all the information about the project,
including the tracked files and sub-directories located within the project's directory.
Якщо ми коли-небудь видалимо підкаталог `.git`, то ми втратимо усю історію проєкту.

Тепер ми можемо почати використовувати одну з найважливіших команд git, яка особливо корисна для початківців. `git status` tells us the status of our project, and better, a list of changes in the project and options on what to do with those changes. Ми можемо використовувати цю команду необмежену кількість разів, як тільки ми хочемо зрозуміти, що відбувається.

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

:::::::::::::::::::::::::::::::::::::::  challenge

## Де створювати репозиторії Git

Along with tracking information about recipes (the project we have already created),
Alfredo would also like to track information about desserts specifically.
Alfredo creates a `desserts` project inside his `recipes`
project with the following sequence of commands:

```bash
$ cd ~/Desktop    # return to Desktop directory
$ cd recipes      # go into recipes directory, which is already a Git repository
$ ls -a           # ensure the .git subdirectory is still present in the recipes directory
$ mkdir desserts # make a sub-directory recipes/desserts
$ cd desserts    # go into desserts subdirectory
$ git init        # make the desserts subdirectory a Git repository
$ ls -a           # ensure the .git subdirectory is present indicating we have created a new Git repository
```

Is the `git init` command, run inside the `desserts` subdirectory, required for
tracking files stored in the `desserts` subdirectory?

:::::::::::::::  solution

## Відповідь

Ні. Alfredo does not need to make the `desserts` subdirectory a Git repository
because the `recipes` repository will track all files, sub-directories, and
subdirectory files under the `recipes` directory.  Thus, in order to track
all information about desserts, Alfredo only needed to add the `desserts` subdirectory
to the `recipes` directory.

Крім того, репозиторії Git можуть заважати один одному, якщо вони "вкладені": зовнішній репозиторій намагатиметься відстежувати зміни у внутрішньому репозиторії. Тому найкраще кожного разу створювати новий репозиторій Git в окремому каталозі. Щоб переконатися, що каталог не є репозиторієм, перевірте результат команди `git status`. Якщо він виглядає як показано нижче, ви можете створити новий репозиторій, як було показано вище:

```bash
$ git status
```

```output
fatal: Not a git repository (or any of the parent directories): .git
```

:::::::::::::::::::::::::

## Виправлення помилок `git init`

Jimmy explains to Alfredo how a nested repository is redundant and may cause confusion
down the road. Alfredo would like to go back to a single git repository. How can Alfredo undo
his last `git init` in the `desserts` subdirectory?

:::::::::::::::  solution

## Solution -- USE WITH CAUTION!

### Контекст

Removing files from a Git repository needs to be done with caution. Проте, ми ще не навчилися вказувати Git як відстежувати певний файл; про це ми дізнаємося в наступному епізоді. Файли, які не відстежуються Git, можна легко видалити, як і будь-які інші "звичайні" файли:

```bash
$ rm filename
```

Подібним чином можна видалити каталог за допомогою команди `rm -r dirname`.
Якщо файли чи каталоги, які видаляються таким чином, вже відстежуються Git, тоді їх видалення стає ще однією зміною, яку нам потрібно буде відстежувати (ми побачимо як це робити в наступному епізоді).

### Відповідь

Git зберігає всі свої файли в каталозі `.git`.
To recover from this little mistake, Alfredo can remove the `.git`
folder in the desserts subdirectory by running the following command from inside the `recipes` directory:

```bash
$ rm -rf desserts/.git
```

Проте, будьте обережні! Виконання цієї команди у неправильному каталозі призведе до видалення всієї історії змін проєкту, яку ви хотіли б зберегти.
In general, deleting files and directories using `rm` from the command line cannot be reversed.
Тому завжди перевіряйте свій поточний каталог за допомогою команди `pwd`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git init` ініціалізує репозиторій.
- Git зберігає всі дані репозиторію в каталозі `.git`.

::::::::::::::::::::::::::::::::::::::::::::::::::
