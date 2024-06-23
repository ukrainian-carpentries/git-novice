---
title: Обговорення
---

## Поширені запитання

People often have questions about Git beyond the scope of the core material.
Студентам, які завершили решту уроків, може бути корисно розглянути наступні теми.

Зауважте, що оскільки цей матеріал не є обов'язковим для базового використання Git, він не буде розглянутий інструктором.

## Додаткові налаштування Git

Під час [Налаштування Git](../episodes/02-setup.md), ми використовували `git config --global`, щоб встановити деякі параметри за замовчуванням для Git.
It turns out that these configuration options get stored in your home directory
in a plain text file called `.gitconfig`.

```bash
$ cat ~/.gitconfig
```

```output
[user]
	name = Vlad Dracula
	email = vlad@tran.sylvan.ia
[color]
	ui = true
[core]
	editor = nano
```

This file can be opened in your preferred text editor.
(Note that it is recommended to continue using the `git config` command,
as this helps avoid introducing syntax errors.)

Eventually, you will want to start customizing Git's behaviour.
Це можна зробити, додавши більше записів до вашого `.gitconfig`.
The available options are described in the manual:

```bash
$ git config --help
```

Зокрема, вам може знадобитися додати псевдоніми.
Це щось на кшталт скорочень для довших команд Git.
Наприклад, якщо вам набридло постійно вводити `git checkout`,
ви можете виконати команду:

```bash
$ git config --global alias.co checkout
```

Now if we return to the example from [Exploring History](../episodes/05-history.md) where we ran:

```bash
$ git checkout f22b25e mars.txt
```

we could now instead type:

```bash
$ git co f22b25e mars.txt
```

## Стилізація журналу Git

A good target for customization is output from the log.
The default log is quite verbose but gives no graphical hints
such as information about which commits were done locally
and which were pulled from remotes.

You can use `git log --help` and `git config --help` to look for different ways to change
the log output.
Try the following commands and see what effect they have:

```bash
$ git config --global alias.lg "log --graph"
$ git config --global log.abbrevCommit true
$ git config --global format.pretty oneline
$ git lg
```

If you don't like the effects,
you can undo them with:

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
Sort them by the number of stars and have a look at the top few.
If you find some you like,
please check that they're covered by an open source license before you clone them.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Нетекстові файли

Recall when we discussed [Conflicts](../episodes/09-conflict.md)
there was a challenge that asked,
"What does Git do
when there is a conflict in an image or some other non-textual file
that is stored in version control?"

Тепер ми розглянемо це питання більш детально.

Багато людей бажають керувати версіями нетекстових файлів, таких як зображення, PDF-файли та документи Microsoft Office або LibreOffice.
It is true that Git can handle these filetypes (which fall under the banner of "binary" file types).
However, just because it _can_ be done doesn't mean it _should_ be done.

Much of Git's magic comes from being able to do line-by-line comparisons ("diffs") between files.
This is generally easy for programming source code and marked up text.
Для нетекстових файлів, diff зазвичай може виявляти лише те, що файли змінилися
але не можуть сказати, як і де.

Це по-різному впливає на продуктивність Git та ускладнює
порівняння різних версій вашого проєкту.

For a basic example to show the difference it makes,
we're going to go see what would have happened if Dracula had tried
using outputs from a word processor instead of plain text.

Створіть нову директорію і перейдіть до неї:

```bash
$ mkdir planets-nontext
$ cd planets-nontext
```

Використовуйте таку програму, як Microsoft Word або LibreOffice Writer, щоб створити новий документ.
Введіть той самий текст, з якого ми починали раніше:

```output
Cold and dry, but everything is my favorite color
```

Save the document into the `planets-nontext` directory with the name of `mars.doc`.
Back in the terminal, run the usual commands for setting up a new Git repository:

```bash
$ git init
$ git add mars.doc
$ git commit -m "Starting to think about Mars"
```

Then make the same changes to `mars.doc` that we (or Vlad) previously made to `mars.txt`.

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
```

Save and close the word processor.
Now see what Git thinks of your changes:

```bash
$ git diff
```

```output
diff --git a/mars.doc b/mars.doc
index 53a66fd..6e988e9 100644
Binary files a/mars.doc and b/mars.doc differ
```

Compare this to the earlier `git diff` obtained when using text files:

```output
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
```

Зверніть увагу, що звичайні текстові файли дають набагато інформативніший diff.
Ви можете побачити, які саме лінії змінилися і які були зміни.

An uninformative `git diff` is not the only consequence of using Git on binary files.
However, most of the other problems boil down to whether or not a good diff is possible.

This isn't to say you should _never_ use Git on binary files.
A rule of thumb is that it's OK if the binary file won't change very often,
and if it does change, you don't care about merging in small differences between versions.

Ми вже бачили, як текстовий оброблений звіт провалить цей тест.
Прикладом, який проходить перевірку, є логотип для вашої організації або проєкту.
Even though a logo will be stored in a binary format such as `jpg` or `png`,
you can expect it will remain fairly static through the lifetime of your repository.
On the rare occasion that branding does change,
you will probably just want to replace the logo completely rather than merge little differences in.

## Видалення файлу

Adding and modifying files are not the only actions one might take
when working on a project.  It might be required to remove a file
from the repository.

Створіть новий файл для планети Nibiru:

```bash
$ echo "This is another name for fake planet X" > nibiru.txt
```

Now add to the repository like you have learned earlier:

```bash
$ git add nibiru.txt
$ git commit -m 'adding info on nibiru'
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

Nibiru is not a real planet.  That was a silly idea.  Let us remove
it from the disk and let Git know about it:

```bash
$ git rm nibiru.txt
$ git status
```

```output
On branch main
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   deleted:    nibiru.txt

```

Зміна була перенесена у зону стейджингу.  Now commit the removal, and remove the
file from the repository itself.  Зауважте, що файл буде вилучено
у новому коміті.  Попередній коміт все одно
матиме файл, якщо ви хочете отримати цей конкретний коміт.

```bash
$ git commit -m 'Removing info on Nibiru.  It is not a real planet!'
```

## Видалення файлу за допомогою Unix

Іноді ми можемо забути видалити файл через Git. If you removed the
file with Unix `rm` instead of using `git rm`, no worries,
Git is smart enough to notice the missing file. Let us recreate the file and
commit it again.

```bash
$ echo "This is another name for fake planet X" > nibiru.txt
$ git add nibiru.txt
$ git commit -m 'adding nibiru again'
```

Now we remove the file with Unix `rm`:

```bash
$ rm nibiru.txt
$ git status
```

```output
On branch main
Changes not staged for commit:
   (use "git add/rm <file>..." to update what will be committed)
   (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    nibiru.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Дивіться, як Git помітив, що файл `nibiru.txt` був видалений
з диска.  Наступним кроком є "стейджинг" видалення файлу
з репозиторію.  This is done with the command `git rm` just as
before.

```bash
$ git rm nibiru.txt
$ git status
```

```output
On branch main
Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   deleted:    nibiru.txt

```

The change that was made in Unix has now been staged and needs to be
committed.

```bash
$ git commit -m 'Removing info on Nibiru, again!'
```

## Перейменування файлу

Another common change when working on a project is to rename a file.

Create a file for the planet Krypton:

```bash
$ echo "Superman's home planet" > krypton.txt
```

Додайте його до репозиторію:

```bash
$ git add krypton.txt
$ git commit -m 'Adding planet Krypton'
```

We all know that Superman moved to Earth.  Not that he had much
choice.  Now his home planet is Earth.

Rename the file `krypton.txt` to `earth.txt` with Git:

```bash
$ git mv krypton.txt earth.txt
$ git status
```

```output
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	renamed:    krypton.txt -> earth.txt
```

Останнім кроком є внесення змін до репозиторію:

```bash
$ git commit -m 'Superman's home is now Earth'
```

## Renaming a File with Unix

If you forgot to use Git and you used Unix `mv` instead
of `git mv`, you will have a touch more work to do but Git will
be able to deal with it. Let's try again renaming the file,
this time with Unix `mv`. First, we need to recreate the
`krypton.txt` file:

```bash
$ echo "Superman's home planet" > krypton.txt
$ git add krypton.txt
$ git commit -m 'Adding planet Krypton again.'
```

Let us rename the file and see what Git can figured out by itself:

```bash
$ mv krypton.txt earth.txt
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    krypton.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    earth.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Git has noticed that the file `krypton.txt` has disappeared from the
file system and a new file `earth.txt` has showed up.

Add those changes to the staging area:

```bash
$ git add krypton.txt earth.txt
$ git status
```

```output
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    krypton.txt -> earth.txt

```

Notice how Git has now figured out that the `krypton.txt` has not
disappeared - it has simply been renamed.

The final step, as before, is to commit our change to the repository:

```bash
$ git commit -m 'Superman's home is Earth, told you before.'
```

## Further .gitignore concepts

For additional documentation on .gitignore, please reference
[the official git documentation](https://git-scm.com/docs/gitignore).

In the ignore exercise, learners were presented with two variations of ignoring
nested files. Depending on the organization of your repository, one may suit
your needs over another. Keep in mind that the way that Git travels along
directory paths can be confusing.

Sometimes the `**` pattern comes in handy, too, which matches multiple
directory levels. E.g. `**/results/plots/*` would make git ignore the
`results/plots` directory in any root directory.

:::::::::::::::::::::::::::::::::::::::  challenge

## Ignoring Nested Files: Challenge Problem

Given a directory structure that looks like:

```bash
results/data
results/plots
results/run001.log
results/run002.log
```

And a .gitignore that looks like:

```output
*.csv
```

How would you track all of the contents of `results/data/`, including `*.csv`
files, but ignore the rest of `results/`?

:::::::::::::::  solution

## Solution

To do this, your .gitignore would look like this:

```output
*.csv                 # ignore the .csv files
results/*             # ignore the files in the results directory
!results/data/        # do not ignore the files in results/data
!results/data/*       # do not ignore the .csv files in reults/data
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::
