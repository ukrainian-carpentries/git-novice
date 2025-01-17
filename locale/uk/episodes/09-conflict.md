---
title: Конфлікти
teaching: 15
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Що таке конфлікти і коли вони можуть виникати.
- Вирішення конфліктів, що виникають внаслідок злиття змін.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Що робити, коли мої зміни конфліктують зі змінами інших?

::::::::::::::::::::::::::::::::::::::::::::::::::

Як тільки люди починають працювати паралельно, вони, швидше за все, "наступають один одному на ноги".  Це навіть може статися з однією людиною: якщо ми працюємо над програмою на нашому ноутбуці і на сервері в лабораторії водночас, ми можемо внести різні зміни в кожну копію.  Контроль версій допомагає нам вирішувати ці [конфлікти](../learners/reference.md#conflict), надаючи інструменти для
[узгодження](../learners/reference.md#resolve) змін, які накладаються одна на одну.

Щоб побачити, як ми можемо розвʼязувати конфлікти, спочатку ми повинні їх створити.  Наразі файл `guacamole.md` виглядає однаково у клонах обох партнерів нашого репозиторію `recipes`:

```bash
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

Тепер додамо один рядок до копії співавтора:

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
* put one avocado into a bowl.
```

а потім відправимо наші зміни на GitHub:

```bash
$ git add guacamole.md
$ git commit -m "First step on the instructions"
```

```output
[main 5ae9631] First step on the instructions
 1 file changed, 1 insertion(+)
```

```bash
$ git push origin main
```

```output
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 331 bytes | 331.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/alflin/recipes.git
   29aba7c..dabb4c8  main -> main
```

Тепер нехай власник зробить іншу зміну у своїй копії без отримання нових змін з GitHub:

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
* peel the avocados
```

Ми можемо зробити коміт наших змін локально:

```bash
$ git add guacamole.md
$ git commit -m "Add first step"
```

```output
[main 07ebc69] Add first step
 1 file changed, 1 insertion(+)
```

але Git не дозволить нам відправити зміни на GitHub:

```bash
$ git push origin main
```

```output
To https://github.com/alflin/recipes.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/alflin/recipes.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

![](fig/conflict.svg){alt='Конфлікт може виникати при злитті двох незалежно виконаних наборів змін'}

Git не дозволяє цю операцію, оскільки виявляє, що віддалений репозиторій має нові оновлення, які не були включені до локальної гілки.
Що ми повинні зробити - це отримати зміни з GitHub, [обʼєднати](../learners/reference.md#merge) їх з копією репозиторію, у якій ми зараз працюємо, а тільки потім надіслати їх на GitHub.
Давайте почнемо з отримання змін:

```bash
$ git pull origin main
```

```output
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
    29aba7c..dabb4c8  main     -> origin/main
Auto-merging guacamole.md
CONFLICT (content): Merge conflict in guacamole.md
Automatic merge failed; fix conflicts and then commit the result.
```

:::::::::::::::::::::::::::::::::::::::::  callout

## You may need to tell Git what to do

If you see the below in your output, Git is asking what it should do.

```output
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
```

In newer versions of Git it gives you the option of specifying different
behaviours when a pull would merge divergent branches. In our case we want
'the default strategy'. To use this strategy run the following command to
select it as the default thing git should do.

```bash
$ git config pull.rebase false
```

Then attempt the pull again.

```bash
$ git pull origin main
```

::::::::::::::::::::::::::::::::::::::::::::::::::

Команда `git pull` оновлює локальний репозиторій, щоб додати до нього ті зміни, які вже містяться у віддаленому репозиторії.
Після отримання змін із віддаленої гілки Git визначає, що зміни, внесені до локальної копії, конфліктують зі змінами, зробленими у віддаленому репозиторії. Тому, щоб запобігти перезапису нашої роботи, Git відмовляється об’єднувати дві версії. У файлі, де є конфлікт, він позначається наступним чином:

```bash
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
<<<<<<< HEAD
* peel the avocados
=======
* put one avocado into a bowl.
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
```

Нашим змінам передує `<<<<<<< HEAD`.
Потім Git вставив `=======` як роздільник між суперечливими змінами, та позначив кінець вмісту, завантаженого з GitHub, за допомогою `>>>>>>>`.
(Рядок літер і цифр після цього маркера ідентифікує щойно завантажений коміт.)

Тепер ми маємо відредагувати цей файл, щоб видалити ці маркери та узгодити зміни.
Ми можемо зробити все, що бажаємо: зберегти зміни з локального чи віддаленого репозиторію, написати щось нове та замінити обидві версії, або позбутися змін повністю.
Замінимо обидві версії так, щоб файл виглядав наступним чином:

```bash
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 645 bytes | 645.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 2 local objects.
To https://github.com/vlad/planets.git
   dabb4c8..2abf2b1  main -> main
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
* peel the avocados and put them into a bowl.
```

To finish merging,
we add `guacamole.md` to the changes being made by the merge
and then commit:

```bash
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 6 (delta 4), reused 6 (delta 4), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/vlad/planets
 * branch            main     -> FETCH_HEAD
    dabb4c8..2abf2b1  main     -> origin/main
Updating dabb4c8..2abf2b1
Fast-forward
 mars.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```output
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   guacamole.md

```

```bash
$ git commit -m "Merge changes from GitHub"
```

```output
[main 2abf2b1] Merge changes from GitHub
```

Тепер ми можемо відправити наші зміни на GitHub:

```bash
$ git push origin main
```

```output
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 645 bytes | 645.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 2 local objects.
To https://github.com/alflin/recipes.git
   dabb4c8..2abf2b1  main -> main
```

Git відстежує, що з чим було об’єднано, тому нам не потрібно знову виправляти конфлікти вручну коли співавтор, який зробив першу зміну, знову виконує `git pull`:

```bash
$ git pull origin main
```

```output
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 6 (delta 4), reused 6 (delta 4), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
    dabb4c8..2abf2b1  main     -> origin/main
Updating dabb4c8..2abf2b1
Fast-forward
 guacamole.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Ми отримуємо файл з об'єднаними змінами:

```bash
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lime
* salt
## Instructions
* peel the avocados and put them into a bowl.
```

Нам не потрібно знову робити злиття змін, оскільки Git знає, об’єднання вже завершено.

Здатність Git вирішувати конфлікти є дуже корисною, але вона вимагає часу та зусиль, і може призвести до помилок, якщо конфлікти не вирішуються належним чином. Якщо ви часто стикаєтеся з конфліктами в вашому проекті, розгляньте ці технічні підходи щоб мінімізувати їх:

- Pull from upstream more frequently, especially before starting new work
- Use topic branches to segregate work, merging to main when complete
- Make smaller more atomic commits
- Push your work when it is done and encourage your team to do the same to reduce work in progress and, by extension, the chance of having conflicts
- Там, де логічно доречно, розбивайте великі файли на менші, щоб зменшити ймовірність того, що кілька авторів редагуватимуть один і той самий файл одночасно

Конфлікти також можна мінімізувати за допомогою дотримання деяких стратегій управління проєктами:

- Чітко визначте обов’язки кожного співавтора, хто відповідає за які аспекти проєкту
- Обговоріть зі своїми співавторами порядок виконання задач, щоб уникнути одночасної роботи над тими самими рядками
- Якщо конфлікти є стилістичними (наприклад, табуляції або пробіли), домовтеся про стиль коду, яким ви будете керуватися, та використовуйте за необхідністю інструменти форматування (наприклад, `htmltidy`, `perltidy`, `rubocop`, та ін.) для забезпечення єдиного стилю

:::::::::::::::::::::::::::::::::::::::  challenge

## Вправа: створення та вирішення конфліктів

Клонуйте репозиторій, створений вашим інструктором.
Додайте до нього новий файл та змініть наявний файл (ваш інструктор скаже, який саме).
Потім, на запит вашого інструктора, отримайте нові зміни з цього репозиторію, щоб створити конфлікт, а потім вирішіть його.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Конфлікти у бінарних файлах

Як Git вирішує конфлікти в зображеннях або інших бінарних файлах, що зберігаються в системі контролю версій?

:::::::::::::::  solution

## Відповідь

Спробуймо це дослідити. Suppose Alfredo takes a picture of its guacamole and
calls it `guacamole.jpg`.

If you do not have an image file of guacamole available, you can create
a dummy binary file like this:

```bash
$ head --bytes 1024 /dev/urandom > guacamole.jpg
$ ls -lh guacamole.jpg
```

```output
-rw-r--r-- 1 alflin 57095 1.0K Mar  8 20:24 guacamole.jpg
```

`ls` показує, що було створено файл розміром 1 кілобайт. Він містить випадкові байти, які були зчитані зі спеціального файлу `/dev/urandom`.

Now, suppose Alfredo adds `guacamole.jpg` to his repository:

```bash
$ git add guacamole.jpg
$ git commit -m "Add picture of guacamole"
```

```output
[main 8e4115c] Add picture of guacamole
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 guacamole.jpg
```

Suppose that Jimmy has added a similar picture in the meantime.
His is a picture of a guacamole with nachos, but it is _also_ called `guacamole.jpg`.
When Alfredo tries to push, he gets a familiar message:

```bash
$ git push origin main
```

```output
To https://github.com/alflin/recipes.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/alflin/recipes.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Як ми вже дізналися раніше, ми спочатку маємо отримати зміни та вирішити усі конфлікти:

```bash
$ git pull origin main
```

When there is a conflict on an image or other binary file, git prints
a message like this:

```output
$ git pull origin main
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/alflin/recipes.git
 * branch            main     -> FETCH_HEAD
   6a67967..439dc8c  main     -> origin/main
warning: Cannot merge binary files: guacamole.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
Auto-merging guacamole.jpg
CONFLICT (add/add): Merge conflict in guacamole.jpg
Automatic merge failed; fix conflicts and then commit the result.
```

The conflict message here is mostly the same as it was for `guacamole.md`, but
there is one key additional line:

```output
warning: Cannot merge binary files: guacamole.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
```

Git не може автоматично вставляти маркери конфлікту у зображення, як він це робить для текстових файлів. Тому замість того, щоб редагувати файл зображення, нам потрібно викликати з історії змін ту його версію, яку ми хочемо зберегти. Після цього ми можемо виконати відповідні команди `git add` та `git commit`, щоб зберегти цю версію.

On the key line above, Git has conveniently given us commit identifiers
for the two versions of `guacamole.jpg`. Our version is `HEAD`, and Jimmy's
version is `439dc8c0...`. Якщо ми хочемо використовувати нашу версію, ми можемо застосувати `git checkout`:

```bash
$ git checkout HEAD guacamole.jpg
$ git add guacamole.jpg
$ git commit -m "Use image of just guacamole instead of with nachos"
```

```output
[main 21032c3] Use image of just guacamole instead of with nachos
```

If instead we want to use Jimmy's version, we can use `git checkout` with
Jimmy's commit identifier, `439dc8c0`:

```bash
$ git checkout 439dc8c0 guacamole.jpg
$ git add guacamole.jpg
$ git commit -m "Use image of guacamole with nachos instead of just guacamole"
```

```output
[main da21b34] Use image of guacamole with nachos instead of just guacamole
```

Ми також можемо зберегти _обидва_ зображення. Але ж проблема полягає в тому, що ми не зможемо зберегти їх з однаковими назвами. Проте ми можемо викликати з історії кожну версію послідовно та _перейменувати_ її, а потім додати перейменовані версії до репозиторію за допомогою `git add`. Спочатку "дістаньте" з історії змін кожне зображення та перейменуйте його:

```bash
$ git checkout HEAD guacamole.jpg
$ git mv guacamole.jpg guacamole-only.jpg
$ git checkout 439dc8c0 guacamole.jpg
$ mv guacamole.jpg guacamole-nachos.jpg
```

Then, remove the old `guacamole.jpg` and add the two new files:

```bash
$ git rm guacamole.jpg
$ git add guacamole-only.jpg
$ git add guacamole-nachos.jpg
$ git commit -m "Use two images: just guacamole and with nachos"
```

```output
[main 94ae08c] Use two images: just guacamole and with nachos
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 guacamole-nachos.jpg
 rename guacamole.jpg => guacamole-only.jpg (100%)
```

Now both images of guacamole are checked into the repository, and `guacamole.jpg`
no longer exists.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Звичайна робоча сесія

Ви сідаєте за комп'ютер, щоб працювати над спільним проєктом, який відстежується у віддаленому репозиторії Git. Під час робочого сеансу ви виконуєте наступні дії, хоча не обов’язково в такому порядку:

- _Make changes_ by appending the number `100` to a text file `numbers.txt`
- _Оновити віддалений репозиторій_, щоб він відповідав локальному репозиторію
- _Відсвяткувати_ свій успіх
- _Оновити локальний репозиторій_, щоб він відповідав віддаленому репозиторію
- _Додати зміни_ до зони стейджингу
- _Зробити коміт_ у локальному репозиторії

В якому порядку слід виконувати ці дії, щоб мінімізувати ймовірність конфліктів? Розташуйте їх у порядку виконання в стовпці "дія" в таблиці нижче. Коли ви розташуєте їх у відповідному порядку, спробуйте написати відповідні команди в стовпці "команда". Кілька кроків вже заповнені, щоб допомогти вам розпочати.

| крок | дія . . . . . . . . . . | команда . . . . . . . . . . |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 2    |                                                                                                                                                                                         | `echo 100 >> numbers.txt`                                                                                                                                                                   |
| 3    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 4    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 5    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 6    | Відсвяткувати!                                                                                                                                                                          |                                                                                                                                                                                             |

:::::::::::::::  solution

## Відповідь

| крок | дія . . . . . . | команда . . . . . . . . . . . . . . . . . . . |
| ---- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Оновити локальний репозиторій                                                                                   | `git pull origin main`                                                                                                                                                                                                                                                                                                                                        |
| 2    | Зробити зміни                                                                                                   | `echo 100 >> numbers.txt`                                                                                                                                                                                                                                                                                                                                     |
| 3    | Додати зміни до зони стейджингу                                                                                 | `git add numbers.txt`                                                                                                                                                                                                                                                                                                                                         |
| 4    | Зробити коміт                                                                                                   | `git commit -m "Add 100 to numbers.txt"`                                                                                                                                                                                                                                                                                                                      |
| 5    | Оновити віддалений репозиторій                                                                                  | `git push origin main`                                                                                                                                                                                                                                                                                                                                        |
| 6    | Відсвяткувати!                                                                                                  |                                                                                                                                                                                                                                                                                                                                                               |

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Конфлікти виникають, коли двоє або більше людей змінюють ті самі рядки в одному файлі.
- Система контролю версій не дозволяє користувачам перезаписувати зміни один одного наосліп, але виділяє конфлікти, щоб їх можна було вирішити.

::::::::::::::::::::::::::::::::::::::::::::::::::
