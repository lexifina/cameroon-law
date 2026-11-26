# Structure du dépôt / Repository layout

Les noms de dossiers sont en français, la langue dans laquelle on cherche le droit camerounais.
Folder names are in French, the language people search Cameroonian law in.

| Dossier | Contenu |
| --- | --- |
| `constitution/` | La Constitution (loi n° 96/06 du 18 janvier 1996, modifiée par la loi n° 2008/001), un fichier par article |
| `codes/<code>/` | Les codes : `code-penal/`, `code-de-procedure-penale/`, `code-du-travail/`, `code-minier/`, ... ; un fichier par article (`art-001.md`), `visas.md`, `README.md` |
| `lois/<année>/<numéro>-<objet>/` | Les lois (et lois organiques), par année de promulgation |
| `ordonnances/<année>/<numéro>-<objet>/` | Les ordonnances |
| `decrets/<année>/<numéro>-<objet>/` | Les décrets du Président de la République et du Premier Ministre (`-pm` : décret du Premier Ministre) |
| `arretes/<année>/<numéro>-<objet>/` | Les arrêtés (du Premier Ministre et des ministres) |
| `decisions/<année>/<numéro>-<objet>/` | Les décisions réglementaires (Premier Ministre, ministres) |

Dans chaque dossier d'acte : `README.md` (intitulé, source, date, nature du texte), `visas.md` (visas et préambule),
`art-NNN.md` (un fichier par article ; `art-012-bis.md` pour un article bis), `signatures.md` (formule finale et
signatures) ; `texte-integral.md` quand la source ne découpe pas le texte en articles.

Aucun dossier ne contient plus de 1 000 entrées.

Un commit par version, daté à midi (Africa/Douala) du jour d'entrée en vigueur, avec les trailers `In-force` et
`In-force-basis` ; avant 1970, le commit est daté du 1er janvier 1970 et le vrai jour figure dans l'objet et dans
`In-force`. Les textes repris d'une copie secondaire portent `Source-status: secondary copy (...)` ; une couche texte
fournie par l'éditeur sur un document numérisé porte `Text-source: publisher OCR`.

`corpus.json` (`cm/national/acts`) liste chaque acte : dossier, nature, numéro, date, intitulé, source.
