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

Ми продовжимо нашу вигадану історію, у якій Альфредо досліджує, чи можливо відправити планетарний посадковий модуль на Марс.

Перш за все, створімо у каталозі `Desktop` новий підкаталог для нашої роботи, а потім змінимо поточний робочий каталог на новостворений:

```bash
$ cd ~/Desktop
$ mkdir recipes
$ cd recipes
```

Далі ми кажемо Git зробити `recipes` [репозиторієм](../learners/reference.md#repository)
\-- місцем, де Git може зберігати версії наших файлів:

```bash
$ git init
```

It is important to note that `git init` will create a repository that
can include subdirectories and their files---there is no need to create
separate repositories nested within the `recipes` repository, whether
subdirectories are present from the beginning or added later. Крім того, зверніть увагу, що створення каталогу `recipes` та його ініціалізація як репозиторію є повністю окремими процесами.

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

Git використовує цей спеціальний підкаталог для зберігання всієї інформації про проєкт, у тому числі відстежувані файли і підкаталоги, що розташовані в каталозі проєкту.
Якщо ми коли-небудь видалимо підкаталог `.git`, то ми втратимо усю історію проєкту.

Тепер ми можемо почати використовувати одну з найважливіших команд git, яка особливо корисна для початківців. `git status` друкує інформацію про поточний стан нашого проєкту, та перелік зроблених змін у проєкті з варіантами щодо подальших дій. Ми можемо використовувати цю команду необмежену кількість разів, як тільки ми хочемо зрозуміти, що відбувається.

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Якщо ви користуєтеся іншою версією `git`, вигляд результату цієї команди може дещо відрізнятися.

:::::::::::::::::::::::::::::::::::::::  challenge

## Де створювати репозиторії Git

Along with tracking information about recipes (the project we have already created),
Alfredo would also like to track information about desserts specifically.
Alfredo creates a `desserts` project inside his `recipes`
project with the following sequence of commands:

```bash
$ cd ~/Desktop   # повернутися до каталогу Desktop
$ cd recipes     # перейти до каталогу recipes, який вже є репозиторієм Git
$ ls -a          # перевірити, що підкаталог .git все ще присутній у каталозі recipes
$ mkdir desserts # створити підкаталог recipes/desserts
$ cd desserts    # перейти у підкаталог desserts
$ git init       # створити репозиторій Git у підкаталозі desserts
$ ls -a          # перевірити наявність підкаталогу .git, що підтверджує створення нового репозиторію
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

## Відповідь (ВИКОРИСТОВУЙТЕ ОБЕРЕЖНО!)

### Контекст

Видаляти файли з репозиторію Git треба обережно. Проте, ми ще не навчилися вказувати Git як відстежувати певний файл; про це ми дізнаємося в наступному епізоді. Файли, які не відстежуються Git, можна легко видалити, як і будь-які інші "звичайні" файли:

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
Загалом, видалення файлів і каталогів за допомогою `rm` з командного рядка не можна скасувати.
Тому завжди перевіряйте свій поточний каталог за допомогою команди `pwd`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git init` ініціалізує репозиторій.
- Git зберігає всі дані репозиторію в каталозі `.git`.

::::::::::::::::::::::::::::::::::::::::::::::::::
