---
title: Ліцензування
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Пояснити, чому важливо додавати інформацію про ліцензування до репозиторію.
- Як обрати відповідну ліцензію?
- Пояснити відмінності в ліцензуванні та соціальних очікуваннях.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Яку інформацію про ліцензію має містити моя робота?

::::::::::::::::::::::::::::::::::::::::::::::::::

As an open source project, Software Carpentry relies on volunteers to create our lessons and includes a file named `LICENSE` or `LICENSE.txt` file in all public lesson repositories. This file is used to specify that all materials are freely available under the Creative Commons Attribution license. Without a file that clearly states under which license any public source code, manuscript or other creative works is being made available, the default copyright laws apply. To learn more about licensing and open source, you can read more about [Github's description of licenses] and the [legal grounds for open source licensing].

Ліцензія вирішує цю проблему, надаючи іншим особам (ліцензіатам) права, яких вони в іншому випадку не мали б. Те, які права надаються, і за яких умов, може відрізнятися, але часто не дуже, у різних ліцензіях. На практиці, деякі ліцензії на сьогодні є найпопулярнішими, і [choosealicense.com](https://choosealicense.com/) може допомогти вам знайти поширену ліцензію, яка відповідає вашим потребам.  Важливими міркуваннями є:

- Чи хочете ви вказати патентні права.
- Чи потрібно вам, щоб люди, які будуть поширювати похідні роботи, також розповсюджували свій вихідний код.
- Чи є вміст, який ви ліцензуєте, вихідним кодом.
- Чи хочете ви взагалі ліцензувати код.

Вибір ліцензії, яка є загальновживаною, полегшує життя для учасників і користувачів, тому що вони, швидше за все, вже знайомі з ліцензією, і їм не доведеться продиратися через купу жаргону, щоб вирішити, чи згодні вони з нею.  [Ініціатива відкритого коду](https://opensource.org/licenses) та [Фонд вільного програмного забезпечення](https://www.gnu.org/licenses/license-list.html) підтримують списки ліцензій, які є гарним вибором.

[Ця стаття][software-licensing] пропонує відмінний огляд ліцензування та його варіантів з точки зору вчених, які також пишуть код.

Зрештою, важливо те, що є чітке твердження про те, яка ліцензія використовується. Також, ліцензію краще вибирати з самого початку, навіть для репозиторію, який не є загальнодоступним. Відкладання
цієї справи лише ускладнить ваше становище у майбутньому, тому що кожного разу,
коли нові співавтори зроблять свій внесок, вони також володітимуть авторським правом. Таким чином, будь-який вибір ліцензії пізніше також буде потрібно узгоджувати з ними.

:::::::::::::::::::::::::::::::::::::::  challenge

## Чи можу я використовувати відкриту ліцензію?

Дізнайтеся, чи дозволено вам застосовувати відкриту ліцензію до вашого програмного забезпечення.
Чи можете ви зробити це в односторонньому порядку, або вам потрібен дозвіл від когось у вашому закладі?
Якщо так, то від кого?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## З якими ліцензіями ми вже погодились?

Багато програм, якими ми користуємося щодня (у тому числі і на цьому семінарі) доступні як програмне забезпечення з відкритим вихідним кодом. Виберіть на GitHub проєкт зі списку нижче, або інший проєкт на ваш вибір. Знайдіть його ліцензію (зазвичай у файлі під назвою `LICENSE` або `COPYING`) і подивіться як вона обмежує використання програмного забезпечення. Чи це одна з ліцензій обговорювана у цьому епізоді? Якщо ні, то чим вона відрізняється?

- [Git](https://github.com/git/git) - інструмент для управління вихідним кодом
- [CPython](https://github.com/python/cpython) - стандартна реалізація мови Python
- [Jupyter](https://github.com/jupyter) - проєкт, який стоїть за вебноутбуками Python, які ми будемо використовувати
- [EtherPad](https://github.com/ether/etherpad-lite) - редактор для спільної роботи в реальному часі

::::::::::::::::::::::::::::::::::::::::::::::::::

[software-licensing]: https://doi.org/10.1371/journal.pcbi.1002598
[Github's description of licenses]: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository
[legal grounds for open source licensing]: https://opensource.guide/legal/#are-public-github-projects-open-source

:::::::::::::::::::::::::::::::::::::::: keypoints

- The `LICENSE`, `LICENSE.md`, or `LICENSE.txt` file is often used in a repository to indicate how the contents of the repo may be used by others.
- People who incorporate General Public License (GPL'd) software into their own software must make the derived software also open under the GPL license if they decide to share it; most other open licenses do not require this.
- The Creative Commons family of licenses allow people to mix and match requirements and restrictions on attribution, creation of derivative works, further sharing, and commercialization.
- People who are not lawyers should not try to write licenses from scratch.

::::::::::::::::::::::::::::::::::::::::::::::::::
