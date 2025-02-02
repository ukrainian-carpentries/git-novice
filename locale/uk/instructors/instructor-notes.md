---
title: Нотатки для інструктора
---

Використання програмного інструменту для обробки версій файлів вашого проєкту дозволяє зосередитися на більш цікавих/інноваційних аспектах вашого проєкту.

- Переваги контролю версій
  - Його легко встановити
  - Кожна копія репозиторію Git є повною резервною копією проєкту та його історії
  - Кілька простих для запам'ятовування команд - це все, що потрібно для більшості повсякденних завдань з контролю версій
  - Послуга хостингу від [GitHub][github] надає вебплатформу для спільної роботи
- Два основних поняття
  - _commit_: збережений набір змін у файлах вашого проєкту
  - _repository_: історія всіх комітів вашого проєкту
- Навіщо використовувати GitHub?
  - Нема потреби в сервері: легко налаштувати
  - Численна спільнота GitHub: ваші колеги, напевно, вже там

## Загалом

Контроль версій може бути найважливішою темою, яку ми викладаємо, але Git, безумовно, є найскладнішим інструментом.  Однак, GitHub наразі домінує у сфері репозиторіїв відкритого програмного забезпечення, що робить час і зусилля для вивчення основ користування Git виправданими та цінними.

Через ці складнощі, ми не вчимо початківців багатьом цікавим темам, таким як гілки, хеші та об'єкти комітів.

Замість цього ми намагаємося переконати їх у тому, що контроль версій корисний для дослідників, які працюють в командах чи самостійно, бо це

- ліпший спосіб "скасувати" зміни,
- ліпший спосіб для співпраці, ніж розсилка файлів у різних напрямках, та
- ліпший спосіб поділитися своїм кодом та іншими науковими роботами зі світом.

## Teaching Notes

- Ви можете "розділити" ваш термінал так, щоб останні команди залишалися в полі зору за допомогою [цього](https://github.com/rgaiacs/swc-shell-split-window) скрипту.

- Переконайтеся, що мережа працює _до_ початку цього уроку.

- Малюнки особливо корисні у цьому уроці: якщо у вас є дошка, [користуйтеся нею][drawings]!

- Контроль версій, зазвичай, не перша тема, яка розглядається на семінарі, тому заохочуйте учнів створити обліковий запис GitHub заздалегідь, наприклад після попередньої сесії.
  Нагадайте слухачам, що ім\`я користувача та адреса електронної пошти, яку вони використовують для GitHub (та налаштовують під час конфігурації Git), за замовчуванням будуть наявні для загального перегляду.
  However, there are many reasons why a learner may not want their personal
  information viewable, and GitHub has [resources for keeping an email address
  private][github-privacy].

- Якщо деякі слухачі використовують Windows, неминуче виникнуть проблеми з об`єднанням файлів із різними закінченнями рядків.  (Even if everyone's on
    some flavor of Unix, different editors may or may not add a
    newline to the last line of a file.) Take a moment to explain
    these issues, since learners will almost certainly trip over them
    again.  If learners are running into line ending problems, GitHub
    has a [page][github-line-endings] that helps with troubleshooting.
    Specifically, the [section on refreshing a repository][github-line-endings-refresh]
    may be helpful if learners need to change the `core.autocrlf\` setting
  after already having made one or more commits.

- Ми не використовуємо Git GUI в цих нотатках, бо не знайшли графічний інтерфейс, який плавно встановлюється та надійно працює у трьох основних операційних системах. Крім того, нам треба, щоб учні розуміли основні команди, які виконуються.  Проте, інструкторам треба продемонструвати графічний інтерфейс на своєму комп'ютері в деякий момент цього уроку та вказати учням на [цю сторінку][github-gui].

- Instructors should show learners graphical diff/merge tools like
  [DiffMerge][diffmerge].

- When appropriate, explain that we teach Git rather than CVS, Subversion, or
  Mercurial primarily because of GitHub's growing popularity: CVS and
  Subversion are now seen as legacy systems, and Mercurial isn't nearly as
  widely used in the sciences right now.

- Додаткові ресурси:

  - [git-it] is a self-paced command-line Git demo,
    with [git-it-electron] its GitHub Desktop successor.
  - [Code School][code-school] has a free interactive course, [Try Git][try-git].
  - для викладачів корисною довідковою літературою є [Git parable][git-parable].

## [Автоматизований контроль версії](../episodes/01-basics.md)

- Ask, "Who uses 'undo' in their editor?" Всі скажуть: "Я". 'Undo' is the simplest
  form of version control.

- Give learners a five-minute overview of what version control does for them
  before diving into the watch-and-do practicals.  Most of them will have
  tried to co-author papers by emailing files back and forth, or will have
  biked into the office only to realize that the USB key with last night's
  work is still on the kitchen table.  Інструктори також можуть жартувати про каталоги з назвами на кшталт "остаточна версія", "переглянута остаточна версія", "остаточна версія з трьома виправленнями", "дійсно остаточна версія", та "це вже дійсно має бути остання версія", щоб заохочувати контроль версій як ліпший спосіб для співпраці та резервного копіювання роботи.

## [Налаштування Git](../episodes/02-setup.md)

- Ми радимо інструкторам та слухачам використовувати `nano` як текстовий редактор для цих уроків, тому що

  - він працює у всіх трьох основних операційних системах
  - він працює всередині терміналу (перемикання вікон може заплутати учнів), та
  - it has shortcut help at the bottom of the window.

  Please point out to students during setup that they can and should use
  another text editor if they're already familiar with it.

- Під час налаштування Git чітко вказуйте, що слухачі мають вводити: зазвичай вони редагують деталі викладача (наприклад, електронну пошту).  Зрештою, перевірте це за допомогою `git config --list`.

- При встановленні типової назви гілки, якщо слухачі мають версію Git старішу за 2.28, назву гілки для уроку можна змінити за замовчуванням за допомогою 'git branch -M main' - коли в репозиторії є коміти, або 'git checkout -b main' - коли комітів немає/репозиторій повністю порожній.

## [Створення репозиторію](../episodes/03-create.md)

- Коли ви вводите 'git status', користувачі Mac можуть побачити файл '.DS_Store', який відображується як не відстежуваний. Це файл, який Mac OS створює в кожному каталозі.

- Завдання "Місця для створення репозиторіїв" намагається посилити ідею, що каталог `.git` містить весь репозиторій Git і видалення цього каталогу скасовує `git init`. It also gives the learner the way to fix the common
  mistake of putting unwanted folders (like `Desktop`) under version control.

  Замість безпосереднього видалення каталогу `.git`, ви можете спочатку перемістити його в більш безпечну директорію і видалити його звідти:

  ```bash
  $ mv .git temp_git
  $ rm -rf  temp_git
  ```

  The challenge suggests that it is a bad idea to create a Git repo inside another repo.
  Для додаткової дискусії на цю тему, будь ласка, дивіться [це питання][repos-in-repos].

## [Відстеження змін](../episodes/04-changes.md)

- It's important that learners do a full commit cycle by themselves (make
  changes, `git diff`, `git add`, and `git commit`). Завдання "репозиторій `bio`" допоможе з цим.

- Це слушний момент, щоб показати diff за допомогою графічного інструмента. If you
  skip it because you're short on time, show it once in GitHub.

- One thing may cause confusion is recovering old versions.  Якщо замість команди `$ git checkout f22b25e mars.txt`, хтось введе `$ git checkout f22b25e`, то вони опиняться у стані "detached HEAD", що призведе до непорозуміння.
  It's then possible to keep on committing, but things like `git push origin main` a bit later will not give easily comprehensible results.  It also
  makes it look like commits can be lost.  Щоб "повторно прикріпити" HEAD, використовуйте `git checkout main`.

- This is a good moment to show a log within a Git GUI. If you skip it
  because you're short on time, show it once in GitHub.

## [Ігнорування файлів](../episodes/06-ignore.md)

Just remember that you can use wildcards and regular expressions to ignore a
particular set of files in `.gitignore`.

## [Віддалені репозиторії у GitHub](../episodes/07-github.md)

- Поясніть, що Git і GitHub - це не одне і те ж саме: Git - це інструмент контролю версій з відкритим кодом, GitHub - це компанія, яка розміщує Git репозиторії в Інтернеті та надає вебінтерфейс для взаємодії з репозиторіями, які вони розміщують.

- Дуже корисно намалювати діаграму, що показує різні залучені репозиторії.

- When pushing to a remote, the output from Git can vary slightly depending on
  what leaners execute. Урок відображує результат з git, якщо слухач виконує `git push origin main`. Однак, деякі слухачі можуть використовувати синтаксис `git push -u origin main`, що може запропонувати GitHub, для відправлення на наявний віддалений репозиторій. Слухачі, які використовують синтаксис від GitHub,
  `git push -u origin main`, матимуть дещо інший результат, в тому числі
  рядок `Branch main set up to track remote branch main from origin by rebasing.`

## [Співпраця](../episodes/08-collab.md)

- Вирішіть заздалегідь, чи всі учні працюватимуть в одному спільному репозиторії, або у парах (або інших невеликих групах) в окремих репозиторіях.  Перший варіант легше налаштувати; а другий працює більш плавно.

- Рольова гра між двома інструкторами може бути ефективним методом навчання співпраці та розв’язанню конфліктів.  Один інструктор може грати роль власника репозиторію, а другий інструктор може грати роль співавтора.  Якщо є можливість, спробуйте використовувати два проєктори, щоб було видно компʼютерні екрани обох інструкторів.  This makes for
  a very clear illustration to the students as to who does what.

- It is also effective to pair up students during this lesson and assign one
  member of the pair to take the role of the owner and the other the role of
  the collaborator.  In this setup, challenges can include asking the
  collaborator to make a change, commit it, and push the change to the remote
  repository so that the owner can then retrieve it, and vice-versa.  The
  role playing between the instructors can get a bit "dramatic" in the
  conflicts part of the lesson if the instructors want to inject some humor
  into the room.

- If you don't have two projectors, have two instructors at the front of the
  room.  Each instructor does their piece of the collaboration demonstration
  on their own computer and then passes the projector cord back and forth
  with the other instructor when it's time for them to do the other part of
  the collaborative workflow.  It takes less than 10 seconds for each
  switchover, so it doesn't interrupt the flow of the lesson.
  And of course it helps to give each of the instructors a different-colored
  hat, or put different-colored sticky notes on their foreheads.

- If you're the only instructor, the best way to create is clone the two
  repos in your Desktop, but under different names, e.g., pretend one is your
  computer at work:

  ```bash
  $ git clone https://github.com/vlad/planets.git planets-at-work
  ```

- It's very common that learners mistype the remote alias or the remote URL
  when adding a remote, so they cannot `push`. You can diagnose this with
  `git remote -v` and checking carefully for typos.

  - Щоб виправити помилковий псевдонім, скористайтеся командою `git remote rename <old> <new>`.
  - Щоб виправити помилковий URL, ви можете виконати `git remote set-url <alias> <newurl>`.

- Before cloning the repo, be sure that nobody is inside another repo. Найкращий спосіб досягти цього - перейти на робочий стіл перед клонуванням: `cd && cd Desktop`.

- If both repos are in the `Desktop`, have them to clone their collaborator
  repo under a given directory using a second argument:

  ```bash
  $ git clone https://github.com/vlad/planets.git vlad-planet
  ```

- The most common mistake is that learners `push` before `pull`ing. If they
  `pull` afterward, they may get a conflict.

- Іноді можуть виникнути дивні конфлікти. Зберігайте спокій: конфлікти розглядаються у наступному епізоді.

- Learners may have slightly different output from `git push` and `git pull`
  depending on the version of git, and if upstream (`-u`) is used.

## [Конфлікти](../episodes/09-conflict.md)

- Очікуйте, що учні зроблять помилки. Очікуйте, що ви можете зробити помилки.
  Це відбувається тому, що урок триває вже достатньо довго і всі втомилися.

- If you're the only instructor, the best way to create a conflict is:

  - Clone your repo in a different directory, pretending is your computer at
    work: `git clone https://github.com/alflin/recipes.git recipes-at-work`.
  - At the office, you make a change, commit and push.
  - At your laptop repo, you (forget to pull and) make a change, commit and
    try to push.
  - Тепер введіть `git pull` та покажіть як виглядає конфлікт.

- Учні зазвичай забувають `git add` файл після виправлення конфлікту та просто (намагаються) зробити коміт. Ви можете це продіагностувати за допомогою `git status`.

- Remember that you can discard one of the two parents of the merge:

  - discard the remote file, `git checkout --ours conflicted_file.txt`
  - відкинути локальний файл, `git checkout --theirs conflicted_file.txt`

  You still have to `git add` and `git commit` after this. This is
  particularly useful when working with binary files.

- Майте на увазі, що в залежності від версії Git, яку ви використовуєте, результати для `git push` та `git pull` можуть дещо відрізнятися.

## [Відкрита наука](../episodes/10-open.md)

## [Ліцензування](../episodes/11-licensing.md)

We teach about licensing because questions about who owns what, or can use
what, arise naturally once we start talking about using public services like
GitHub to store files. Крім того, обговорення дає учням можливість перевести подих після тривалого навантаження.

The Creative Commons family of licenses is recommended for many types of
works (including software documentation and images used in software) but not
software itself. Creative Commons [recommends][cc-faq-software] a
software-specific license instead.

## [Цитування](../episodes/12-citation.md)

## [Хостинг](../episodes/13-hosting.md)

A common concern for learners is having their work publicly available on
GitHub.  Хоча ми заохочуємо відкриту науку, іноді приватні репозиторії є єдиним варіантом. Завжди цікаво згадати варіанти розміщення приватних репозиторіїв.

[github]: https://github.com/
[drawings]: https://marklodato.github.io/visual-git-guide/index-en.html
[github-privacy]: https://help.github.com/articles/keeping-your-email-address-private/
[github-line-endings]: https://docs.github.com/en/github/using-git/configuring-git-to-handle-line-endings
[github-line-endings-refresh]: https://docs.github.com/en/github/using-git/configuring-git-to-handle-line-endings#refreshing-a-repository-after-changing-line-endings
[github-gui]: https://git-scm.com/downloads/guis
[diffmerge]: https://sourcegear.com/diffmerge/
[git-it]: https://github.com/jlord/git-it
[git-it-electron]: https://github.com/jlord/git-it-electron
[code-school]: https://www.codeschool.com/
[try-git]: https://try.github.io
[git-parable]: https://tom.preston-werner.com/2009/05/19/the-git-parable.html
[repos-in-repos]: https://github.com/swcarpentry/git-novice/issues/272
[cc-faq-software]: https://creativecommons.org/faq/#can-i-apply-a-creative-commons-license-to-software
