# How this repository was made (Cameroon)

This file is written by the build on 2026-10-29, the day the source data was retrieved.

## Sources

| Source | Address | Retrieved | Licence |
| --- | --- | --- | --- |
| Chambre des comptes, La_Constitution.pdf (Wayback 20240422094215) | <https://web.archive.org/web/20240422094215id_/https://chambredescomptes.cm/rails/active_storage/disk/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaDdDVG9JYTJWNVNTSWhPV1IxWTJsbU0yRm9jSEZrTm5GMVkyeG5jV3hvY1hkbmFqVnFlZ1k2QmtWVU9oQmthWE53YjNOcGRHbHZia2tpVVdsdWJHbHVaVHNnWm1sc1pXNWhiV1U5SWt4aFgwTnZibk4wYVhSMWRHbHZiaTV3WkdZaU95Qm1hV3hsYm1GdFpTbzlWVlJHTFRnbkoweGhYME52Ym5OMGFYUjFkR2x2Ymk1d1pHWUdPd1pVT2hGamIyNTBaVzUwWDNSNWNHVkpJaFJoY0hCc2FXTmhkR2x2Ymk5d1pHWUdPd1pVT2hGelpYSjJhV05sWDI1aGJXVTZDbXh2WTJGcyIsImV4cCI6IjIwMjQtMDQtMjJUMDk6NDc6MTUuMTc3WiIsInB1ciI6ImJsb2Jfa2V5In19--4c0d4390889e0a72ddcbc865ce5c7dd74796e817/La_Constitution.pdf> | 2026-10-29 | none stated |

Every word of law in this repository comes from these sources. Lexifina reads them and records each act's dated versions, and the laws that made each change, in one corpus file. This repository is built from that file.

## What is in it

| Act | Title | Versions | Span |
| --- | --- | ---: | --- |
| `constitution` | Constitution de la République du Cameroun (Loi n°96/06 du 18 janvier 1996 portant révision de la Constitution du 02 juin 1972, modifiée et complétée par la loi n°2008/001 du 14 avril 2008) | 1 | 2008-04-14 to 2008-04-14 |

The build made 1 commits:

| Commits | Kind |
| ---: | --- |
| 1 | first version of an act |
| 0 | one law on its in-force day |
| 0 | changes no amendment note explains, credited to the publisher |

Other counts:
- 0 commits on the branch `future`: laws not yet in force on 2026-10-29.
- 0 commits dated 1 January 1970: in force before 1970, with the real day in their `In-force` trailer.
- 2 laws in the record.
- 1 tags.

## Dates

Each commit is dated at noon (Africa/Douala) on the day the change took effect. The other dates, enacted and published, are trailers.

When a law gives no date of its own, the default rule applies: Constitution art. 31: the President promulgates laws, and they are published in the Journal officiel in French and English. The Journal officiel is not open, so versions are dated by the promulgation day printed in the law's title. https://web.archive.org/web/20240422094215id_/https://chambredescomptes.cm/rails/active_storage/disk/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaDdDVG9JYTJWNVNTSWhPV1IxWTJsbU0yRm9jSEZrTm5GMVkyeG5jV3hvY1hkbmFqVnFlZ1k2QmtWVU9oQmthWE53YjNOcGRHbHZia2tpVVdsdWJHbHVaVHNnWm1sc1pXNWhiV1U5SWt4aFgwTnZibk4wYVhSMWRHbHZiaTV3WkdZaU95Qm1hV3hsYm1GdFpTbzlWVlJHTFRnbkoweGhYME52Ym5OMGFYUjFkR2x2Ymk1d1pHWUdPd1pVT2hGamIyNTBaVzUwWDNSNWNHVkpJaFJoY0hCc2FXTmhkR2x2Ymk5d1pHWUdPd1pVT2hGelpYSjJhV05sWDI1aGJXVTZDbXh2WTJGcyIsImV4cCI6IjIwMjQtMDQtMjJUMDk6NDc6MTUuMTc3WiIsInB1ciI6ImJsb2Jfa2V5In19--4c0d4390889e0a72ddcbc865ce5c7dd74796e817/La_Constitution.pdf

## How the history is built

`tools/build.py` turns the corpus into this history in one pass: one commit per law, on the day it took effect. The same corpus always gives the same commit hashes. `tools/check_repo.py` reads every version back from git and compares it with the corpus. `tools/on.py` shows the law as it stood on any date. The corpus format is described in `tools/SCHEMA.md`.

- Corpus: sha256 `3be3141ab7cbba17ff25cafbce6c57f5b4a86d7bc2f6bcf9316c5a5b9f34a8ac` (69998 bytes).
- Builder blob hashes (`git hash-object`): `build.py` `a4faa25af85facb64b956b372f6b528aafdd7ec1`, `check.py` `3d00541d258745461ab52b1b0cb4e1716a27a95d`, `on.py` `644a6c0857fbcc72562032b58d57aa113fee0641`.
- Python 3.12.10; git version 2.46.0.windows.1.

## Checks

1 versions of 1 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

A second build from the same corpus into an empty folder gave identical hashes for all 1 history commits and 1 tags (history tip `ec837fd830ee30735241f01c266632d6805f40af`).

## Decisions made by hand

- Every prc.cm text (Loi 96/06, Loi 2008/001, Loi 2026/002, Penal Code 2016, Loi 2019/020, Loi 2024/017) is a scan, at most with an Acrobat Paper Capture OCR layer full of errors; under the no-OCR rule none is used.
- WIPO Lex's HTML of the 2016 Penal Code is itself OCR-derived ('the Jaw set out below'); not used.
- The Constitution is read from the Chambre des comptes' born-digital French PDF (pinned Wayback capture).
- The version is dated 14 April 2008, the promulgation of Loi n°2008/001 as printed in the consolidation's title; the day of its publication in the Journal officiel (Constitution art. 31(3)) was not found.
- Loi 2026/002 of 14 April 2026 amends the Constitution, but only a scan exists: it is not applied, and the record ends with the 2008 text.
- Units are rebuilt from the PDF's layout: an indented line, or a line starting with an item label ((1), a-, 1-, -), starts a paragraph; a line at the left margin, or after a line that reaches the right margin, continues it. Labels set apart from their text on the same line are joined to it.
- The cover (law number, title, enacting formula) is the unit intitule, the closing date and signature the unit signature; title and chapter headings are gathered in the unit structure, each line naming the article it precedes; page numbers are left out.
- Article headings are normalised to 'Article N' / 'Article premier': the consolidation prints 'ARICLE 48' (sic) and 'ARTICLE 53 : (nouveau) :'. The consolidation's '(nouveau)' marks after paragraph labels (arts. 6, 14, 15, 51, 67) flag provisions changed in 2008; they are editorial and left out.
- Spelling and typography are kept as printed (for example '(2 )' in art. 8, the duplicate item '3-' in art. 26, French spacing before ';' and ':').

## What AI did and did not do

The scripts that read the sources and build this repository were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- none at data-build time
