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

Щоб побачити, як ми можемо розвʼязувати конфлікти, спочатку ми повинні їх створити.  Файл `mars.txt` наразі для обох партнерів виглядає однаково в їх клонах репозиторію `planets`:

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
```

Тепер додамо один рядок до копії співавтора:

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
This line added to Wolfman's copy
```

а потім відправимо наші зміни на GitHub:

```bash
$ git add mars.txt
$ git commit -m "Add a line in our home copy"
```

```output
[main 5ae9631] Add a line in our home copy
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
To https://github.com/vlad/planets.git
   29aba7c..dabb4c8  main -> main
```

Тепер нехай власник зробить іншу зміну у своїй копії без отримання нових змін з GitHub:

```bash
$ nano mars.txt
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We added a different line in the other copy
```

Ми можемо зробити коміт наших змін локально:

```bash
$ git add mars.txt
$ git commit -m "Add a line in my copy"
```

```output
[main 07ebc69] Add a line in my copy
 1 file changed, 1 insertion(+)
```

але Git не дозволить нам відправити зміни на GitHub:

```bash
$ git push origin main
```

```output
To https://github.com/vlad/planets.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/vlad/planets.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

![](fig/conflict.svg){alt='Суперечливі зміни'}

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
From https://github.com/vlad/planets
 * branch            main     -> FETCH_HEAD
    29aba7c..dabb4c8  main     -> origin/main
Auto-merging mars.txt
CONFLICT (content): Merge conflict in mars.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Команда `git pull` оновлює локальний репозиторій, щоб додати до нього ті зміни, які вже містяться у віддаленому репозиторії.
Після того, як зміни з віддаленої гілки отримані, Git виявляє, що зміни, внесені до локальної копії, накладаються на ті, що були додані до віддаленого репозиторію, і тому Git відмовляється об'єднати дві версії, щоб уберегти нас від зіпсування нашої попередньої роботи. У файлі, де є конфлікт, він позначається наступним чином:

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
<<<<<<< HEAD
We added a different line in the other copy
=======
This line added to Wolfman's copy
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
```

Нашим змінам передує `<<<<<<< HEAD`.
Потім Git вставив `=======` як роздільник між змінами, що конфліктують, та позначив кінець вмісту, завантаженого з GitHub, за допомогою ` `.
(Рядок букв і цифр після цього маркера ідентифікує щойно завантажений коміт.)

Тепер ми маємо відредагувати цей файл, щоб видалити ці маркери та узгодити зміни.
Ми можемо зробити все, що бажаємо: зберегти зміни, зроблені у локальному сховищі, зберегти зміни, зроблені у віддаленому сховищі, написати щось нове, щоб замінити обидві версії, або позбутися змін повністю.
Замінимо обидві версії так, щоб файл виглядав наступним чином:

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We removed the conflict on this line
```

Щоб закінчити злиття, ми додаємо `mars.txt` до зони стейджингу для цього злиття, а потім робимо коміт:

```bash
$ git add mars.txt
$ git status
```

```output
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   mars.txt

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
To https://github.com/vlad/planets.git
   dabb4c8..2abf2b1  main -> main
```

Git відстежує, що ми об'єднали з чим, тому нам не потрібно знову виправляти речі вручну коли співавтор, який зробив першу зміну, знову виконує `git pull`:

```bash
$ git pull origin main
```

```output
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

Ми отримуємо файл з об'єднаними змінами:

```bash
$ cat mars.txt
```

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We removed the conflict on this line
```

Нам не потрібно знову робити злиття змін, тому що Git знає, що хтось це вже зробив.

Здатність Git вирішувати конфлікти дуже корисна, але це вимагає часу і зусиль, і може призвести до помилок, якщо конфлікти не вирішуються правильно. Якщо ви побачите, що конфлікти при злитті змін трапляються у вашому проєкті дуже часто, розгляньте ці технічні підходи до їх зменшення:

- Отримуйте зміни частіше, особливо перед початком нової роботи
- Використовуйте тематичні гілки для розділення роботи, з їх подальшим об'єднанням до гілки `main`, коли робота буде завершена
- Робіть коміти невеликими та атомарними
- Надсилайте свою роботу, коли вона закінчена, до спільного репозиторію, та заохочуйте свою команду робити те ж саме, щоб зменшити обсяг незавершеної роботи та, відповідно, ймовірність конфліктів
- Там, де логічно доречно, розбивайте великі файли на менші, щоб було менше шансів, що два автори змінять один і той самий файл одночасно

Конфлікти також можна мінімізувати за допомогою дотримання деяких стратегій управління проєктами:

- Домовляйтеся з вашими співавторами про те, хто відповідає за які аспекти проєкту
- Обговоріть зі своїми співавторами, у якому порядку слід виконувати задачі, щоб ті, які можуть змінити одні і ті ж самі рядки, не виконувались одночасно
- Якщо конфлікти є стилістичними (наприклад, табуляції або пробіли), домовтеся про стиль коду, яким ви будете керуватися, та використовуйте, якщо необхідно, інструменти для перевірки цього стилю (наприклад, `htmltidy`, `perltidy`, `rubocop`, та ін.) для забезпечення єдиного стилю

:::::::::::::::::::::::::::::::::::::::  challenge

## Вправа: створення та вирішення конфліктів

Клонуйте репозиторій, створений вашим інструктором.
Додайте до нього новий файл та змініть деякий файл, що вже існує (ваш інструктор скаже, який саме).
Потім, після підтвердження від вашого інструктора, отримайте нові зміни з цього репозиторію, щоб створити конфлікт, а потім вирішіть його.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Конфлікти у бінарних файлах

Що робить Git, коли виникає конфлікт у зображенні або іншому бінарному файлі, що зберігається у системі контролю версій?

:::::::::::::::  solution

## Рішення

Спробуймо це дослідити. Припустимо, Dracula робить фотографію марсіанської поверхні та називає її `mars.jpg`.

Якщо у вас немає файлу із зображенням Марса, ви можете створити фіктивний бінарний файл ось так:

```bash
$ head -c 1024 /dev/urandom > mars.jpg
$ ls -lh mars.jpg
```

```output
-rw-r--r-- 1 vlad 57095 1.0K Mar  8 20:24 mars.jpg
```

`ls` показує, що було створено файл розміром 1 кілобайт. Він містить випадкові байти, які були зчитані зі спеціального файлу `/dev/urandom`.

Тепер припустимо, що Dracula додає `mars.jpg` до свого репозиторію:

```bash
$ git add mars.jpg
$ git commit -m "Add picture of Martian surface"
```

```output
[main 8e4115c] Add picture of Martian surface
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 mars.jpg
```

Припустимо, що Wolfman тим часом додав схожу фотографію.
Його зображення - марсіанське небо, але воно _також_ названо `mars.jpg`.
Коли Dracula намагається відправити зміни до віддаленого репозиторію, він отримує знайоме повідомлення:

```bash
$ git push origin main
```

```output
To https://github.com/vlad/planets.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/vlad/planets.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Як ми вже дізналися раніше, ми повинні спочатку отримати зміни та вирішити будь-які конфлікти:

```bash
$ git pull origin main
```

Коли конфлікт виникає у зображенні або іншому бінарному файлі, git друкує повідомлення на кшталт наступного:

```output
$ git pull origin main
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/vlad/planets.git
 * branch            main     -> FETCH_HEAD
   6a67967..439dc8c  main     -> origin/main
warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
Auto-merging mars.jpg
CONFLICT (add/add): Merge conflict in mars.jpg
Automatic merge failed; fix conflicts and then commit the result.
```

Повідомлення про конфлікт в основному виглядає так само як і для `mars.txt`, але воно має один додатковий рядок:

```output
warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
```

Git не може автоматично вставляти маркери конфлікту у зображення, як він це робить для текстових файлів. Отже, замість редагування файлу зображення, ми повинні викликати з історії змін ту його версію, яку ми хочемо зберегти. Після цього ми можемо виконати відповідні команди `git add` та `git commit`, щоб зберегти цю версію.

В додатковому рядку вище, Git зручно надав нам ідентифікатори коміту для обох версій `mars.jpg`. Наша версія - `HEAD`, а Wolfman зберіг версію `439dc8c0...`. Якщо ми хочемо використовувати нашу версію, ми можемо застосувати `git checkout`:

```bash
$ git checkout HEAD mars.jpg
$ git add mars.jpg
$ git commit -m "Use image of surface instead of sky"
```

```output
[main 21032c3] Use image of surface instead of sky
```

Якщо замість цього ми хочемо використовувати версію, яку додав Wolfman, ми можемо застосувати `git checkout` з ідентифікатором коміту, який зробив Wolfman - `439dc8c0`:

```bash
$ git checkout 439dc8c0 mars.jpg
$ git add mars.jpg
$ git commit -m "Use image of sky instead of surface"
```

```output
[main da21b34] Use image of sky instead of surface
```

Ми також можемо зберегти _обидва_ зображення. Але ж проблема у тому, що ми не зможемо залишити їх під однією й тією ж назвою. Проте ми можемо викликати з історії кожну версію послідовно та _перейменувати_ файл, а потім додати перейменовані версії до репозиторію за допомогою `git add`. Спочатку відновіть з історії змін кожне зображення та перейменуйте його:

```bash
$ git checkout HEAD mars.jpg
$ git mv mars.jpg mars-surface.jpg
$ git checkout 439dc8c0 mars.jpg
$ mv mars.jpg mars-sky.jpg
```

Потім видаліть старий `mars.jpg` та додайте два нових файли:

```bash
$ git rm mars.jpg
$ git add mars-surface.jpg
$ git add mars-sky.jpg
$ git commit -m "Use two images: surface and sky"
```

```output
[main 94ae08c] Use two images: surface and sky
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 mars-sky.jpg
 rename mars.jpg => mars-surface.jpg (100%)
```

Тепер обидва зображення Марса містяться у репозиторії, а файл `mars.jpg` більше не існує.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Типова робоча сесія

Ви сідаєте за комп'ютер, щоб працювати над спільним проєктом, який відстежується у віддаленому репозиторії Git. Під час сеансу роботи ви виконуєте наступні дії, але не в тому порядку, як вони перелічені нижче:

- _Зробити зміни_, додавши число '100' до текстового файлу `numbers.txt`
- _Оновити віддалений репозиторій_, щоб він відповідав локальному репозиторію
- _Відсвяткувати_ свій успіх
- _Оновити локальний репозиторій_, щоб він відповідав віддаленому репозиторію
- _Додати зміни_ до зони стейджингу
- _Зробити коміт_ у локальному репозиторії

В якому порядку слід виконувати ці дії, щоб мінімізувати ймовірність конфліктів? Розташуйте їх у порядку виконання у стовпці "дія" в таблиці нижче. Коли ви розташуєте їх у відповідному порядку, подивіться, чи можете ви написати відповідні команди в стовпці "команда". Кілька кроків вже заповнені, щоб допомогти вам розпочати.

| крок | дія . . . . . . . . . . | команда . . . . . . . . . . |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 2    |                                                                                                                                                                                         | `echo 100 >> numbers.txt`                                                                                                                                                                   |
| 3    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 4    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 5    |                                                                                                                                                                                         |                                                                                                                                                                                             |
| 6    | Відсвяткувати!                                                                                                                                                                          | `AFK` (Away From Keyboard)                                                                                                                                               |

:::::::::::::::  solution

## Відповідь

| крок | дія . . . . . . | команда . . . . . . . . . . . . . . . . . . . |
| ---- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Оновити локальний репозиторій                                                                                   | `git pull origin main`                                                                                                                                                                                                                                                                                                                                        |
| 2    | Зробити зміни                                                                                                   | `echo 100 >> numbers.txt`                                                                                                                                                                                                                                                                                                                                     |
| 3    | Додати зміни до зони стейджингу                                                                                 | `git add numbers.txt`                                                                                                                                                                                                                                                                                                                                         |
| 4    | Зробити коміт                                                                                                   | `git commit -m "Add 100 to numbers.txt"`                                                                                                                                                                                                                                                                                                                      |
| 5    | Оновити віддалений репозиторій                                                                                  | `git push origin main`                                                                                                                                                                                                                                                                                                                                        |
| 6    | Відсвяткувати!                                                                                                  | `AFK` (Away From Keyboard)                                                                                                                                                                                                                                                                                                                 |

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Конфлікти виникають, коли двоє або більше людей змінюють однакові рядки в одному файлі.
- Система контролю версій не дозволяє користувачам перезаписувати зміни один одного наосліп, але виділяє конфлікти, щоб їх можна було вирішити.

::::::::::::::::::::::::::::::::::::::::::::::::::
