---
title: Обговорення
---

## Поширені запитання

Зазвичай, у людей виникають питання щодо Git, що виходять за рамки основного матеріалу.
Студентам, які завершили решту уроків, може бути корисно розглянути наступні теми.

Зауважте, що оскільки цей матеріал не є обов'язковим для базового використання Git, він не буде розглядатися інструктором.

## Додаткові налаштування Git

Під час [Налаштування Git](../episodes/02-setup.md), ми використовували `git config --global`, щоб встановити деякі параметри за замовчуванням для Git.
Отже, ці параметри конфігурації зберігаються у вашому домашньому каталогу
у звичайному текстовому файлі під назвою `.gitconfig`.

```bash
$ cat ~/.gitconfig
```

```output
[user]
	name = Alfredo Linguini
	email = a.linguini@ratatouille.fr
[color]
	ui = true
[core]
	editor = nano
```

Цей файл можна відкрити у вашому улюбленому текстовому редакторі.
(Але ми радимо продовжити використання команди `git config` для змін у ньому,
оскільки це допомагає уникнути синтаксичних помилок.)

Зрештою, ви забажаєте почати налаштовувати поведінку Git, щоб зробити її більш зручною.
Це можна зробити, додавши більше записів до вашого `.gitconfig`.
Наявні параметри описані в документації:

```bash
$ git config --help
```

Зокрема, вам може знадобитися додати псевдоніми для деяких команд.
Це щось на кшталт скорочень для довших команд Git.
Наприклад, якщо вам набридло постійно вводити `git checkout`,
ви можете виконати команду:

```bash
$ git config --global alias.co checkout
```

Тепер, якщо ми повернемося до прикладу з епізоду [Досліджуючи історію](../episodes/05-history.md), де ми виконували:

```bash
$ git checkout f22b25e guacamole.md
```

ми могли б тепер ввести:

```bash
$ git co f22b25e guacamole.md
```

## Стилізація журналу Git

A good target for customization is output from the log.
Типовий журнал є досить детальним, але йому бракує графічних підказок,
таких як вказівок на те, які коміти були зроблені локально,
а які були отримані з віддалених репозиторіїв.

You can use `git log --help` and `git config --help` to look for different ways to change
the log output.
Спробуйте наступні команди та подивіться, який ефект вони матимуть:

```bash
$ git config --global alias.lg "log --graph"
$ git config --global log.abbrevCommit true
$ git config --global format.pretty oneline
$ git lg
```

Якщо вам не подобаються ці ефекти,
ви можете скасувати їх за допомогою:

```bash
$ git config --global --unset alias.lg
$ git config --global --unset log.abbrevCommit
$ git config --global --unset format.pretty
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Скасування змін конфігурації Git

Ви можете використовувати опцію `--unset` для видалення небажаних параметрів з `.gitconfig`.
Another way to roll back changes is to store your `.gitconfig` using Git.

For hints on what you might want to configure,
go to GitHub and search for "gitconfig".
Ви знайдете сотні репозиторіїв, у яких люди зберегли
свої власні файли конфігурації Git.
Відсортуйте їх за кількістю зірок і розгляньте кілька найкращих.
Якщо ви знайдете ті, які вам подобаються,
будь ласка, переконайтеся, що вони є ліцензійними з відкритим вихідним кодом, перш ніж клонувати їх.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Нетекстові файли

Пригадайте, коли ми обговорювали [Конфлікти](../episodes/09-conflict.md)
було завдання, яке запитувало:
"Що робить Git
коли виникає конфлікт у зображенні або якомусь іншому нетекстовому файлі,
який зберігається у системі контролю версій?"

Тепер ми розглянемо це питання більш детально.

Багато людей бажають відстежувати версії нетекстових файлів, таких як зображення, PDF-файли та документи Microsoft Office або LibreOffice.
Насправді Git може обробляти ці типи файлів (які потрапляють у категорію "бінарних" файлів).
Однак те, що це _можна_ зробити, не означає, що це _потрібно_ зробити.

Much of Git's magic comes from being able to do line-by-line comparisons ("diffs") between files.
This is generally easy for programming source code and marked up text.
Для нетекстових файлів, diff зазвичай може виявляти лише те, що файли змінилися
але не можуть сказати, як і де.

Це по-різному впливає на продуктивність Git та ускладнює
порівняння різних версій вашого проєкту.

For a basic example to show the difference it makes,
we're going to go see what would have happened if Alfredo had tried
using outputs from a word processor instead of plain text.

Create a new directory and go into it:

```bash
$ mkdir recipes-nontext
$ cd recipes-nontext
```

Використовуйте таку програму, як Microsoft Word або LibreOffice Writer, щоб створити новий документ.
Введіть той самий текст, з якого ми починали раніше:

```output
# Ingredients
# Instructions
```

Save the document into the `recipes-nontext` directory with the name of `guacamole.doc`.
Поверніться в термінал та запустіть звичайні команди для налаштування нового репозиторію Git:

```bash
$ git init
$ git add guacamole.doc
$ git commit -m "Create a template for recipe"
```

Then make the same changes to `guacamole.doc` that we (or Alfredo) previously made to `guacamole.md`.

```output
# Ingredients
- avocado
- lemon
- salt
# Instructions
```

Збережіть зміни та закрийте текстовий процесор.
Тепер подивіться, що Git думає про них:

```bash
$ git diff
```

```output
diff --git a/guacamole.doc b/guacamole.doc
index 53a66fd..6e988e9 100644
Binary files a/guacamole.doc and b/guacamole.doc differ
```

Compare this to the earlier `git diff` obtained when using text files:

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..315bf3a 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,2 +1,5 @@
 # Ingredients
+- avocado
+- lemon
+- salt
 # Instructions
```

Зверніть увагу, що звичайні текстові файли дають набагато інформативніший diff.
Ви можете побачити, які саме лінії змінилися і які були зміни.

Неінформативний `git diff` не є єдиним наслідком використання Git на бінарних файлах.
However, most of the other problems boil down to whether or not a good diff is possible.

Це не означає, що ви _ніколи_ не повинні використовувати Git на бінарних файлах.
A rule of thumb is that it's OK if the binary file won't change very often,
and if it does change, you don't care about merging in small differences between versions.

We've already seen how a word processed report will fail this test.
An example that passes the test is a logo for your organization or project.
Even though a logo will be stored in a binary format such as `jpg` or `png`,
you can expect it will remain fairly static through the lifetime of your repository.
У тих рідкісних випадках, коли брендинг змінюється, ви, ймовірно, просто захочете повністю замінити логотип, а не зливати невеликі відмінності.

## Видалення файлу

Додавання та зміна файлів - це не єдині дії, які можна виконати
під час роботи над проєктом.  It might be required to remove a file
from the repository.

Створіть новий файл для невидимих чорнил:

```bash
$ echo "This is where we keep the secret sauce" > invisible.md
```

Now add to the repository like you have learned earlier:

```bash
$ git add invisible.md
$ git commit -m 'Add secret sauce'
$ git status
```

```output
On branch main
nothing to commit, working directory clean
```

Невидиме чорнило не є справжньою їжею.  Це була погана ідея.  Let us remove
it from the disk and let Git know about it:

```bash
$ git rm invisible.md
$ git status
```

```output
On branch main
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   deleted:    invisible.md

```

Зміна була перенесена у зону стейджингу.  Тепер збережіть ваш коміт та видаліть
файл із самого репозиторію.  Зауважте, що файл буде вилучено
у новому коміті.  Попередній коміт все одно
матиме файл, якщо ви хочете отримати цей конкретний коміт.

```bash
$ git commit -m 'Remove info on Invisible ink.  It is not an edible sauce!'
```

## Видалення файлу за допомогою Unix

Іноді ми можемо забути видалити файл через Git. Якщо ви видалили файл за допомогою команди Unix `rm` замість `git rm`, не хвилюйтеся —
Git досить розумний і помітить відсутній файл. Let us recreate the file and
commit it again.

```bash
$ echo "This is another way to make invisible ink" > secret.md
$ git add secret.md
$ git commit -m 'Add invisible ink again'
```

Now we remove the file with Unix `rm`:

```bash
$ rm secret.md
$ git status
```

```output
On branch main
Changes not staged for commit:
   (use "git add/rm <file>..." to update what will be committed)
   (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    secret.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Дивіться, як Git помітив, що файл `secret.md` був видалений
з диска.  Наступним кроком є "стейджинг" видалення файлу
з репозиторію.  Це робиться за допомогою команди `git rm` так само як і
раніше.

```bash
$ git rm secret.md
$ git status
```

```output
On branch main
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   deleted:    secret.md

```

The change that was made in Unix has now been staged and needs to be
committed.

```bash
$ git commit -m 'Remove info on invisible ink, again!'
```

## Перейменування файлу

Іншою поширеною зміною під час роботи над проєктом є перейменування файлу.

Створіть файл для рецепта білого соусу:

```bash
$ echo "Very fun recipe to do" > whitesauce.md
```

Додайте його до репозиторію:

```bash
$ git add whitesauce.md
$ git commit -m 'Add white sauce recipe'
```

We all know that white sauce has a more sophisticated name.

Змініть назву файлу з `whitesauce.md` на `bechamel.md` за допомогою Git:

```bash
$ git mv whitesauce.md bechamel.md
$ git status
```

```output
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	renamed:    whitesauce.md ->  bechamel.md
```

Останнім кроком є внесення змін до репозиторію:

```bash
$ git commit -m 'Use the French name for the whitesauce'
```

## Перейменування файлу за допомогою Unix

If you forgot to use Git and you used Unix `mv` instead
of `git mv`, you will have a touch more work to do but Git will
be able to deal with it. Let's try again renaming the file,
this time with Unix `mv`. По-перше, нам потрібно відтворити
файл `whitesauce.md`:

```bash
$ echo "Very fun recipe to do" > whitesauce.md
$ git add whitesauce.md
$ git commit -m 'Add white sauce recipe'
```

Тепер перейменуємо файл і подивимося, що Git може з'ясувати самостійно:

```bash
$ mv whitesauce.md bechamel.md
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    whitesauce.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    bechamel.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Git помітив, що файл `whitesauce.md` зник з
файлової системи та з'явився новий файл `bechamel.md`.

Додайте ці зміни в зону стейджингу:

```bash
$ git add whitesauce.md bechamel.md
$ git status
```

```output
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    whitesauce.md -> bechamel.md

```

Зверніть увагу, що тепер Git зрозумів, що `whitesauce.md` не
зник - його просто перейменували.

Останнім кроком, як і раніше, є внесення змін до репозиторію:

```bash
$ git commit -m 'Use the French name for the whitesauce'
```

## Further .gitignore concepts

Для отримання додаткової документації щодо .gitignore, будь ласка, зверніться до
[офіційної документації git](https://git-scm.com/docs/gitignore).

У вправі на ігнорування, слухачам було запропоновано два варіанти ігнорування вкладених файлів. Залежно від організації вашого репозиторію, одне рішення може підійти вам ліпше ніж інше. Keep in mind that the way that Git travels along
directory paths can be confusing.

Sometimes the `**` pattern comes in handy, too, which matches multiple
directory levels. Наприклад, `**/results/plots/*` would make git ignore the
`results/plots` directory in any root directory.

:::::::::::::::::::::::::::::::::::::::  challenge

## Ігнорування вкладених файлів: завдання

Враховуючи структуру каталогу, яка виглядає так:

```bash
results/data
results/plots
results/run001.log
results/run002.log
```

Та .gitignore, який виглядає так:

```output
*.csv
```

Як би ви відстежували весь вміст `results/data/`, включно з файлами `*.csv`, але ігноруючи решту `results/`?

:::::::::::::::  solution

## Рішення

Для цього ваш .gitignore буде виглядати так:

```output
*.csv                 #  ігноруйте файли .csv
results/*             # ігноруйте файли в директорії
!results/data/        # не ігноруйте файли в results/data
!results/data/*       # не ігноруйте файли .dat в results/data
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::
