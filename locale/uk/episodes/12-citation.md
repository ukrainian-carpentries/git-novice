---
title: Цитування
teaching: 2
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Зробити вашу роботу легкою для цитування

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Як полегшити цитування моєї роботи?

::::::::::::::::::::::::::::::::::::::::::::::::::

Ви можете додати файл під назвою `CITATION` або `CITATION.txt`,
який описує, як цитувати ваш проєкт;
наприклад, для Software
Carpentry [цей файл](https://github.com/swcarpentry/website/blob/gh-pages/CITATION)
зазначає:

```source
To reference Software Carpentry in publications, please cite:

Greg Wilson: "Software Carpentry: Lessons Learned". F1000Research,
2016, 3:62 (doi: 10.12688/f1000research.3-62.v2).

@online{wilson-software-carpentry-2016,
  author      = {Greg Wilson},
  title       = {Software Carpentry: Lessons Learned},
  version     = {2},
  date        = {2016-01-28},
  url         = {http://f1000research.com/articles/3-62/v2},
  doi         = {10.12688/f1000research.3-62.v2}
}
```

Більш детальні поради та інші способи зробити ваш код цитованим можна знайти
[у блозі Software Sustainability Institute](https://www.software.ac.uk/how-cite-and-describe-software), а також у:

```source
Smith AM, Katz DS, Niemeyer KE, FORCE11 Software Citation Working Group. (2016) Software citation
principles. [PeerJ Computer Science 2:e86](https://peerj.com/articles/cs-86/)
https://doi.org/10.7717/peerj-cs.8
```

Існує також тип запису [`@software{...`](https://www.google.com/search?q=git+citation+%22%40software%7B%22) для [BibTeX](https://www.ctan.org/pkg/bibtex) на випадок, якщо для проєкту, який ви хочете процитувати,
немає «джерела-парасольки» для цитування, такого як стаття або книга.

:::::::::::::::::::::::::::::::::::::::: keypoints

- Додайте файл `CITATION` до репозиторію, щоб пояснити у ньому, як ви хочете бачити посилання на свою роботу.

::::::::::::::::::::::::::::::::::::::::::::::::::
