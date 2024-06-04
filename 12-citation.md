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
To reference Software Carpentry in publications, please cite both of the following:

Greg Wilson: "Software Carpentry: Getting Scientists to Write Better
Code by Making Them More Productive".  Computing in Science &
Engineering, Nov-Dec 2006.

Greg Wilson: "Software Carpentry: Lessons Learned". arXiv:1307.5448,
July 2013.

@article{wilson-software-carpentry-2006,
    author =  {Greg Wilson},
    title =   {Software Carpentry: Getting Scientists to Write Better Code by Making Them More Productive},
    journal = {Computing in Science \& Engineering},
    month =   {November--December},
    year =    {2006},
}

@online{wilson-software-carpentry-2013,
  author      = {Greg Wilson},
  title       = {Software Carpentry: Lessons Learned},
  version     = {1},
  date        = {2013-07-20},
  eprinttype  = {arxiv},
  eprint      = {1307.5448}
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
