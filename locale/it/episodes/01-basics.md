---
title: Controllo di versione automatico
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Comprendere i vantaggi di un sistema automatizzato di controllo delle versioni.
- Comprendere le basi di come funzionano i sistemi di controllo automatico delle versioni.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Che cos'è il controllo di versione e perché dovrei usarlo?

::::::::::::::::::::::::::::::::::::::::::::::::::

Inizieremo esplorando come il controllo di versione può essere utilizzato
per tenere traccia di ciò che qualcuno ha fatto e quando.
Anche se non stai collaborando con altre persone, il controllo automatico di versione
è molto meglio di questa situazione:

!["notFinal.doc" di Jorge Cham, https://www.phdcomics.com](fig/phd101212s.png){alt='Comic: un dottorando invia "FINAL.doc" al loro supervisore, ma dopo diversi turni sempre più intensi e frustranti di commenti e revisioni finiscono con un file chiamato "FINAL_rev.22.comments49.corrections.10.#@$%PERCHEHOFATTOILDOTTORATO????.doc"'}

Ci siamo trovati tutti in questa situazione: sembra inutile avere
versioni multiple quasi identiche dello stesso documento. Alcuni editor di testo ci permettono di affrontare questo problema un po' meglio, come il
[rilevamento modifiche](https://support.office.com/en-us/article/Track-changes-in-Word-197ba630-0f5f-4a8e-9a77-3712475e806a) di Microsoft Word,
la [cronologia versioni](https://support.google.com/docs/answer/190843?hl=it) di Google Docs,
la [Registrazione e visualizzazione delle modifiche](https://help.libreoffice.org/latest/it/text/shared/guide/redlining.html) di LibreOffice.

I sistemi di controllo delle versioni iniziano con una versione base del documento e
quindi registrano le modifiche apportate a ogni passo. Puoi immaginarlo come un salvataggio dei tuoi progressi: puoi tornare indietro al documento iniziale e riprodurre ogni cambiamento che hai fatto, fino alla tua versione più recente.

![](fig/play-changes.svg){alt='Le modifiche sono salvate in sequenza'}

Once you think of changes as separate from the document itself, you
can then think about "playing back" different sets of changes on the base document, ultimately
resulting in different versions of that document. For example, two users can make independent
sets of changes on the same document.

![](fig/versions.svg){alt='Different Versions Can be Saved'}

Unless multiple users make changes to the same section of the document - a
[conflict](../learners/reference.md#conflict) - you can
incorporate two sets of changes into the same base document.

![](fig/merge.svg){alt='Multiple Versions Can be Merged'}

A version control system is a tool that keeps track of these changes for us,
effectively creating different versions of our files. It allows us to decide
which changes will be made to the next version (each record of these changes is
called a [commit](../learners/reference.md#commit)), and keeps useful metadata
about them. The complete history of commits for a particular project and their
metadata make up a [repository](../learners/reference.md#repository).
Repositories can be kept in sync across different computers, facilitating
collaboration among different people.

:::::::::::::::::::::::::::::::::::::::::  callout

## The Long History of Version Control Systems

Automated version control systems are nothing new.
Tools like [RCS](https://en.wikipedia.org/wiki/Revision_Control_System), [CVS](https://en.wikipedia.org/wiki/Concurrent_Versions_System), or [Subversion](https://en.wikipedia.org/wiki/Apache_Subversion) have been around since the early 1980s and are used by
many large companies.
However, many of these are now considered legacy systems (i.e., outdated) due to various
limitations in their capabilities.
More modern systems, such as Git and [Mercurial](https://swcarpentry.github.io/hg-novice/),
are _distributed_, meaning that they do not need a centralized server to host the repository.
These modern systems also include powerful merging tools that make it possible for
multiple authors to work on
the same files concurrently.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Paper Writing

- Imagine you drafted an excellent paragraph for a paper you are writing, but later ruin
  it. How would you retrieve the _excellent_ version of your conclusion? Is it even possible?

- Imagine you have 5 co-authors. How would you manage the changes and comments
  they make to your paper?  If you use LibreOffice Writer or Microsoft Word, what happens if
  you accept changes made using the `Track Changes` option? Do you have a
  history of those changes?

:::::::::::::::  solution

## Solution

- Recovering the excellent version is only possible if you created a copy
  of the old version of the paper. The danger of losing good versions
  often leads to the problematic workflow illustrated in the PhD Comics
  cartoon at the top of this page.

- Collaborative writing with traditional word processors is cumbersome.
  Either every collaborator has to work on a document sequentially
  (slowing down the process of writing), or you have to send out a
  version to all collaborators and manually merge their comments into
  your document. The 'track changes' or 'record changes' option can
  highlight changes for you and simplifies merging, but as soon as you
  accept changes you will lose their history. You will then no longer
  know who suggested that change, why it was suggested, or when it was
  merged into the rest of the document. Even online word processors like
  Google Docs or Microsoft Office Online do not fully resolve these
  problems.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Version control is like an unlimited 'undo'.
- Version control also allows many people to work in parallel.

::::::::::::::::::::::::::::::::::::::::::::::::::
