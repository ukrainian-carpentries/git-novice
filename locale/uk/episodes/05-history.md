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

Оскільки кожного разу ми додавали невеликі зміни до `guacamole.md`, зараз нам буде легко відстежувати наш прогрес. Зробімо це, використовуючи `HEAD`.  Перш ніж ми почнемо, змінімо `guacamole.md`, додавши ще один рядок.

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

Ми також можемо посилатися на коміти, використовуючи
їх ідентифікатори (ці довгі рядки цифр і літер),
які можна побачити за допомогою `git log` та `git show`.
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
Припустимо, що ми передумали щодо останнього оновлення файлу `guacamole.md` (рядок "ill-considered change").

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

Як можна здогадатися з назви, 'git restore' відновлює стару версію файлу.
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

Звернуть увагу, що зміни зараз знаходяться у зоні стейджінгу та не були збережені у коміті.
Знову ж таки, ми можемо повернути цей файл до його попереднього стану за допомогою `git restore`:

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
recent commit (`HEAD~1`), which is commit `f22b25e`. Ми використовуємо `.`, щоб позначити всі файли:

![](fig/git-restore.svg){alt='Використання git restore для відновлення попередньої версії двох файлів'}

Отже, якщо скласти це все разом, то Git працює як зображено у цьому коміксі:

![https://figshare.com/articles/How\_Git\_works\_a\_cartoon/1328266](fig/git_staging.svg){alt='Як працює git: зміни додаються до зони стейджингу (git add), зберігаються у репозиторії (git commit), та можуть бути відновлені з репозиторію (git checkout)'}

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

Команда `restore` відновлює файли з репозиторію, перезаписуючи файли у вашому робочому каталозі. Обидві відповіді 2 та 4 відновлюють _останню збережену в репозиторії_ версію файлу `data_cruncher.py`. Відповідь 2 використовує `HEAD`, щоб вказати _останній_ коміт, тоді як відповідь 4 використовує унікальний ідентифікатор останнього коміту, що саме й означає `HEAD`.

Відповідь 3 замінить `data_cruncher.py` його версією з коміту _перед_ `HEAD`, що НЕ є тим, що ми хотіли.

Відповідь 1 призведе до помилки. Вам потрібно вказати файл, який треба відновити. Якщо вам треба відновити всі файли,
скористайтеся `git restore .`

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Скасування коміту

Дженніфер співпрацює з колегами над її скриптом Python.  Вона зрозуміла, що її останній коміт до репозиторію проєкту містив помилку, і хоче його скасувати.  Дженніфер хоче скасувати його правильним чином, щоб всі користувачі репозиторію цього проєкту отримали правильні зміни. Команда `git revert [erroneous commit ID]` створить новий коміт, який скасує помилковий коміт.

Команда `git revert` відрізняється від `git restore -s [commit ID] .` тим, що `git restore` повертає файли (де зміни ще не ввійшли до нового коміту у локальному репозиторії) до їх попереднього стану. У той час, як `git revert` скасовує зміни, які вже внесені до локального та віддаленого репозиторіїв.

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

## Відповідь

Правильною є відповідь 2.

Зміни, внесені другою командою `echo`, будуть зроблені лише у робочій копії цього файлу, залишаючи версію в зоні стейджингу без змін.
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

## Перевіримо розуміння `git diff`

Consider this command: `git diff HEAD~9 guacamole.md`. Що ви очікуєте від цієї команди після виконання? Що насправді відбувається, коли ви запускаєте її? Чому?

Try another command, `git diff [ID] guacamole.md`, where [ID] is replaced with
the unique identifier for your most recent commit. Що ви очікуєте, і що насправді станеться?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Скасування змін у зоні стейджингу

Команда `git restore` може бути використана для відновлення попереднього коміту, коли зміни ще не додані до зони стейджингу. Але чи спрацює вона зі змінами, які були додані до зони стейджингу, але ще не збережені у коміті?
Make a change to `guacamole.md`, add that change using `git add`,
then use `git restore` to see if you can remove your change.

:::::::::::::::  solution

## Відповідь

Після додавання зміни за допомогою `git restore`, команду `git checkout` не можна використовувати безпосередньо.
Подивіться на результат `git status`:

```output
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   guacamole.md

```

Зауважте, якщо ваш результат відрізняється, то можливо ви забули змінити файл, або вже додали його до зони стейджингу _та_ зробили коміт.

Using the command `git restore guacamole.md` now does not give an error,
but it does not restore the file either.
Git друкує корисне повідомлення - нам потрібно спочатку використати `git restore --staged`, щоб видалити файл з зони стейджингу:

```bash
$ git restore --staged guacamole.md
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
        modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Це означає, що тепер ми можемо використовувати `git restore` для відновлення файлу до попереднього коміту:

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
Коли ви вводите `git log`, з'являється дуже довгий список.
Як можна звузити коло пошуку?

Recall that the `git diff` command allows us to explore one specific file,
e.g., `git diff guacamole.md`. Ми можемо застосувати подібну ідею тут.

```bash
$ git log guacamole.md
```

На жаль, деякі повідомлення комітів дуже неоднозначні, наприклад, `update files`.
Як же переглянути усі ці версії файлу?

Обидві команди `git diff` та `git log` дуже корисні для отримання звітів про різні частини історії проєкту.
Але чи можна об'єднати їх результат в одну команду? Спробуймо наступне:

```bash
$ git log --patch guacamole.md
```

Ви повинні отримати довгий список, у якому ви побачите як повідомлення коміту, так і зроблені зміни.

Питання: Що робить наступна команда?

```bash
$ git log --patch HEAD~9 *.md
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git diff` displays differences between commits.
- `git restore` відновлює старі версії файлів.

::::::::::::::::::::::::::::::::::::::::::::::::::
