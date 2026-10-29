# Cameroon: law history in git

The law of Cameroon as a git history. Each provision is one file, each law gets one commit on the day it took effect, and the law that made each change is the commit's author. `git blame` names the law behind every paragraph, `git log` lists every change to an article, and `tools/en_vigueur_au.py` gives the text in force on any day.

It follows the shared design of [lexifina/global-law](https://github.com/lexifina/global-law), which places it at `cm/`.

> **Not today's text.** The Constitution here is the text of 1996 as amended in 2008. Loi n°2026/002 du 14 avril 2026 amended the Constitution again, but it is published only as a scanned image, so it is not applied (see below). This record ends with the 2008 text: for any later date it shows that text, not the Constitution as amended in 2026.

**What is in it now**

| Folder | Act | Versions | History |
| --- | --- | ---: | --- |
| `constitution/` | Constitution de la République du Cameroun (French) | 1 | Loi n°96/06 du 18 janvier 1996 portant révision de la Constitution du 02 juin 1972, as amended and supplemented by Loi n°2008/001 du 14 avril 2008: the title and enacting formula (`intitule.md`), the preamble, articles 1 to 69, the date and signature (`signature.md`), and the structure of titles and chapters (`structure.md`) |

**Why the Constitution has one version.** The open copies of the 1996 law, of Loi n°2008/001 and of Loi n°2026/002, on the Presidency's site, are scanned images, and scans are not transcribed. The only copy with a real text layer is the consolidated French text published by the Chambre des comptes de la Cour suprême. So the Constitution enters the history once, on 14 April 2008, credited to that publisher as the oldest version available; its commit names Loi n°96/06 and Loi n°2008/001.

**What is missing**

- **Before 2008**: the Constitution of 1972, its 1996 revision as first enacted, and the words Loi n°2008/001 replaced.
- **After 2008**: Loi n°2026/002 du 14 avril 2026 (constitutional amendment), a scan only.
- **The English text.** Cameroon publishes its laws in French and English; no official English copy with a text layer was found.
- **Other acts.** The Penal Code (Loi n°2016/007) and the personal data law (Loi n°2024/017) are published only as scans; neither is built.

## A lawyer's questions as git commands

| Question | Command |
| --- | --- |
| What did art. 6 say on 1 January 2020? | `python tools/en_vigueur_au.py 2020-01-01 constitution 6` |
| The same, with raw git (bash) | `git show $(git rev-list -1 --before="2020-01-01T23:59:59Z" HEAD -- constitution/art-006.md):constitution/art-006.md` |
| The same, in PowerShell | `$r = git rev-list -1 --before="2020-01-01T23:59:59Z" HEAD -- constitution/art-006.md; git show "$($r):constitution/art-006.md"` |
| Which law wrote each paragraph of art. 6? | `git blame constitution/art-006.md` |
| Every change to an article, word by word | `git log -p --word-diff constitution/art-006.md` |
| Every law in the record, in order | `git log --reverse --format="%ad %an" --date=short` |
| The whole Constitution on a date | `python tools/en_vigueur_au.py 2020-01-01 constitution` |
| Where an article sits (title, chapter) | `constitution/structure.md` |

After a rebuild, refresh tags with `git fetch --force --tags`: a plain fetch keeps stale tags.

## Dates

- Each commit is dated at **noon, Africa/Douala, on the day the change took effect**. The day is also in the `In-force:` trailer, which the tools read.
- **The one version is dated by the promulgation of Loi n°2008/001, 14 April 2008,** as printed in the consolidation's title. Under art. 31 of the Constitution laws are published in the Journal officiel in French and English; the Journal officiel is not openly available, so the day of publication was not found.
- `--before="YYYY-MM-DD"` alone uses your clock's time of day, so a change that took effect that day may be missed before noon. Use `T23:59:59Z` as above, or the tool.

## How it is checked

- Every version is read back from git on the day it took effect and on the day before (`tools/check_repo.py`); both must match `corpus.json`. Results are in `REPRODUCE.md`.
- The repository is rebuilt from the same data a second time and must give identical hashes.
- The text is read from the PDF's own text layer (the file was made with Word, not scanned). Every article was compared, character for character with spacing ignored, with a second, independent extraction of the same PDF (pdftotext): every character of every file is in the PDF in the same place, and the only text the PDF has besides the files is page numbers, the article and title headings, and the "(nouveau)" marks.

## What "author" means

The author of a commit is the law that made the change, with an e-mail address that cannot exist, so `git blame` and `git log --author` name laws. The committer is always Lexifina. Where a version is not the act as enacted, as with the Constitution here, the first commit is credited to the publisher (the Chambre des comptes de la Cour suprême) as the oldest version available. Blame credits whole lines (one paragraph per line); use `git log -p --word-diff` to see exactly which words a change made.

## What it does not show

- **The Constitution before 2008 and after 2008** (see above).
- **Headings** (titres, chapitres) are not in the article files; they are gathered in `structure.md`, each line naming the article it precedes.
- **The consolidation's "(nouveau)" marks**, which flag the provisions Loi n°2008/001 changed (arts. 6, 14, 15, 51, 53, 67), and its page numbers.
- **Typos are kept as printed** in the article text, for example "(2 )" in art. 8 and the item "3-" printed twice in art. 26; only the heading "ARICLE 48" is normalised to "Article 48".
- **No OCR.** Texts published only as scanned images are not included.

## How it is built

Lexifina reads the sources listed here and records each act's dated versions, and the laws that made each change, in one corpus file. `tools/build.py` writes this repository from that file, and the same corpus always gives the same commit hashes; `REPRODUCE.md` gives the details and the checks. A model (Claude) wrote Lexifina's code; it wrote no legal text, and no model runs when the history is built.

Lexifina's code reads the consolidation's PDF through its own text layer with PyMuPDF, rebuilds paragraphs from the layout (an indented line or a line starting with an item label starts a paragraph), splits the text into one file per article and writes `corpus.json`; the shared builder `tools/build.py` turns it into the commits.

## Sources

| Source | What it gives | Used for |
| --- | --- | --- |
| [Chambre des comptes de la Cour suprême](https://chambredescomptes.cm) | The consolidated French text of the Constitution (Word PDF, 30 pages), as kept by the Internet Archive (copy of 22 April 2024) | The Constitution's text |
| [Présidence de la République](https://www.prc.cm) | Laws and decrees as scanned PDFs, with their titles and dates | The title and date of Loi n°2008/001; Loi n°2026/002 (scan, not used) |
| [Services du Premier Ministre](https://www.spm.gov.cm) | Decrees and appointments | Not used |

Research guides that cover every country: [GlobaLex](https://www.nyulawglobal.org/globalex/) and the [Law Library of Congress Guide to Law Online](https://www.loc.gov/research-centers/law-library-of-congress/researcher-resources/guide-to-law-online/).

## Licence

See [LICENSE](LICENSE). Text: the Constitution de la République du Cameroun (Loi n°96/06 as amended by Loi n°2008/001), French text as published by the Chambre des comptes de la Cour suprême; an official legislative text of the Republic of Cameroon, reproduced with that attribution. Lexifina's arrangement, messages, tables and tools are CC0.

This repository is not legal advice.

## En français

Ce dépôt retrace l'histoire du droit camerounais dans git : un fichier par article, un commit par loi à la date de son entrée en vigueur, et la loi qui a modifié le texte comme auteur du commit. Il contient pour l'instant la Constitution de la République du Cameroun (loi n°96/06 du 18 janvier 1996 modifiée et complétée par la loi n°2008/001 du 14 avril 2008), texte français publié par la Chambre des comptes de la Cour suprême. **Ce n'est pas le texte en vigueur aujourd'hui** : la loi n°2026/002 du 14 avril 2026, publiée seulement en image numérisée, n'est pas appliquée. `python tools/en_vigueur_au.py 2020-01-01 constitution 6` affiche l'article 6 en vigueur à cette date, et `git blame constitution/art-006.md` indique la loi de chaque paragraphe. Ce dépôt ne constitue pas un conseil juridique.
