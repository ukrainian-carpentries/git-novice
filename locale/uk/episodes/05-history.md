---
title: Дослідження історії
teaching: 25
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Пояснення, що таке HEAD репозиторію та як його використовувати.
- Визначення та використання ідентифікаторів комітів Git.
- Порівняння різних версій відстежуваних файлів.
- Відновлення старих версій файлів.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Як визначити старі версії файлів?
- Як переглянути свої зміни?
- Як відновити старі версії файлів?

::::::::::::::::::::::::::::::::::::::::::::::::::

Як ми побачили у попередньому епізоді, ми можемо посилатися на коміти за їх ідентифікаторами.  Ви можете звернутися до _останнього коміту_ робочого каталогу
за допомогою ідентифікатора `HEAD`.

We've been adding small changes at a time to `guacamole.md`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `guacamole.md`, adding yet another line.

```bash
$ nano guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
An ill-considered change
```

Тепер подивимося на наш результат.

```bash
$ git diff HEAD guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index b36abfd..0848c8d 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -4,3 +4,4 @@
 * lime
 * salt
 ## Instructions
+An ill-considered change
```

Це те саме, що ви отримаєте, якщо пропустите `HEAD` (спробуйте це).\
Але справжня користь полягає у тому, що ви таким чином можете посилатися на попередні коміти.  Наприклад, додаючи `~1` (де "~" - це тільда), ми посилаємось на коміт зроблений безпосередньо перед `HEAD`.

```bash
$ git diff HEAD~1 guacamole.md
```

Якщо ми хочемо побачити різницю між старішими комітами, ми можемо знову використовувати `git diff`, використовуючи для послання на них `HEAD~1`, `HEAD~2` тощо:

```bash
$ git diff HEAD~2 guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..b36abfd 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,6 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lime
+* salt
 ## Instructions
```

Ми також можемо використати команду `git show`, яка показує які зміни ми внесли у будь-якому попередньо зробленому коміті, а також і повідомлення коміту (на відміну від команди `git diff` яка покаже _різницю_ між комітом та нашим робочим каталогом).

```bash
$ git show HEAD~2 guacamole.md
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Create a template for recipe

diff --git a/guacamole.md b/guacamole.md
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/guacamole.md
@@ -0,0 +1,3 @@
+# Guacamole
+## Ingredients
+## Instructions
```

Таким чином, ми можемо побудувати ланцюжок комітів.
Останній кінець ланцюжка називається `HEAD`;
ми можемо посилатися на попередні коміти, використовуючи нотацію `~`,
тому `HEAD~1`
означає "попередній коміт",
тоді як `HEAD~123` повертається на 123 коміти назад від того місця, де ми зараз знаходимось.

We can also refer to commits using
those long strings of digits and letters
that both `git log` and `git show` display.
Це унікальні ідентифікатори змін, де "унікальний" насправді означає унікальний: кожна зміна будь-якого набору файлів на будь-якому комп'ютері буде мати унікальний 40-символьний ідентифікатор.
Наш перший коміт отримав ідентифікатор `f22b25e3233b4645dabd0d81e651fe074bd8e73b`, тож спробуймо наступне:

```bash
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..93a3e13 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,7 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lime
+* salt
 ## Instructions
+An ill-considered change
```

Це правильна відповідь, проте, введення випадкових рядків з 40 символів дуже незручно. Тож Git дозволяє нам використовувати тільки перші кілька символів (як правило, сім для проєктів нормального розміру):

```bash
$ git diff f22b25e guacamole.md
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..93a3e13 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,7 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lime
+* salt
 ## Instructions
+An ill-considered change
```

Це чудово! Отже,
ми можемо зберегти зміни у файлах і побачити, що ми змінили. Наступне питання - а як ми можемо відновити їх старіші версії?
Let's suppose we change our mind about the last update to
`guacamole.md` (the "ill-considered change").

`git status` зараз каже нам, що файл був змінений, але ці зміни не були додані до зони стейджингу:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Ми можемо повернути цей файл до його попереднього стану за допомогою `git restore`:

```bash
$ git restore guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
```

As you might guess from its name,
`git restore` restores an old version of a file.
By default,
it recovers the version of the file recorded in `HEAD`,
which is the last saved commit.
Якщо ми хочемо повернутися ще раніше, замість цього ми можемо використовувати ідентифікатор коміту з опцією `-s`:

```bash
$ git restore -s f22b25e guacamole.md
```

```bash
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
## Instructions
```

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")

```

Notice that the changes are not currently in the staging area, and have not been committed.
If we wished, we can put things back the way they were at the last commit by using `git restore` to overwrite
the working copy with the last committed version:

```bash
$ git restore guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
```

Важливо пам'ятати, що ми повинні використовувати номер коміту, який ідентифікує стан репозиторію _до_ зміни, яку ми намагаємося скасувати.
Поширеною помилкою є використання номеру коміту, в якому ми зробили зміни, які намагаємося скасувати.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `f22b25e`. We use the `.` to mean all files:

![](fig/git-restore.svg){alt='A diagram showing how git restore can be used to restore the previous version of two files'}

Отже, якщо скласти це все разом, то Git працює як зображено у цьому коміксі:

![https://figshare.com/articles/How\_Git\_works\_a\_cartoon/1328266](fig/git_staging.svg){alt='A diagram showing the entire git workflow: local changes are staged using git add, applied to the local repository using git commit, and can be restored from the repository using git checkout'}

Той факт, що файли можна відновлювати окремо, сприяє змінам в організації роботи.
Якщо все знаходиться в одному величезному документі, буде важко (але не неможливо) скасувати зміни у вступі без скасування також змін, внесених пізніше до висновку.
З іншого боку, якщо вступ і висновок зберігаються в окремих файлах, рухатися вперед і назад в часі стає набагато легше.

:::::::::::::::::::::::::::::::::::::::  challenge

## Відновлення старих версій файлу

Дженніфер зробила цього ранку деякі зміни у скрипті Python, над яким вона до цього працювала тижнями, та "зламала" його - він більше не запускається. Вона витратила майже годину, намагаючись виправити його, але безрезультатно...

На щастя, вона використовує Git для відстеження змін у свому проєкті! Які з наведених нижче команд допоможуть їй відновити останню збережену у Git версію її скрипту, який називається `data_cruncher.py`?

1. `$ git restore`

2. `$ git restore data_cruncher.py`

3. `$ git restore -s HEAD~1 data_cruncher.py`

4. `$ git restore -s <unique ID of last commit> data_cruncher.py`

5. Як 2, так і 4

:::::::::::::::  solution

## Рішення

Відповідь (5) - як 2, так і 4.

The `restore` command restores files from the repository, overwriting the files in your working
directory. Обидві відповіді 2 та 4 відновлюють _останню збережену в репозиторії_ версію файлу `data_cruncher.py`. Відповідь 2 використовує `HEAD`, щоб вказати _останній_ коміт, тоді як відповідь 4 використовує унікальний ідентифікатор останнього коміту, що саме й означає `HEAD`.

Відповідь 3 замінить `data_cruncher.py` його версією з коміту _перед_ `HEAD`, що НЕ є тим, що ми хотіли.

Answer 1 results in an error. You need to specify a file to restore. If you want to restore all files
you should use `git restore .`

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Скасування коміту

Дженніфер співпрацює з колегами над її скриптом Python.  Вона зрозуміла, що її останній коміт до репозиторію проєкту містив помилку, і хоче його скасувати.  Дженніфер хоче скасувати його правильним чином, щоб всі користувачі репозиторію цього проєкту отримали правильні зміни. Команда `git revert [erroneous commit ID]` створить новий коміт, який скасує помилковий коміт.

The command `git revert` is
different from `git restore -s [commit ID] .` because `git restore` returns the
files not yet committed within the local repository to a previous state, whereas `git revert`
reverses changes committed to the local and project repositories.

Нижче наведені правильні кроки та пояснення для Дженніфер щодо користування `git revert`. Яка команда відсутня?

1. `________ # Подивіться на історію змін, щоб знайти ідентифікатор коміту`

2. Скопіюйте цей ID (перші кілька символів ID, наприклад 0b1d055).

3. `git revert [commit ID]`

4. Введіть повідомлення для нового коміту.

5. Save and close.

:::::::::::::::  solution

## Рішення

Команда `git log` зображує історію проєкту з ідентифікаторами комітів.

Команда `git show HEAD` покаже зміни, зроблені в останньому коміті, і покаже його ID; однак Дженніфер повинна перевірити, що це правильний коміт, а не нові зміни, які зробив у спільному репозиторії хтось інший.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Розуміння послідовності дій та історії

Який результат останньої команди в цій послідовності?

```bash
$ cd recipes
$ echo "I like tomatoes, therefore I like ketchup" > ketchup.md
$ git add ketchup.md
$ echo "ketchup enhances pasta dishes" >> ketchup.md
$ git commit -m "My opinions about the red sauce"
$ git restore ketchup.md
$ cat ketchup.md # this will print the content of ketchup.md on screen
```

1. ```output
   ketchup enhances pasta dishes
   ```
2. ```output
   I like tomatoes, therefore I like ketchup
   ```
3. ```output
   I like tomatoes, therefore I like ketchup
   ketchup enhances pasta dishes
   ```
4. ```output
   Error because you have changed ketchup.md without committing the changes
   ```

:::::::::::::::  solution

## Solution

The answer is 2.

The changes to the file from the second `echo` command are only applied to the working copy,
not the version in the staging area.
The command `git add ketchup.md` places the current version of `ketchup.md` into the staging area.

So, when `git commit -m "My opinions about the red sauce"` is executed,
the version of `ketchup.md` committed to the repository is the one from the staging area and
has only one line.

At this time, the working copy still has the second line (and

`git status` will show that the file is modified). However, `git restore ketchup.md`
replaces the working copy with the most recently committed version of `ketchup.md`.
So, `cat ketchup.md` will output

```output
I like tomatoes, therefore I like ketchup
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Checking Understanding of `git diff`

Consider this command: `git diff HEAD~9 guacamole.md`. What do you predict this command
will do if you execute it? What happens when you do execute it? Why?

Try another command, `git diff [ID] guacamole.md`, where [ID] is replaced with
the unique identifier for your most recent commit. What do you think will happen,
and what does happen?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Getting Rid of Staged Changes

`git restore` can be used to restore a previous commit when unstaged changes have
been made, but will it also work for changes that have been staged but not committed?
Make a change to `guacamole.md`, add that change using `git add`,
then use `git restore` to see if you can remove your change.

:::::::::::::::  solution

## Solution

After adding a change, `git restore` can not be used directly.
Let's look at the output of `git status`:

```output
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   guacamole.md

```

Note that if you don't have the same output
you may either have forgotten to change the file,
or you have added it _and_ committed it.

Using the command `git restore guacamole.md` now does not give an error,
but it does not restore the file either.
Git helpfully tells us that we need to use `git restore --staged` first
to unstage the file:

```bash
$ git restore --staged guacamole.md
```

Now, `git status` gives us:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git git restore <file>..." to discard changes in working directory)
        modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

This means we can now use `git restore` to restore the file
to the previous commit:

```bash
$ git restore guacamole.md
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Explore and Summarize Histories

Exploring history is an important part of Git, and often it is a challenge to find
the right commit ID, especially if the commit is from several months ago.

Imagine the `recipes` project has more than 50 files.
You would like to find a commit that modifies some specific text in `guacamole.md`.
When you type `git log`, a very long list appeared.
How can you narrow down the search?

Recall that the `git diff` command allows us to explore one specific file,
e.g., `git diff guacamole.md`. We can apply a similar idea here.

```bash
$ git log guacamole.md
```

Unfortunately some of these commit messages are very ambiguous, e.g., `update files`.
How can you search through these files?

Both `git diff` and `git log` are very useful and they summarize a different part of the history
for you.
Is it possible to combine both? Let's try the following:

```bash
$ git log --patch guacamole.md
```

You should get a long list of output, and you should be able to see both commit messages and
the difference between each commit.

Question: What does the following command do?

```bash
$ git log --patch HEAD~9 *.md
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git diff` displays differences between commits.
- `git restore` recovers old versions of files.

::::::::::::::::::::::::::::::::::::::::::::::::::
