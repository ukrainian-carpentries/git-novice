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

Оскільки кожного разу ми додавали тільки один рядок до `mars.txt`, зараз нам буде легко простежити досягнутий прогрес. Зробімо це, використовуючи `HEAD`.  Перш ніж ми почнемо,
давайте змінимо значення на `mars.txt`, додавши ще один рядок.

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
An ill-considered change
```

Тепер подивимося на наш результат.

```bash
$ git diff HEAD mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index b36abfd..0848c8d 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,3 +1,4 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
 But the Mummy will appreciate the lack of humidity
+An ill-considered change.
```

Це те саме, що ви отримаєте, якщо пропустите `HEAD` (спробуйте це).\
Але справжня користь полягає у тому, що ви таким чином можете посилатися на попередні коміти.  Наприклад, додаючи `~1` (де "~" - це тільда), ми посилаємось на коміт зроблений безпосередньо перед `HEAD`.

```bash
$ git diff HEAD~1 mars.txt
```

Якщо ми хочемо побачити різницю між старішими комітами, ми можемо знову використовувати `git diff`, використовуючи для послання на них `HEAD~1`, `HEAD~2` тощо:

```bash
$ git diff HEAD~2 mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

Ми також можемо використати команду `git show`, яка показує які зміни ми внесли у будь-якому попередньо зробленому коміті, а також і повідомлення коміту (на відміну від команди `git diff` яка покаже _різницю_ між комітом та нашим робочим каталогом).

```bash
$ git show HEAD~2 mars.txt
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base

diff --git a/mars.txt b/mars.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/mars.txt
@@ -0,0 +1 @@
+Cold and dry, but everything is my favorite color
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
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

Це правильна відповідь, проте, введення випадкових рядків з 40 символів дуже незручно. Тож Git дозволяє нам використовувати тільки перші кілька символів (як правило, сім для проєктів нормального розміру):

```bash
$ git diff f22b25e mars.txt
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
```

Це чудово! Отже,
ми можемо зберегти зміни у файлах і побачити, що ми змінили. Наступне питання - а як ми можемо відновити їх старіші версії?
Припустимо, що ми передумали щодо останнього оновлення файлу `mars.txt` (рядок "ill-considered change").

`git status` зараз каже нам, що файл був змінений, але ці зміни не були додані до зони стейджингу:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)

    modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

We can put things back the way they were
by using `git restore`:

```bash
$ git restore mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
```

As you might guess from its name,
`git restore` restores an old version of a file.
By default,
it recovers the version of the file recorded in `HEAD`,
which is the last saved commit.
If we want to go back even further,
we can use a commit identifier instead, using `-s` option:

```bash
$ git restore -s f22b25e mars.txt
```

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
```

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

Звернуть увагу, що зміни зараз знаходяться у зоні стейджінгу.
Again, we can put things back the way they were by using `git restore`:

```bash
$ git restore mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
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

5. Збережіть його та закрийте редактор.

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
$ cd planets
$ echo "Venus is beautiful and full of love" > venus.txt
$ git add venus.txt
$ echo "Venus is too hot to be suitable as a base" >> venus.txt
$ git commit -m "Comment on Venus as an unsuitable base"
$ git restore venus.txt
$ cat venus.txt #this will print the contents of venus.txt to the screen
```

1. ```output
   ```

Venus is too hot to be suitable as a base

````
2. ```output
Venus is beautiful and full of love
````

3. ```output
   ```

Venus is beautiful and full of love
Venus is too hot to be suitable as a base

````
4. ```output
Error because you have changed venus.txt without committing the changes
````

:::::::::::::::  solution

## Рішення

Правильною є відповідь 2.

Команда `git add venus.txt` розміщує поточну версію 'venus.txt' в зоні стейджингу.
Зміни до файлу з другої команди `echo` будуть зроблені лише у робочій копії цього файлу, але не у його версії в зоні стейджингу.

Тож, коли виконується команда `git commit -m "Comment on Venus as an unsuitable base"`, версія `venus.txt`, яка буде збережена у коміті, буде з зони стейджингу та буде мати тільки один рядок.

На цей час робоча копія файлу ще має другий рядок (і тому `git status` покаже, що файл змінено). However, `git restore venus.txt`
replaces the working copy with the most recently committed version of `venus.txt`.

Тож, `cat venus.txt` покаже

```output
Venus is beautiful and full of love.
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Перевірка розуміння `git diff`

Розглянемо цю команду: `git diff HEAD~9 mars.txt`. Що, на вашу думку, зробить ця команда, якщо ви її виконаєте? Що станеться, коли ви її виконаєте? Чому?

Спробуйте іншу команду, `git diff [ID] mars.txt`, де [ID] замінено на унікальний ідентифікатор вашого останнього коміту. Як ви думаєте, що вона зробить? Виконайте її та перевірте, чи це так.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Скасування змін у зоні стейджингу

`git restore` can be used to restore a previous commit when unstaged changes have
been made, but will it also work for changes that have been staged but not committed?
Make a change to `mars.txt`, add that change using `git add`,
then use `git restore` to see if you can remove your change.

:::::::::::::::  solution

## Відповідь

After adding a change, `git restore` can not be used directly.
Подивіться на результат `git status`:

```output
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   mars.txt

```

Зауважте, що якщо ви не маєте такого самого результату, то можливо, ви забули змінити файл, або ви не тільки додали його до зони стейджингу, _а також_ зробили коміт.

Using the command `git restore mars.txt` now does not give an error,
but it does not restore the file either.
Git helpfully tells us that we need to use `git restore --staged` first
to unstage the file:

```bash
$ git restore --staged mars.txt
```

Тепер `git status` зображує наступне:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git git restore <file>..." to discard changes in working directory)

        modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

This means we can now use `git restore` to restore the file
to the previous commit:

```bash
$ git restore mars.txt
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Перегляд історії

Перегляд історії є важливою частиною роботи з Git, але часто нелегко знайти правильний ідентифікатор коміту, особливо якщо коміт був зроблений декілька місяців тому.

Уявіть, що проєкт `planets` має більш ніж 50 файлів.
Ви хотіли б знайти коміт, який змінює певний текст у `mars.txt`.
Коли ви вводите `git log`, з'являється дуже довгий список.
Як можна звузити коло пошуку?

Нагадаємо, що команда `git diff` може використовуватись для одного конкретного файлу, наприклад, `git diff mars.txt`. Ми можемо застосувати тут подібну ідею.

```bash
$ git log mars.txt
```

На жаль, деякі з цих повідомлень комітів дуже неоднозначні, наприклад, `update files`.
Як же переглянути усі ці версії файлу?

Обидві команди `git diff` та `git log` дуже корисні для отримання звітів про різні деталі історії проєкту.
Але чи можна об'єднати їх результат в одну команду? Давайте спробуємо наступне:

```bash
$ git log --patch mars.txt
```

Ви повинні отримати довгий список, у якому ви побачите як повідомлення коміту, так і зроблені зміни.

Питання: Що робить наступна команда?

```bash
$ git log --patch HEAD~9 *.txt
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git diff` відображає відмінності між комітами.
- `git restore` recovers old versions of files.

::::::::::::::::::::::::::::::::::::::::::::::::::
