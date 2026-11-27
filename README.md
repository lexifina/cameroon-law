# Cameroun : l’historique du droit dans git

**Français** · [English](README.en.md)

<picture>
<source media="(prefers-color-scheme: dark)" srcset="data/fr/effort-dark.svg">
<img alt="Le droit camerounais dans ce dépôt : ce qu’il contient, les modifications par année d’entrée en vigueur, l’exhaustivité au regard des sources officielles et l’exactitude de son contenu" src="data/fr/effort-light.svg" width="880">
</picture>

Le droit camerounais sous forme d’historique git : un fichier par article, chaque version enregistrée au jour de son entrée en vigueur, et pour auteur de chaque commit la loi qui a apporté la modification. `git blame` désigne la loi à l’origine de chaque alinéa ; `git log` énumère toutes les modifications d’un article ; le choix d’une date donne le texte en vigueur ce jour-là.

Fait partie de [lexifina/global-law](https://github.com/lexifina/global-law), un historique git unique pour le droit de chaque pays : ce dépôt contient le droit national camerounais, que global-law lit dans `cameroon/national/acts/`.

**Contenu actuel :** 449 textes, chacun dans la version que publie sa source : la Constitution ; 9 codes ([code-de-la-marine-marchande](codes/code-de-la-marine-marchande/), [code-de-procedure-penale](codes/code-de-procedure-penale/), [code-du-travail](codes/code-du-travail/), [code-electoral](codes/code-electoral/), [code-gazier](codes/code-gazier/), [code-minier](codes/code-minier/), [code-penal](codes/code-penal/), [code-petrolier](codes/code-petrolier/)) ; 83 lois, 7 ordonnances, 204 décrets, 115 arrêtés et 30 décisions, rangés par nature et par année ([STRUCTURE.md](STRUCTURE.md), liste complète dans [`corpus.json`](corpus.json)). S’y ajoutent 0 versions anglaises officielles (sous-dossier `en/` du texte) et 2 textes de droit communautaire applicables au Cameroun (`cemac/`, `ohada/`). Les décisions de justice (OHADA) sont dans un dépôt séparé.

**Ce n’est pas le texte en vigueur aujourd’hui.** La loi n°2026/002 du 14 avril 2026 a de nouveau modifié la Constitution et n’est pas appliquée (voir [Ce qu’il ne montre pas](#ce-quil-ne-montre-pas)) ; pour tout jour postérieur au 14 avril 2008, ce registre donne donc le texte de 2008.

Chaque article de la Constitution est un fichier dans `constitution/` (`art-006.md` est l’article 6, relatif à l’élection et au mandat du Président de la République) ; le préambule est `preambule.md`, et [`constitution/structure.md`](constitution/structure.md) indique l’article que précède chaque intitulé de titre et de chapitre. Les autres textes sont rangés par nature et par année (`lois/2016/…`, `decrets/2018/…`) ; les codes sont dans `codes/<code>/`, un fichier par article (`codes/code-penal/art-074.md`). Les deux lois de 1991 et 2001 restent chacune un fichier unique, `texte.md` (`lois/1991/91-022-rehabilitation/`, `lois/2001/2001-016-chambres-consulaires/`).

Le texte est en français, tel que l’impriment ses éditeurs. Le Cameroun publie ses lois en français et en anglais ; le texte anglais ne fait pas partie de ce registre.

## Questions sous forme de commandes git

| Question | Commande |
| --- | --- |
| Toutes les modifications de l’article 6 de la Constitution (le mandat du Président) | `git log -p --word-diff -- constitution/art-006.md` |
| Quelle loi a écrit chaque ligne de l’article 6 ? | `git blame constitution/art-006.md` |
| Toutes les versions de l’article 6, avec le jour d’entrée en vigueur de chacune | `git log --format="%h %(trailers:key=In-force,valueonly,separator=) %an" -- constitution/art-006.md` |
| L’article 6 tel qu’en vigueur le 1er janvier 2020 | `python tools/en_vigueur_au.py 2020-01-01 constitution 6` |

**En dehors de git**, nous hébergeons aussi ce registre publiquement, gratuitement :

- **[Interroger Claude sur le droit camerounais](https://global-law.lexifina.com/v1/try_claude?country=cameroon)** : ouvre Claude avec une première question prête.
- **[Interroger ChatGPT](https://global-law.lexifina.com/v1/try_chatgpt?country=cameroon)** : la même question dans ChatGPT.
- **[Rechercher dans le droit camerounais sur notre site](https://lexifina.com/law?country=cameroon&q=droit+de+gr%C3%A8ve)** : une recherche de « droit de grève », avec le texte de chaque disposition et ses versions, sans compte.
- **[API REST pour les développeurs](https://global-law.lexifina.com/v1/search_law?query=droit+de+gr%C3%A8ve&jurisdiction=cameroon)** : la même recherche en JSON, sans clé. Voir la [documentation ici](https://lexifina.com/api/global-law).

Les deux assistants lisent le registre au moyen du connecteur gratuit Global law (MCP, `https://global-law.lexifina.com/mcp`) ; la première fois, la conversation vous donne le lien qui l’ajoute. Posez vos questions en français ou en anglais.

**L’héberger vous-même, simplement.** Il suffit de git et de Python, et les outils s’exécutent sur votre machine :

```sh
git clone --depth 1 https://github.com/lexifina/global-law && cd global-law
python3 tools/countries.py add cameroon/national/acts
python3 tools/law.py show 2026-01-01 cameroon/national/acts/constitution/art-006.md
```

Ouvert dans Claude Code, Cursor, VS Code ou Codex, le clone lance aussi un serveur MCP local doté des mêmes outils ([MCP.md](https://github.com/lexifina/global-law/blob/main/MCP.md)).

## Dates

Chaque commit est daté à midi, heure de Douala (Africa/Douala), du jour où la version est entrée en vigueur, et porte ce jour dans son trailer `In-force:` :

- la Constitution : le 14 avril 2008, date de promulgation de la loi n°2008/001, telle que l’indique le titre de la version consolidée ;
- la loi n°91/022 et la loi n°2001/016 : le 16 décembre 1991 et le 23 juillet 2001, dates de promulgation qu’indiquent leurs textes ;
- les autres textes : la date du texte (signature ou promulgation) que donne son intitulé ; les lois camerounaises sont publiées selon la procédure d’urgence et s’appliquent dès leur promulgation, sauf disposition contraire (trailer `In-force-basis`) ; avant 1970, le commit est daté du 1er janvier 1970 et le vrai jour figure dans l’objet et dans `In-force` ;
- les autres textes : la date du texte (signature ou promulgation) que donne son intitulé ; les lois camerounaises sont publiées selon la procédure d’urgence et s’appliquent dès leur promulgation, sauf disposition contraire (trailer `In-force-basis`) ; avant 1970, le commit est daté du 1er janvier 1970 et le vrai jour figure dans l’objet et dans `In-force` ;
- les autres textes : la date du texte (signature ou promulgation) que donne son intitulé ; les lois camerounaises sont publiées selon la procédure d’urgence et s’appliquent dès leur promulgation, sauf disposition contraire (trailer `In-force-basis`) ; avant 1970, le commit est daté du 1er janvier 1970 et le vrai jour figure dans l’objet et dans `In-force` ;
- les lois sont publiées au Journal officiel en français et en anglais (article 31 de la Constitution), mais le Journal officiel n’est pas librement accessible ; aucune date de publication n’a donc été trouvée.

Chaque premier commit est attribué à son éditeur en tant que plus ancienne version disponible, et nomme les lois dont le texte est issu : la Chambre des comptes de la Cour suprême pour la Constitution (loi n°96/06 et loi n°2008/001), la Présidence de la République pour les deux lois. Le committer est toujours Lexifina.

## Comment il est vérifié

- `tools/check_repo.py` relit chaque version depuis git au jour de son entrée en vigueur et la veille, et les deux doivent correspondre au corpus à partir duquel l’historique a été construit ; il vérifie aussi que les trailers de chaque commit se lisent sans erreur et qu’il n’existe aucun commit de fusion.
- `tools/build.py` écrit l’historique à partir de ce corpus en une seule passe ; une seconde construction dans un dossier vide a donné des empreintes identiques. Les nouveaux textes sont ajoutés au-dessus de `main` par `tools/append.py`, qui ne réécrit rien. Les résultats figurent dans [`REPRODUCE.md`](REPRODUCE.md), et [`data/corpus.sha256`](data/corpus.sha256) donne l’empreinte du corpus.
- La Constitution est la couche texte propre au PDF (le fichier a été produit avec Word, et non numérisé), comparée caractère par caractère avec une seconde extraction indépendante ; les textes HTML officiels des deux lois ont été lus par deux analyseurs indépendants, et chaque ligne concordait.
- [vulnerabilities/](vulnerabilities/): erreurs de rédaction possibles dans ce droit, trouvées par les détecteurs de global-law et vérifiées à la main. Pas encore lancé.

## Ce qu’il ne montre pas

- **La Constitution en vigueur aujourd’hui.** La loi n°2026/002 du 14 avril 2026 l’a modifiée, mais elle n’est publiée que sous forme de numérisation, et les numérisations ne sont pas transcrites.
- **La Constitution avant 2008** : la Constitution de 1972, sa révision de 1996 telle qu’adoptée et les termes que la loi n°2008/001 a remplacés ; les exemplaires des deux lois tenus par la Présidence sont des numérisations.
- **Le texte anglais**, qui fait foi au même titre que le texte français (article 1er) : aucun exemplaire anglais officiel doté d’une couche texte n’a été trouvé.
- **Les textes publiés seulement en numérisation** (829 à ce jour) : aucune OCR n’est faite ici ; une couche texte fournie par l’éditeur est reprise (62 textes, trailer `Text-source: publisher OCR`, non relue). Le site de la Présidence (prc.cm) ne nous répondait pas le 26 novembre 2026 (284 adresses connues en attente). Le Code de procédure pénale ne compte que les articles 59 à 672, seuls publiés en texte par les Services du Premier Ministre. Le Code civil, le Code CEMAC et les actes uniformes OHADA (copies disponibles seulement en OCR de tiers) manquent.
- **Les modifications des codes et des lois** : chaque texte est donné dans la version publiée par la source ; les lois modificatives sont des textes distincts et ne sont pas appliquées au texte modifié.
- **Les intitulés et les mentions.** Les intitulés de titres et de chapitres figurent dans `structure.md`, et non dans les fichiers des articles ; les mentions « (nouveau) » de la version consolidée, qui signalent les dispositions modifiées par la loi n°2008/001, ainsi que ses numéros de page, sont omis. Les coquilles sont conservées telles qu’imprimées.

## Sources

| Source | Nature | Versions antérieures | Remarques |
| --- | --- | --- | --- |
| [Chambre des comptes de la Cour suprême](https://chambredescomptes.cm) | Le texte français consolidé de la Constitution, un PDF Word de 30 pages, tel que conservé par l’Internet Archive le 22 avril 2024 | non, le seul texte de 2008 | Le texte de la Constitution et sa date |
| [Présidence de la République](https://www.prc.cm/fr/actualites/actes/lois) | Pages officielles des lois : textes HTML et PDF numérisés | non, chaque texte tel que publié | Les textes et les dates de la loi n°91/022 et de la loi n°2001/016, ainsi que le titre et la date de la loi n°2008/001 ; la loi n°2026/002 est une numérisation, non utilisée |
| [Services du Premier Ministre](https://www.spm.gov.cm/site/?q=fr/documentation/lois-et-r%C3%A8glements) | « Lois et Règlements » : textes HTML et PDF (lois, décrets, arrêtés, décisions) | non, chaque texte tel que publié | Source officielle ; le Code de procédure pénale (articles 59 à 672) en HTML ; les PDF numérisés sans couche texte sont écartés |
| [FAOLEX](https://www.fao.org/faolex/) (FAO) | Copies PDF des textes publiés, avec leur couche texte | non | Copie secondaire (trailer `Source-status`) : Code du travail, Code pénal, codes minier, pétrolier, forestier, foncier et domanial, etc. ; réutilisation non commerciale avec mention de la FAO |
| [WIPO Lex](https://www.wipo.int/wipolex/) (OMPI) | Copies PDF de quelques lois | non | Copie secondaire (trailer `Source-status`) |
| Hugging Face `endomorphosis/ipfs_cameroon_laws` | Liste d’adresses seulement | – | Sert uniquement de liste d’adresses : chaque texte est relu à sa source officielle ; aucun texte de ce jeu n’est repris |
| [Assemblée nationale](https://www.assnat.cm/index.php/fr/lois/lois-adoptees) | « Lois adoptées » : PDF des lois promulguées, en français et en anglais | non | Source officielle ; la plupart des PDF sont des numérisations sans couche texte, écartées |
| [Ministère de la Justice](http://www.minjustice.gov.cm/index.php/fr/legalis/textes-de-lois) | « Textes et lois » (français) et « Instruments and laws » (anglais) : PDF | non | Source officielle ; numérisations écartées |
| [Cour suprême](https://www.coursupreme.cm/publication/textes-et-lois) | « Textes et lois » : PDF natifs | non | Source officielle |
| Ministères et autorités de régulation (MINDDEVEL, MINPROFF, MINEDUB, MINMIDT, MINEE, ARSEL, ART, ARMP, CNPS, CNC, DGI) | Pages de textes juridiques : PDF | non | Sources officielles ; les mesures individuelles (nominations, autorisations, sanctions) sont écartées |
| [Direction générale des Impôts](https://www.impots.cm/fr/documents/39) | Éditions officielles du Code général des impôts (2019 à 2026), lois de finances | oui : une version par édition datée (« mis à jour au 1er janvier … ») | Source officielle ; `codes/code-general-des-impots/` |
| [Commission de la CEMAC](https://cemac.int/reglements-directives/), [BEAC](https://www.beac.int/) | Règlements, directives et actes additionnels de la CEMAC (droit communautaire applicable au Cameroun) | non | Sources officielles ; `cemac/` ; numérisations écartées |
| [OHADA](https://www.ohada.org/) | Journal officiel de l’OHADA : actes uniformes (sûretés ; sociétés coopératives) | non | Source officielle ; `ohada/actes-uniformes/` ; les autres numéros du JO sont des numérisations |

## Licence

Voir [`LICENSE`](LICENSE). Les textes sont des textes législatifs officiels de la République du Cameroun, reproduits avec mention de leur source (Chambre des comptes de la Cour suprême, Présidence de la République, Services du Premier Ministre, FAOLEX, WIPO Lex) ; les éditeurs n’indiquent aucune licence, et le statut de ces textes au regard du droit d’auteur n’est pas confirmé ici. L’historique, les messages de commit, les tableaux et les outils sont sous CC0 1.0.

Aucun modèle d’IA n’a écrit, traduit ou corrigé de texte juridique, et aucun n’est exécuté lors de la construction de l’historique ; les scripts ont été écrits par un modèle d’IA (voir [`REPRODUCE.md`](REPRODUCE.md)).

Ceci est une traduction de la version anglaise ([README.en.md](README.en.md)) ; en cas de divergence, la version anglaise fait foi.

## Visualisations

- [visualisations/](visualisations/): graphiques tirés de ce dépôt. Pas encore dessinés.

## En savoir plus

Dans le dépôt [lexifina/global-law](https://github.com/lexifina/global-law) (pages en anglais) :

- [FEATURES.md](https://github.com/lexifina/global-law/blob/main/FEATURES.md): toutes les fonctionnalités et ce que chaque corpus doit prendre en charge (`git blame`, `law.py`, outils MCP)
- [FAQ.md](https://github.com/lexifina/global-law/blob/main/FAQ.md): questions et réponses · [CONTRIBUTING.md](https://github.com/lexifina/global-law/blob/main/CONTRIBUTING.md): proposer une modification · [MCP.md](https://github.com/lexifina/global-law/blob/main/MCP.md): serveur MCP · [REPRODUCE.md](https://github.com/lexifina/global-law/blob/main/REPRODUCE.md): comment il a été construit · [CHANGELOG.md](https://github.com/lexifina/global-law/blob/main/CHANGELOG.md)
- [FRESHNESS.md](https://github.com/lexifina/global-law/blob/main/FRESHNESS.md): à quel point chaque corpus est à jour · [Surveillance](https://github.com/lexifina/global-law/tree/main/monitoring): si les mises à jour fonctionnent · [WATCHING.md](https://github.com/lexifina/global-law/blob/main/WATCHING.md): comment chaque source officielle est surveillée pour repérer les modifications
- [performance/](https://github.com/lexifina/global-law/tree/main/performance): la rapidité des outils, mesurée
- [visualisations/](https://github.com/lexifina/global-law/tree/main/visualisations): graphiques et liens à travers le droit du monde entier
- [Vulnérabilités](https://github.com/lexifina/global-law/tree/main/vulnerabilities): erreurs dans les textes tels qu’ils ont été publiés
