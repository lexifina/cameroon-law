# Cameroon: law history in git

[Français](README.md) · **English**

<picture>
<source media="(prefers-color-scheme: dark)" srcset="data/effort-dark.svg">
<img alt="Cameroonian law in this repository: what it holds, changes by the year they took effect, completeness against the official sources, and the correctness of what is held" src="data/effort-light.svg" width="880">
</picture>

The law of Cameroon as a git history: one file per article, each version committed on the day it took effect, and the law that made each change as the commit's author. `git blame` names the law behind every paragraph; `git log` lists every change to an article; picking a date gives the text in force that day.

Part of [lexifina/global-law](https://github.com/lexifina/global-law), one git history for every country's law: this is Cameroon's national law, which global-law reads at `cameroon/national/acts/`.

**What is in it now:** 434 texts, each as its source publishes it: the Constitution; 8 codes ([code-de-la-marine-marchande](codes/code-de-la-marine-marchande/), [code-de-procedure-penale](codes/code-de-procedure-penale/), [code-du-travail](codes/code-du-travail/), [code-electoral](codes/code-electoral/), [code-gazier](codes/code-gazier/), [code-minier](codes/code-minier/), [code-penal](codes/code-penal/), [code-petrolier](codes/code-petrolier/)); 71 laws, 6 ordinances, 203 decrees, 115 orders (arrêtés) and 30 decisions, filed by kind and year ([STRUCTURE.md](STRUCTURE.md); the full list is [`corpus.json`](corpus.json)). Court decisions (OHADA) are in a separate repository.

**Not today's text.** Loi n°2026/002 of 14 April 2026 amended the Constitution again and is not applied (see [What it does not show](#what-it-does-not-show)), so for any day after 14 April 2008 this record gives the 2008 text.

Each article of the Constitution is one file in `constitution/` (`art-006.md` is Article 6, the election and term of the President); the preamble is `preambule.md`, and [`constitution/structure.md`](constitution/structure.md) names the article each title and chapter heading precedes. The other texts are filed by kind and year (`lois/2016/…`, `decrets/2018/…`); codes are in `codes/<code>/`, one file per article (`codes/code-penal/art-074.md`). The two laws of 1991 and 2001 are still one file each, `texte.md` (`lois/1991/91-022-rehabilitation/`, `lois/2001/2001-016-chambres-consulaires/`).

The text is in French, as its publishers print it. Cameroon publishes its laws in French and English; the English text is not part of this record.

## Questions as git commands

| Question | Command |
| --- | --- |
| Every change to Article 6 of the Constitution (the President's term) | `git log -p --word-diff -- constitution/art-006.md` |
| Which law wrote each line of Article 6? | `git blame constitution/art-006.md` |
| Every version of Article 6, with the day each took effect | `git log --format="%h %(trailers:key=In-force,valueonly,separator=) %an" -- constitution/art-006.md` |
| Article 6 as in force on 1 January 2020 | `python tools/en_vigueur_au.py 2020-01-01 constitution 6` |

**Outside git**, we also host this record publicly, for free:

- **[Ask Claude about Cameroonian law](https://global-law.lexifina.com/v1/try_claude?country=cameroon)**: opens Claude with a first question ready.
- **[Ask ChatGPT](https://global-law.lexifina.com/v1/try_chatgpt?country=cameroon)**: the same question in ChatGPT.
- **[Search Cameroonian law on our site](https://lexifina.com/law?country=cameroon&q=droit+de+gr%C3%A8ve)**: a search for "droit de grève" (the right to strike), with each provision's text and its versions, no account needed.
- **[Developer REST API](https://global-law.lexifina.com/v1/search_law?query=droit+de+gr%C3%A8ve&jurisdiction=cameroon)**: the same search as JSON, with no key. See the [documentation here](https://lexifina.com/api/global-law).

Both assistants read the record through the free Global law connector (MCP, `https://global-law.lexifina.com/mcp`); the first time, the chat gives you the link that adds it. Ask in French or in English.

**Host it yourself easily.** All it takes is git and Python, and the tools run on your machine:

```sh
git clone --depth 1 https://github.com/lexifina/global-law && cd global-law
python3 tools/countries.py add cameroon/national/acts
python3 tools/law.py show 2026-01-01 cameroon/national/acts/constitution/art-006.md
```

Opened in Claude Code, Cursor, VS Code or Codex, the clone also starts a local MCP server with the same tools ([MCP.md](https://github.com/lexifina/global-law/blob/main/MCP.md)).

## Dates

Each commit is dated at noon, Africa/Douala, on the day the version took effect, and carries it in its `In-force:` trailer:

- the Constitution: 14 April 2008, the promulgation of Loi n°2008/001, as the consolidation's title prints it;
- Loi n°91/022 and Loi n°2001/016: 16 December 1991 and 23 July 2001, the promulgation dates their texts print;
- the other texts: the date of the text (signature or promulgation) in its title; Cameroonian laws are published by the urgency procedure and apply from promulgation unless they say otherwise (`In-force-basis` trailer); before 1970 the commit is dated 1 January 1970 and the real day is in the subject and in `In-force`;
- the other texts: the date of the text (signature or promulgation) in its title; Cameroonian laws are published by the urgency procedure and apply from promulgation unless they say otherwise (`In-force-basis` trailer); before 1970 the commit is dated 1 January 1970 and the real day is in the subject and in `In-force`;
- the other texts: the date of the text (signature or promulgation) in its title; Cameroonian laws are published by the urgency procedure and apply from promulgation unless they say otherwise (`In-force-basis` trailer); before 1970 the commit is dated 1 January 1970 and the real day is in the subject and in `In-force`;
- laws are published in the Journal officiel in French and English (article 31 of the Constitution), but the Journal officiel is not openly available, so no day of publication was found.

Each first commit is credited to its publisher as the oldest version available, and names the laws that produced the text: the Chambre des comptes de la Cour suprême for the Constitution (Loi n°96/06 and Loi n°2008/001), the Présidence de la République for the two laws. The committer is always Lexifina.

## How it is checked

- `tools/check_repo.py` reads every version back from git on the day it took effect and on the day before, and both must match the corpus the history was built from; it also checks that every commit's trailers parse and that there are no merge commits.
- `tools/build.py` writes the history from that corpus in one pass; a second build into an empty folder gave identical hashes. New acts are appended on top of `main` by `tools/append.py`, which rewrites nothing. The results are in [`REPRODUCE.md`](REPRODUCE.md), and [`data/corpus.sha256`](data/corpus.sha256) gives the corpus's hash.
- The Constitution is the PDF's own text layer (the file was made with Word, not scanned), compared character by character with a second, independent extraction; the two laws' official HTML texts were read by two independent parsers, and every line matched.
- [vulnerabilities/](vulnerabilities/README.en.md): possible drafting mistakes in this law, found by global-law's detectors and checked by hand. Not run yet.

## What it does not show

- **The Constitution in force today.** Loi n°2026/002 of 14 April 2026 amended it, but it is published only as a scan, and scans are not transcribed.
- **The Constitution before 2008**: the Constitution of 1972, its 1996 revision as enacted and the words Loi n°2008/001 replaced; the Presidency's copies of both laws are scans.
- **The English text**, of equal standing with the French (article 1): no official English copy with a text layer was found.
- **Texts published only as scans** (829 so far): no OCR is done here; a text layer supplied by the publisher is taken (62 texts, trailer `Text-source: publisher OCR`, not proofread). The Presidency's site (prc.cm) did not answer us on 26 November 2026 (284 known addresses waiting). The Code de procédure pénale has only articles 59 to 672, the only ones the Prime Minister's Services publish as text. The Code civil, the CEMAC code and the OHADA Uniform Acts (only third-party OCR copies found) are missing.
- **Amendments to codes and laws**: each text is given as its source publishes it; amending laws are separate texts and are not applied to the text they amend.
- **Headings and marks.** Title and chapter headings are in `structure.md`, not in the article files; the consolidation's "(nouveau)" marks, which flag the provisions Loi n°2008/001 changed, and its page numbers are left out. Typos are kept as printed.

## Sources

| Source | Kind | Past versions | Notes |
| --- | --- | --- | --- |
| [Chambre des comptes de la Cour suprême](https://chambredescomptes.cm) | The consolidated French text of the Constitution, a Word PDF of 30 pages, as kept by the Internet Archive on 22 April 2024 | no, the 2008 text only | The Constitution's text and its date |
| [Présidence de la République](https://www.prc.cm/fr/actualites/actes/lois) | Official law pages: HTML texts and scanned PDFs | no, each text as published | The texts and dates of Loi n°91/022 and Loi n°2001/016, and the title and date of Loi n°2008/001; Loi n°2026/002 is a scan, not used |
| [Services du Premier Ministre](https://www.spm.gov.cm/site/?q=fr/documentation/lois-et-r%C3%A8glements) | "Lois et Règlements": HTML texts and PDFs (laws, decrees, orders, decisions) | no, each text as published | Official source; the Code de procédure pénale (articles 59 to 672) as HTML; scanned PDFs without a text layer are left out |
| [FAOLEX](https://www.fao.org/faolex/) (FAO) | PDF copies of the published texts, with their text layer | no | Secondary copy (`Source-status` trailer): Labour Code, Penal Code, mining, petroleum, forestry, land and state-property codes, etc.; non-commercial reuse with credit to FAO |
| [WIPO Lex](https://www.wipo.int/wipolex/) (WIPO) | PDF copies of a few laws | no | Secondary copy (`Source-status` trailer) |
| Hugging Face `endomorphosis/ipfs_cameroon_laws` | A list of addresses only | – | Used only as a URL list: every text is read again from its official source; no text from this dataset is used |
| [Services du Premier Ministre](https://www.spm.gov.cm/site/?q=fr/documentation/lois-et-r%C3%A8glements) | "Lois et Règlements": HTML texts and PDFs (laws, decrees, orders, decisions) | no, each text as published | Official source; the Code de procédure pénale (articles 59 to 672) as HTML; scanned PDFs without a text layer are left out |
| [FAOLEX](https://www.fao.org/faolex/) (FAO) | PDF copies of the published texts, with their text layer | no | Secondary copy (`Source-status` trailer): Labour Code, Penal Code, mining, petroleum, forestry, land and state-property codes, etc.; non-commercial reuse with credit to FAO |
| [WIPO Lex](https://www.wipo.int/wipolex/) (WIPO) | PDF copies of a few laws | no | Secondary copy (`Source-status` trailer) |
| Hugging Face `endomorphosis/ipfs_cameroon_laws` | A list of addresses only | – | Used only as a URL list: every text is read again from its official source; no text from this dataset is used |
| [Services du Premier Ministre](https://www.spm.gov.cm/site/?q=fr/documentation/lois-et-r%C3%A8glements) | "Lois et Règlements": HTML texts and PDFs (laws, decrees, orders, decisions) | no, each text as published | Official source; the Code de procédure pénale (articles 59 to 672) as HTML; scanned PDFs without a text layer are left out |
| [FAOLEX](https://www.fao.org/faolex/) (FAO) | PDF copies of the published texts, with their text layer | no | Secondary copy (`Source-status` trailer): Labour Code, Penal Code, mining, petroleum, forestry, land and state-property codes, etc.; non-commercial reuse with credit to FAO |
| [WIPO Lex](https://www.wipo.int/wipolex/) (WIPO) | PDF copies of a few laws | no | Secondary copy (`Source-status` trailer) |
| Hugging Face `endomorphosis/ipfs_cameroon_laws` | A list of addresses only | – | Used only as a URL list: every text is read again from its official source; no text from this dataset is used |

## Licence

See [`LICENSE`](LICENSE). The texts are official legislative texts of the Republic of Cameroon, reproduced with attribution to their sources (Chambre des comptes de la Cour suprême, Présidence de la République, Services du Premier Ministre, FAOLEX, WIPO Lex); the publishers state no licence, and their copyright status is not confirmed here. The history, commit messages, tables and tools are CC0 1.0.

No AI model wrote, translated or corrected legal text, and none runs when the history is built; the scripts were written by an AI model (see [`REPRODUCE.md`](REPRODUCE.md)).

## Visualisations

- [visualisations/](visualisations/README.en.md): charts drawn from this repository. Not drawn yet.

## More

From [lexifina/global-law](https://github.com/lexifina/global-law):

- [FEATURES.md](https://github.com/lexifina/global-law/blob/main/FEATURES.md): every feature, and what every corpus must support (`git blame`, `law.py`, the MCP tools)
- [FAQ.md](https://github.com/lexifina/global-law/blob/main/FAQ.md): questions and answers · [CONTRIBUTING.md](https://github.com/lexifina/global-law/blob/main/CONTRIBUTING.md): proposing changes · [MCP.md](https://github.com/lexifina/global-law/blob/main/MCP.md): the MCP server · [REPRODUCE.md](https://github.com/lexifina/global-law/blob/main/REPRODUCE.md): how it was built · [CHANGELOG.md](https://github.com/lexifina/global-law/blob/main/CHANGELOG.md)
- [FRESHNESS.md](https://github.com/lexifina/global-law/blob/main/FRESHNESS.md): how current each corpus is · [Monitoring](https://github.com/lexifina/global-law/tree/main/monitoring): whether updates are running · [WATCHING.md](https://github.com/lexifina/global-law/blob/main/WATCHING.md): how each official source is watched for changes
- [performance/](https://github.com/lexifina/global-law/tree/main/performance): how fast the tools are, measured
- [visualisations/](https://github.com/lexifina/global-law/tree/main/visualisations): charts and bridges across the world's law
- [Vulnerabilities](https://github.com/lexifina/global-law/tree/main/vulnerabilities): mistakes in legislation as published
