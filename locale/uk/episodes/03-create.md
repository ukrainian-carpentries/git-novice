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

Ми продовжимо нашу вигадану історію, у якій Wolfman та Dracula досліджують, чи можливо відправити планетарний посадковий модуль на Марс.

![](fig/motivatingexample.png){alt='The main elements of the story: Dracula, Wolfman, the Mummy, Mars, Pluto and The Moon'}
[Werewolf vs dracula](https://www.deviantart.com/b-maze/art/Werewolf-vs-Dracula-124893530)
by [b-maze](https://www.deviantart.com/b-maze) / [Deviant Art](https://www.deviantart.com/).
[Mars](https://en.wikipedia.org/wiki/File:OSIRIS_Mars_true_color.jpg) by European Space Agency /
[CC-BY-SA 3.0 IGO](https://creativecommons.org/licenses/by/3.0/deed.en).
[Pluto](https://commons.wikimedia.org/wiki/File:PIA19873-Pluto-NewHorizons-FlyingPastImage-20150714-transparent.png) /
Courtesy NASA/JPL-Caltech.
[Mummy](https://commons.wikimedia.org/wiki/File:Mummy_icon_-_Noun_Project_4070.svg)
© Gilad Fried / [The Noun Project](https://thenounproject.com/) /
[CC BY 3.0](https://creativecommons.org/licenses/by/3.0/deed.en).
[Moon](https://commons.wikimedia.org/wiki/File:Lune_ico.png)
© Luc Viatour / [https://lucnix.be](https://lucnix.be/) /
[CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/deed.en).

Перш за все, створімо новий каталог у каталозі `Desktop` для нашої роботи, а потім змінимо поточний робочий каталог на новостворений:

```bash
$ cd ~/Desktop
$ mkdir planets
$ cd planets
```

Потім ми кажемо Git зробити `planets` [репозиторієм](../learners/reference.md#repository)
\-- місцем, де Git може зберігати версії наших файлів:

```bash
$ git init
```

Важливо пам'ятати, що `git init` створить репозиторій, який може включати підкаталоги та їх файли---немає потреби створювати окремі репозиторії вкладені в репозиторій `planets`, незалежно від того чи були підкаталоги присутні від початку або були додані пізніше. Крім того, зверніть увагу, що створення каталогу `planets` та його ініціалізація як репозиторію є повністю окремими процесами.

Якщо ми використаємо `ls`, щоб передивитись зміст каталогу, то здається, що нічого не змінилося:

```bash
$ ls
```

Але якщо ми додамо опцію `-a`, щоб показати усе, то побачимо, що Git створив у каталозі `planets` прихований каталог під назвою `.git`:

```bash
$ ls -a
```

```output
.	..	.git
```

Git використовує цей спеціальний підкаталог для зберігання всієї інформації про проєкт, у тому числі про файли, що відстежуються, і про підкаталоги, які містяться в каталозі проєкту.
Якщо ми коли-небудь видалимо підкаталог `.git`, то ми втратимо усю історію проєкту.

Далі, ми вкажемо, що гілка, яка використовується за замовчуванням, має назву `main`
(залежно від ваших налаштувань і версії Git, вона може вже мати цю назву).
Перегляньте [епізод про налаштування](02-setup.md#default-git-branch-naming), щоб дізнатися більше про цю зміну.

```bash
$ git checkout -b main
```

```output
Switched to a new branch 'main'
```

We can now start using one of the most important git commands, which is particularly helpful to beginners. `git status` tells us the status of our project, and better, a list of changes in the project and options on what to do with those changes. We can use it as often as we want, whenever we want to understand what is going on.

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Якщо ви користуєтеся іншою версією `git`, точний вигляд результату цієї команди може дещо відрізнятися.

:::::::::::::::::::::::::::::::::::::::  challenge

## Де створювати репозиторії Git

Разом із відстеженням інформації про планети (проєкт, який ми вже створили), Dracula також хотів би відстежувати інформацію про супутники.
Попри занепокоєння Wolfman, Dracula створює проєкт `moons` всередині проєкту `planets` за допомогою наступної послідовності команд:

```bash
$ cd ~/Desktop   # повернутися до каталогу Desktop
$ cd planets     # перейти до каталогу planets, який вже є репозиторієм Git
$ ls -a          # перевірити, що підкаталог .git все ще присутній у каталозі planets
$ mkdir moons    # створити підкаталог planets/moons
$ cd moons       # перейти у підкаталог moons
$ git init       # створити репозиторій Git у підкаталозі moons
$ ls -a          # перевірити наявність підкаталогу .git, що підтверджує створення нового репозиторію
```

Чи потрібна команда `git init`, яка виконується в підкаталозі `moons`, для відстеження файлів, що зберігаються в підкаталозі `moons`?

:::::::::::::::  solution

## Рішення

Ні. Dracula не має необхідності робити підкаталог `moons` репозиторієм Git, тому що репозиторій `planets` може відстежувати будь-які файли, підкаталоги, та файли у підкаталогах, вкладені (на будь-якому рівні) у каталог `planets`.  Таким чином, для того, щоб відстежувати всю інформацію про супутники, Dracula повинен був лише додати підкаталог `moons` до каталогу `planets`.

Крім того, репозиторії Git можуть заважати один одному, якщо вони "вкладені": зовнішній репозиторій намагатиметься відстежувати зміни у внутрішньому репозиторії. Тому найкраще кожного разу створювати новий репозиторій Git в окремому каталозі. Щоб переконатися, що каталог не є репозиторієм, перевірте результат команди `git status`. Якщо він виглядає як показано нижче, ви можете створити новий репозиторій, як було показано вище:

```bash
$ git status
```

```output
fatal: Not a git repository (or any of the parent directories): .git
```

:::::::::::::::::::::::::

## Виправлення помилок `git init`

Wolfman пояснив, як вкладений репозиторій є зайвим і може призвести до плутанини у майбутньому. Dracula would like to go back to a single git repository. Як Dracula може скасувати свою останню команду `git init` виконану у підкаталозі `moons`?

:::::::::::::::  solution

## Відповідь (КОРИСТУЙТЕСЯ З ОБЕРЕЖНІСТЮ!)

### Контекст

Видалення файлів з репозиторію Git слід виконувати з обережністю. Проте, ми ще не навчилися вказувати Git як відстежувати певний файл; про це ми дізнаємося в наступному епізоді. Файли, які не відстежуються Git, можна легко видалити, як і будь-які інші "звичайні" файли:

```bash
$ rm filename
```

Similarly a directory can be removed using `rm -r dirname`.
Якщо файли чи каталоги, які видаляються таким чином, вже відстежуються Git, тоді їх видалення стає ще однією зміною, яку нам потрібно буде відстежувати (ми побачимо як це робити в наступному епізоді).

### Відповідь

Git зберігає всі свої файли в каталозі `.git`.
To recover from this little mistake, Dracula can remove the `.git`
folder in the moons subdirectory by running the following command from inside the `planets` directory:

```bash
$ rm -rf moons/.git
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
