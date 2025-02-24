---
title: Відстеження змін
teaching: 20
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Пройти цикл зміни-додавання-коміту для одного або декількох файлів.
- Пояснити де зберігається інформація на кожному етапі цього циклу.
- Пояснити різницю між інформативними та неінформативними повідомленнями комітів.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Як зберегти зміни в Git?
- Як перевірити стан свого репозиторію?
- Як зробити нотатки про те, які зміни було внесено і чому?

::::::::::::::::::::::::::::::::::::::::::::::::::

Спочатку переконаймося, що ми все ще у правильному каталозі.
Ви повинні знаходитися у каталозі `recipes`.

```bash
$ cd ~/Desktop/recipes
```

Let's create a file called `guacamole.md` that contains the basic structure of our first recipe.
Ми будемо використовувати редактор `nano` для редагування файлу; ви можете використовувати будь-який редактор, який вам подобається.
Зокрема, це не обовʼязково повинен бути `core.editor`, який ви раніше вказали глобально. Але пам'ятайте, що команда bash для створення або редагування нового файлу буде залежати від обраного редактора (який не обов'язково буде `nano`). Для довідки щодо текстових редакторів, дивіться ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create.html#which-editor/) в уроці [The Unix Shell](https://swcarpentry.github.io/shell-novice/).

```bash
$ nano guacamole.md
```

Введіть наведений нижче текст у файл `guacamole.md`:

```output
# Guacamole
## Ingredients
## Instructions
```

Збережіть файл і вийдіть з редактора. Далі переконайтеся, що файл був створений належним чином, запустивши команду `ls`:

```bash
$ ls
```

```output
guacamole.md
```

`guacamole.md` містить три рядки, які ми можемо побачити, запустивши:

```bash
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
## Instructions
```

Якщо ми знову перевіримо статус нашого проєкту, Git повідомляє нам, що він помітив новий файл:

```bash
$ git status
```

```output
On branch main

No commits yet

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	guacamole.md

nothing added to commit but untracked files present (use "git add" to track)
```

Повідомлення "untracked files" означає, що в каталозі існує файл, який Git не відстежує.
Ми можемо повідомити Git, що цей файл треба відстежувати за допомогою команди `git add`:

```bash
$ git add guacamole.md
```

та згодом переконатися, що все виглядає правильно:

```bash
$ git status
```

```output
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   guacamole.md

```

Git тепер знає, що він повинен стежити за файлом `guacamole.md`, але він ще не записав ці зміни як коміт.
Щоб зробити це, нам потрібно виконати ще одну команду:

```bash
$ git commit -m "Create initial structure for a Guacamole recipe"
```

```output
[main (root-commit) f22b25e] Create initial structure for a Guacamole recipe
 1 file changed, 1 insertion(+)
 create mode 100644 guacamole.md
```

Коли ми запускаємо `git commit`, Git бере все, що ми раніше запросили його зберегти за допомогою `git add`, та зберігає постійну копію цих змін у спеціальному каталозі `.git`.
Ця постійна копія називається [commit](../learners/reference.md#commit)
(або [revision](../learners/reference.md#revision)). У цьому прикладі коміт має скорочений ідентифікатор `f22b25e`. У вашого коміту може бути інший ідентифікатор.

Ми використовуємо команду `-m` (від "message") щоб надати короткий, інформативний та конкретний коментар, який допоможе нам згадати пізніше про те, що ми зробили та чому.
Якщо ми просто запустимо `git commit` без опції `-m`, Git запустить `nano` (або будь-який інший редактор, який ми вказали як `core.editor`), щоб ми могли написати довше повідомлення.

[Гарні повідомлення комітів][commit-messages] починаються з короткого (\< 50 символів) тексту про зміни, внесені в коміт. Загалом, повідомлення має завершувати речення "If applied, this commit will" <commit message here> ("Якщо застосовано, цей коміт буде" <0>).
Якщо ви хочете детальніше перейти до подробиць, додайте порожній рядок між першим рядком та вашими додатковими примітками. Використовуйте додаткові примітки, щоб пояснити, чому ви внесли зміни та/або яким буде їх вплив.

Якщо ми тепер запустимо `git status`:

```bash
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

Git говорить нам, що поточний стан файлів відповідає їх стану, який збережений у репозиторії.
Якщо ми хочемо знати, що саме ми зробили нещодавно - ми можемо попросити Git показати нам історію проєкту, використовуючи `git log`:

```bash
$ git log
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Create initial structure for a Guacamole recipe
```

`git log` виводить перелік усіх комітів, які були внесені до репозиторію, у зворотному хронологічному порядку.
Для кожного коміту буде надруковано повний ідентифікатор коміту (який починається з тих же символів, що і скорочений ідентифікатор, попередньо надрукований командою `git commit`), автор коміту, дата його створення, і повідомлення Git, яке було додано під час запису коміту.

:::::::::::::::::::::::::::::::::::::::::  callout

## Де зберігаються мої зміни?

Якщо ми тепер запустимо `ls`, ми все одно побачимо лише один файл, який називається `guacamole.md`.
Це відбувається тому, що Git зберігає інформацію про історію файлів у спеціальному каталозі `.git`, згаданому раніше, щоб наша файлова система не засмічувалася (і щоб ми випадково не могли змінити або видалити стару версію).

::::::::::::::::::::::::::::::::::::::::::::::::::

Тепер припустимо, що Альфредо додає нову інформацію до файлу.
(Знову ж таки, ми будемо редагувати його за допомогою `nano`, і потім перевіряти його зміст за допомогою `cat`; ви можете користуватися іншим редактором, та можете не використовувати `cat`.)

```bash
$ nano guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado
* lemon
* salt
## Instructions
```

Тепер, коли ми запускаємо `git status`, Git повідомляє нам, що файл, про який він вже знає, був змінений:

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

Ключова фраза - це останній рядок: "no changes added to commit".
Ми змінили цей файл, але ми ще не зберегли їх (що ми робимо за допомогою `git commit`), та навіть не сказали Git, що ми маємо намір зберегти ці зміни у майбутньому (що ми робимо за допомогою `git add`).
Тож зробімо це зараз. Хорошою рекомендацією є перегляд наших змін кожного разу перед їх збереженням. Це робиться за допомогою `git diff`.
Результат показує нам відмінності між поточним станом файлу та його останньою збереженою версією:

```bash
$ git diff
```

```output
diff --git a/guacamole.md b/guacamole.md
index df0654a..315bf3a 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,3 +1,6 @@
 # Guacamole
 ## Ingredients
+* avocado
+* lemon
+* salt
 ## Instructions
```

Результат цієї команди важко зрозуміти, тому що це насправді серія команд для таких інструментів, як редактори або `patch`, яка повідомляє їм, як змінити один файл за допомогою іншого.
Якщо розділити цей результат на фрагменти:

1. Перший рядок вказує на те, що результат цієї команди у Git подібний до Unix команди `diff`, яка порівнює стару та нову версії файлу.
2. Другий рядок повідомляє які саме версії файлу Git порівнює; `df0654a` та `315bf3a` є унікальними ідентифікаторами цих версій.
3. Третій та четвертий рядки ще раз показують назву файлу, що змінюється.
4. Решта рядків найцікавіші, вони показують нам фактичні відмінності і рядки, у яких вони відбуваються.
   Зокрема, значок `+` в першому стовпці вказує де ми додали рядок.

Після того, як ми переглянули наші зміни, прийшов час зберегти їх:

```bash
$ git commit -m "Add ingredients for basic guacamole"
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)

	modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Але це не спрацює: Git не буде додавати зміни, тому що ми не використали спочатку `git add`.
Давайте це виправимо:

```bash
$ git add guacamole.md
$ git commit -m "Add ingredients for basic guacamole"
```

```output
[main 34961b1] Add ingredients for basic guacamole
 1 file changed, 3 insertions(+)
```

Git наполягає, щоб ми додали файли до набору змін, які ми хочемо записати, перед тим як ми зробимо коміт. Це дозволяє зберігати зміни поступово та обʼєднувати їх у логічні блоки, аніж у великі набори змін.
Наприклад, припустимо, ми робимо коміт кількох цитат відповідних досліджень у нашій дисертації.
Можливо, ми бажаємо зберегти ці зміни, та відповідні записи у бібліографії, але не зберігати деякі інші зміни в нашій роботі (наприклад, висновок, який ми ще не закінчили).

Щоб це було можливо зробити, Git має спеціальну _зону стейджингу_ (staging area), де він відстежує речі, які були додані до поточного [набору змін (changeset)](../learners/reference.md#changeset) проте, ще не були збережені.

:::::::::::::::::::::::::::::::::::::::::  callout

## Зона стейджингу (staging area)

Якщо ви будете уявляти, ніби Git робить знімки змін протягом життя проєкту, то `git add` вказує що буде на знімку (додаючи речі в зоні стейджингу), а `git commit` після того насправді робить знімок, та назавжди зберігає його (як коміт).
Якщо у зоні стейджингу нічого немає, то коли ви введете `git commit`, Git запропонує вам використати `git commit -a` або `git commit --all`, який ніби збирає разом всіх, щоб зробити групове фото!
Однак майже завжди краще явним чином додати речі до зони стейджингу, тому що без цього ви можете випадково зберегти інші зміни, про які ви забули. (Повертаючись до порівняння з груповим фото, якщо ви використали команду "-а", до вашої фотографії може потрапити зайва людина!)
Тому додавайте речі в зону стейджингу власноруч - > в іншому випадку вам може знадобитися шукати як використовувати "git undo commit" частіше, ніж вам хотілося б!

::::::::::::::::::::::::::::::::::::::::::::::::::

![](fig/git-staging-area.svg){alt='Додавання змін до зони стейджингу за допомогою "git add" та зберігання їх у репозиторії за допомогою "git commit"'}

Подивімося, як наші зміни у файлі проходять шлях від текстового редактора до зони стейджингу і далі у довгострокове зберігання.
По-перше, ми покращимо наш рецепт, змінивши лимон на лайм:

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
```

```bash
$ git diff
```

```output
diff --git a/guacamole.md b/guacamole.md
index 315bf3a..b36abfd 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,6 +1,6 @@
 # Guacamole
 ## Ingredients
 * avocado
-* lemon
+* lime
 * salt
 ## Instructions
```

Поки що все добре: ми замінили один рядок (позначений `-`) на новий рядок (позначений `+`).
Тепер помістімо цю зміну у зону стейджингу та подивимося що після цього звітує `git diff`:

```bash
$ git add guacamole.md
$ git diff
```

Результату немає: це виглядає ніби для Git немає різниці між тим, що вже було збережено назавжди та тим, що зараз міститься у робочій директорії.
Проте, якщо ми зробимо наступне:

```bash
$ git diff --staged
```

```output
diff --git a/guacamole.md b/guacamole.md
index 315bf3a..b36abfd 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,6 +1,6 @@
 # Guacamole
 ## Ingredients
 * avocado
-* lemon
+* lime
 * salt
 ## Instructions
```

то ми побачимо різницю між останніми збереженими змінами та тими, які знаходяться в зоні стейджингу.
Збережімо наші зміни:

```bash
$ git commit -m "Modify guacamole to the traditional recipe"
```

```output
[main 005937f] Modify guacamole to the traditional recipe
 1 file changed, 1 insertion(+)
```

Перевіримо наш статус:

```bash
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

і подивимося на історію попередніх змін:

```bash
$ git log
```

```output
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Modify guacamole to the traditional recipe

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add ingredients for basic guacamole

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Create initial structure for a Guacamole recipe
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Порівняння з підсвіткою змінених слів у рядках

Інколи, наприклад, у випадку текстових документів, результат `diff` дуже важко зрозуміти. Саме тут `--color-words` опція для `git diff` є надзвичайно зручною, бо вона виділяє кольором змінені слова.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Перегляд історії змін за сторінками

Коли розмір результату `git log` перевищує розмір вашого екрану, `git` використовує спеціальну програму "пейджер", щоб поділити результат на сторінки розміром з ваш екран.
Після виклику "пейджера", ви помітите, що останній рядок на екрані - це `:`, замість звичайного запрошення командного рядка.

- Натисніть <kbd>Q</kbd>, щоб вийти з пейджеру.
- Натисніть <kbd>пробіл</kbd>, щоб перейти на наступну сторінку.
- Щоб шукати `some_word` на всіх сторінках, натисніть <kbd>/</kbd> і введіть `some_word`.
  Натисніть <kbd>N</kbd> для навігації між результатами пошуку.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Обмеження розміру зображуваної історії змін

Щоб запобігти випадку, коли `git log` повністю займає ваш термінал, ви можете обмежувати кількість комітів які відображує Git, використовуючи опцію `-N`, де `N` - кількість комітів які б ви бажали бачити на екрані. Наприклад, якщо ви бажаєте побачити лише останній коміт, використовуйте команду

```bash
$ git log -1
```

```output
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:14:07 2013 -0400

   Modify guacamole to the traditional recipe
```

Ви також можете зменшити кількість інформації, використовуючи опцію `--oneline`:

```bash
$ git log --oneline
```

```output
005937f (HEAD -> main) Modify guacamole to the traditional recipe
34961b1 Add ingredients for basic guacamole
f22b25e Create initial structure for a Guacamole recipe
```

Ви також можете комбінувати опцію `--oneline` з іншими опціями. Наступна корисна комбінація використовує опцію `--graph` для графічного зображення історії комітів за допомогою псевдографіки, вказуючи при цьому які коміти пов'язані з поточним `HEAD`, поточною гілкою `main`, або [іншими обʼєктами у репозиторії][git-references]':

```bash
$ git log --oneline --graph
```

```output
* 005937f (HEAD -> main) Modify guacamole to the traditional recipe
* 34961b1 Add ingredients for basic guacamole
* f22b25e Create initial structure for a Guacamole recipe
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Каталоги

Дві важливі речі, які ви повинні знати про каталоги в Git.

1. Git не відстежує каталоги самостійно, тільки файли всередині них.
   Спробуйте власноруч:

```bash
$ mkdir cakes
$ git status
$ git add cakes
$ git status
```

Зауважте, наш новостворений порожній каталог `cakes` не зʼявляється в переліку невідстежуваних файлів, навіть якщо ми конкретно додали його (_через_ `git add`) до нашого репозиторію. Ось чому ви іноді бачите файли `.gitkeep` в інших порожніх каталогах. The sole purpose of `.gitkeep` files is to populate a directory so that Git adds it to the repository. The name `.gitkeep` is just a convention, and in fact, you can name these files anything you like.

2. If you create a directory in your Git repository and populate it with files,
   you can add all the files in the directory at once by referring to the directory in your `git add` command. Try it for yourself:

```bash
$ touch cakes/brownie_cakes/lemon_drizzle
$ git status
$ git add cakes
$ git status
```

Before moving on, we will commit these changes.

```bash
$ git commit -m "Add some initial cakes"
```

::::::::::::::::::::::::::::::::::::::::::::::::::

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![](fig/git-committing.svg){alt='A diagram showing two documents being separately staged using git add, before being combined into one commit using git commit'}

:::::::::::::::::::::::::::::::::::::::  challenge

## Choosing a Commit Message

Which of the following commit messages would be most appropriate for the
last commit made to `guacamole.md`?

1. "Changes"
2. "Changed lemon for lime"
3. "Guacamole modified to the traditional recipe"

:::::::::::::::  solution

## Solution

Answer 1 is not descriptive enough, and the purpose of the commit is unclear;
and answer 2 is redundant to using "git diff" to see what changed in this commit;
but answer 3 is good: short, descriptive, and imperative.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Committing Changes to Git

Which command(s) below would save the changes of `myfile.txt`
to my local Git repository?

1. ```bash
   $ git commit -m "my recent changes"
   ```
2. ```bash
   $ git init myfile.txt
   $ git commit -m "my recent changes"
   ```
3. ```bash
   $ git add myfile.txt
   $ git commit -m "my recent changes"
   ```
4. ```bash
   $ git commit -m myfile.txt "my recent changes"
   ```

:::::::::::::::  solution

## Solution

1. Would only create a commit if files have already been staged.

2. Would try to create a new repository.

3. Is correct: first add the file to the staging area, then commit.

4. Would try to commit a file "my recent changes" with the message myfile.txt.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Committing Multiple Files

The staging area can hold changes from any number of files
that you want to commit as a single snapshot.

1. Add some text to `guacamole.md` noting the rough price of the
   ingredients.
2. Create a new file `groceries.md` with a list of products and
   their prices for different markets.
3. Add changes from both files to the staging area,
   and commit those changes.

:::::::::::::::  solution

## Solution

First we make our changes to the `guacamole.md` and `groceries.md` files:

```bash
$ nano guacamole.md
$ cat guacamole.md
```

```output
# Guacamole
## Ingredients
* avocado (1.35)
* lime (0.64)
* salt (2)
```

```bash
$ nano groceries.md
$ cat groceries.md
```

```output
# Market A
* avocado: 1.35 per unit.
* lime: 0.64 per unit
* salt: 2 per kg
```

Now you can add both files to the staging area. We can do that in one line:

```bash
$ git add guacamole.md groceries.md
```

Or with multiple commands:

```bash
$ git add guacamole.md
$ git add groceries.md
```

Now the files are ready to commit. You can check that using `git status`. If you are ready to commit use:

```bash
$ git commit -m "Write prices for ingredients and their source"
```

```output
[main cc127c2]
 Write prices for ingredients and their source
 2 files changed, 7 insertions(+)
 create mode 100644 groceries.md
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## `bio` Repository

- Create a new Git repository on your computer called `bio`.
- Write a three-line biography for yourself in a file called `me.txt`,
  commit your changes
- Modify one line, add a fourth line
- Display the differences
  between its updated state and its original state.

:::::::::::::::  solution

## Solution

If needed, move out of the `recipes` folder:

```bash
$ cd ..
```

Create a new folder called `bio` and 'move' into it:

```bash
$ mkdir bio
$ cd bio
```

Initialise git:

```bash
$ git init
```

Create your biography file `me.txt` using `nano` or another text editor.
Once in place, add and commit it to the repository:

```bash
$ git add me.txt
$ git commit -m "Add biography file"
```

Modify the file as described (modify one line, add a fourth line).
To display the differences
between its updated state and its original state, use `git diff`:

```bash
$ git diff me.txt
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

[commit-messages]: https://chris.beams.io/posts/git-commit/
[git-references]: https://git-scm.com/book/en/v2/Git-Internals-Git-References

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git status` shows the status of a repository.
- Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded).
- `git add` puts files in the staging area.
- `git commit` saves the staged content as a new commit in the local repository.
- Write a commit message that accurately describes your changes.

::::::::::::::::::::::::::::::::::::::::::::::::::
