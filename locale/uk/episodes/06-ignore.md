---
title: Ігнорування файлів
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Налаштувати Git для ігнорування певних файлів.
- Зрозуміти чому ігнорування файлів може бути корисним.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Як зробити так, щоб Git ігнорував файли, які я не хочу відстежувати?

::::::::::::::::::::::::::::::::::::::::::::::::::

Що робити, якщо у нас є файли, які ми не хочемо відстежувати у Git, наприклад, резервні файли, створені нашим редактором або проміжні файли, створені під час аналізу даних?
Створімо декілька фіктивних файлів:

```bash
$ mkdir receipts
$ touch a.png b.png c.png receipts/a.jpg receipts/b.jpg
```

і подивимося, що скаже Git:

```bash
$ git status
```

```output
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.png
	b.png
	c.png
	receipts/

nothing added to commit but untracked files present (use "git add" to track)
```

Відстеження цих файлів за допомогою контролю версій буде марною тратою місця на диску.
Що гірше, висвітлення змін в них під час перегляду історії або статусу проєкту може відвернути нас від змін, які насправді важливі, Тож скажімо Git, що ці файли треба ігнорувати.

Ми можемо зробити це, створюючи у кореневому каталозі нашого проєкту файл під назвою `.gitignore`:

```bash
$ nano .gitignore
$ cat .gitignore
```

```output
*.png
receipts/
```

Ці шаблони наказують Git ігнорувати будь-який файл, ім’я якого закінчується на `.png`, і все в каталозі `receipts`.
(Якщо будь-які з цих файлів вже відстежуються, то Git продовжить їх відстежувати.)

Як тільки ми створили цей файл, результат команди `git status` стає набагато зрозумілішим:

```bash
$ git status
```

```output
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

Єдина річ, яку Git помічає зараз - це новостворений файл `.gitignore`.
Ви можете подумати, що його не треба відстежувати, але всі, з ким ми ділимося нашим репозиторієм, ймовірно, захочуть ігнорувати ті самі речі, які ігноруємо ми.
Додамо до репозиторію та здійснимо коміт файлу `.gitignore`:

```bash
$ git add .gitignore
$ git commit -m "Ignore png files and the receipts folder."
$ git status
```

```output
On branch main
nothing to commit, working tree clean
```

Як бонус, використання `.gitignore` допомагає нам уникнути випадкового додавання до репозиторію файлів, які ми не хочемо відстежувати:

```bash
$ git add a.png
```

```output
The following paths are ignored by one of your .gitignore files:
a.png
Use -f if you really want to add them.
```

Якщо ми дійсно хочемо змінити наші налаштування ігнорування, ми можемо використати `git add -f`, щоб змусити Git щось додати. Наприклад, `git add -f a.csv`.
Якщо потрібно, можна також побачити статус ігнорованих файлів:

```bash
$ git status --ignored
```

```output
On branch main
Ignored files:
 (use "git add -f <file>..." to include in what will be committed)

        a.png
        b.png
        c.png
        receipts/

nothing to commit, working tree clean
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Ігнорування вкладених файлів

Враховуючи структуру каталогу, яка виглядає так:

```bash
receipts/data
receipts/plots
```

Як зробити так, щоб Git ігнорував тільки `receipts/plots`, а не `receipts/data`?

:::::::::::::::  solution

## Рішення

If you only want to ignore the contents of
`receipts/plots`, you can change your `.gitignore` to ignore
only the `/plots/` subfolder by adding the following line to
your .gitignore:

```output
receipts/plots/
```

This line will ensure only the contents of `receipts/plots` is ignored, and
not the contents of `receipts/data`.

Як і в більшості питань програмування, є ще кілька альтернативних способів, які можуть забезпечити виконання цього правила ігнорування.
Вправа "Варіант ігнорування вкладених файлів" нижче має трохи іншу структуру каталогів, та пояснює альтернативну відповідь.
Крім того, сторінка обговорення має більш детальну інформацію про правила ігнорування.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Додавання конкретних файлів

How would you ignore all `.png` files in your root directory except for
`final.png`?
Підказка: дізнайтеся, що робить `!` (оператор знаку оклику).

:::::::::::::::  solution

## Відповідь

Треба додати наступні два рядки до вашого файлу `.gitignore`:

```output
*.png           # ignore all png files
!final.png      # except final.png
```

Знак оклику призведе до включення раніше виключеного запису.

Note also that because you've previously committed `.png` files in this
lesson they will not be ignored with this new rule. Only future additions
of `.png` files added to the root directory will be ignored.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Варіант ігнорування вкладених файлів

Нехай ми маємо структуру каталогів, яка виглядає подібно до попередньої вправи "Ігнорування файлів у підкаталогах", проте дещо відрізняється:

```bash
receipts/data
receipts/images
receipts/plots
receipts/analysis
```

How would you ignore all of the contents in the receipts folder, but not `receipts/data`?

Підказка: подумайте про те, як ви раніше зробили виняток за допомогою оператору `!`.

:::::::::::::::  solution

## Рішення

If you want to ignore the contents of
`receipts/` but not those of `receipts/data/`, you can change your `.gitignore` to ignore
the contents of receipts folder, but create an exception for the contents of the
`receipts/data` subfolder. Ваш `.gitignore` буде виглядати так:

```output
receipts/*               # ignore everything in receipts folder
!receipts/data/          # do not ignore receipts/data/ contents
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Ігнорування всіх файлів з даними у каталозі

Припустимо, що у вас порожній файл `.gitignore`, і ви бачите структуру каталогів, яка виглядає так:

```bash
receipts/data/market_position/gps/a.dat
receipts/data/market_position/gps/b.dat
receipts/data/market_position/gps/c.dat
receipts/data/market_position/gps/info.txt
receipts/plots
```

What's the shortest `.gitignore` rule you could write to ignore all `.dat`
files in `result/data/market_position/gps`? При цьому, не ігноруйте `info.txt`.

:::::::::::::::  solution

## Рішення

Appending `receipts/data/market_position/gps/*.dat` will match every file in `receipts/data/market_position/gps`
that ends with `.dat`.
The file `receipts/data/market_position/gps/info.txt` will not be ignored.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Ігнорування всіх файлів з даними у репозиторії

Припустимо тепер, що у вас є багато файлів з розширенням `.csv` в різних підкаталогах вашого репозиторію.
Наприклад, ви можете мати:

```bash
results/a.csv
data/experiment_1/b.csv
data/experiment_2/c.csv
data/experiment_2/variation_1/d.csv
```

Як ігнорувати всі файли `.csv` без явного переліку усіх відповідних каталогів?

:::::::::::::::  solution

## Рішення

Додайте наступний рядок до файлу `.gitignore`:

```output
**/*.csv
```

Це призведе до ігнорування усіх файлів `.csv`, незалежно від їх розташування у дереві каталогів.
Ви все ще можете робити певні винятки з цього правила за допомогою знаку оклику.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Порядок застосування правил

Нехай нам дано файл `.gitignore` з наступним вмістом:

```bash
*.csv
!*.csv
```

Який вплив це буде мати на ігнорування файлів?

:::::::::::::::  solution

## Рішення

Оператор `!` скасує запис з попередньо визначеного шаблону ігнорування.
Оскільки запис `! *.csv` скасовує всі попередні файли `.csv` в `.gitignore`, жоден з них не буде проігноровано, і всі файли `.csv` будуть відстежуватися.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Log-файли

Ви написали скрипт, який створює багато проміжних log-файлів з назвами `log_01`, `log_02`, `log_03` тощо.
Ви хочете зберегти їх, але не бажаєте відстежувати їх за допомогою `git`.

1. Додайте до `.gitignore` один рядок, який виключає файли `log_01`, `log_02` тощо.

2. Перевірте свій "шаблон ігнорування", створивши деякі фіктивні файли з назвами `log_01`, `log_02` тощо.

3. Уявіть тепер, що файл `log_01` дуже важливий, та додайте його до відстежуваних файлів, не змінюючи `.gitignore`.

4. Обговоріть з сусідом, які інші типи файлів можуть перебувати у вашому проєкті, які ви не бажаєте відстежувати і тому бажаєте проігнорувати за допомогою `.gitignore`.

:::::::::::::::  solution

## Рішення

1. додайте або `log_*`  або  `log*`  як новий рядок до вашого `.gitignore`

2. відстежуйте `log_01` за допомогою `git add -f log_01`

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- The .gitignore file is a text file that tells Git which files to track and which to ignore in the repository.
- You can list specific files or folders to be ignored by Git, or you can include files that would normally be ignored.

::::::::::::::::::::::::::::::::::::::::::::::::::
