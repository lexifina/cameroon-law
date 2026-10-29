# How this repository was made (Cameroon)

This file is written by the build on 2026-10-29, the day the source data was retrieved. Everything below is enough to rebuild this repository hash for hash.

## Sources

| Source | Address | Retrieved | Requests | Licence |
| --- | --- | --- | ---: | --- |
| Chambre des comptes, La_Constitution.pdf (Wayback 20240422094215) | <https://web.archive.org/web/20240422094215id_/https://chambredescomptes.cm/rails/active_storage/disk/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaDdDVG9JYTJWNVNTSWhPV1IxWTJsbU0yRm9jSEZrTm5GMVkyeG5jV3hvY1hkbmFqVnFlZ1k2QmtWVU9oQmthWE53YjNOcGRHbHZia2tpVVdsdWJHbHVaVHNnWm1sc1pXNWhiV1U5SWt4aFgwTnZibk4wYVhSMWRHbHZiaTV3WkdZaU95Qm1hV3hsYm1GdFpTbzlWVlJHTFRnbkoweGhYME52Ym5OMGFYUjFkR2x2Ymk1d1pHWUdPd1pVT2hGamIyNTBaVzUwWDNSNWNHVkpJaFJoY0hCc2FXTmhkR2x2Ymk5d1pHWUdPd1pVT2hGelpYSjJhV05sWDI1aGJXVTZDbXh2WTJGcyIsImV4cCI6IjIwMjQtMDQtMjJUMDk6NDc6MTUuMTc3WiIsInB1ciI6ImJsb2Jfa2V5In19--4c0d4390889e0a72ddcbc865ce5c7dd74796e817/La_Constitution.pdf> | 2026-10-29 | 1 | none stated |

Every response is cached by the sha256 of its URL, and the table above lists every source with its retrieval date. Lexifina keeps its fetch scripts privately; `tools/parse.py` reads only the cache and writes `corpus.json`, so the same cache always gives the same corpus.

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

When a law gives no date of its own, the default rule applies: Not yet sourced: versions are dated by promulgation day (decision). https://www.prc.cm/en/news/the-acts/laws

## Commands, in order

```sh
# fill the cache from the sources listed above (network; Lexifina's fetch scripts are kept privately)
python -X utf8 tools/parse.py                  # cache -> corpus.json (no network)
python -X utf8 tools/build.py <folder> --repo <empty folder> --docs-date 2026-10-29
python -X utf8 tools/check_repo.py <repo> --corpus <folder>/corpus.json --write
python -X utf8 tools/build.py <folder> --repo <another empty folder> --docs-date 2026-10-29 --compare <repo>
```

In the main Lexifina repository these files live as follows:
- `fetch.py`, `parse.py` and `repo/`: in `scripts/cameroon-law/`.
- `build.py`, `check_repo.py` (`check.py` there) and `on.py`: in `scripts/gitlaw_common/`, with the corpus format in `SCHEMA.md`.

## Exact inputs

- `corpus.json` sha256 `45cb5eafd24c5a6b65a409b89c1965747729f56f4a85c9eec7b3261bb78dca79` (67001 bytes)
- Script blob hashes (`git hash-object`):

| Script | Blob |
| --- | --- |
| build.py | `2bfc4ac9df193a7be844556d7f69cdb9d93ec3f2` |
| check.py | `cd346c7ff9996d450240e3bab52bcae03b2bc865` |
| on.py | `644a6c0857fbcc72562032b58d57aa113fee0641` |
| fetch.py | `058c1102fe6b3d25454b3688775ff1c07b40383b` |
| parse.py | `0a7a239b20914bc8ee20da623f72ee45bb709189` |

- Tools:
  - Python 3.14.0
  - git version 2.52.0.windows.1
  - curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP

## Checks

1 versions of 1 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

Not yet run for this build.

These results are from the check run on the build before this one, from the same corpus.

## Decisions made by hand

- Every prc.cm text (Loi 96/06, Loi 2008/001, Loi 2026/002, Penal Code 2016, Loi 2019/020, Loi 2024/017) is a scan, at most with an Acrobat Paper Capture OCR layer full of errors; under the no-OCR rule none is used.
- WIPO Lex's HTML of the 2016 Penal Code is itself OCR-derived ('the Jaw set out below'); not used.
- The Constitution is read from the Chambre des comptes' born-digital French PDF (pinned Wayback capture).
- The version is dated 14 April 2008 (promulgation of Loi 2008/001); no sourced in-force rule yet.
- Loi 2026/002 of 14 April 2026 amends the Constitution, but only a scan exists: not applied, so this is preview only and not published.

## What AI did and did not do

The fetch, parse and check scripts were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above, through code that anyone can rerun.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- none at data-build time
