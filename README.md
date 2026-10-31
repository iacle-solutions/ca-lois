# ca-law-fr — Recherche de lois canadiennes (version française)

**Parseurs verbatim pour Hermes Agent.** Aucune interprétation LLM — chaque adaptateur récupère la source officielle, la met en cache, et retourne le texte exact des sections avec les URLs sources.

---

## Contenu

```
scripts/
├── ca_law.py                    # Fédéral : Code criminel (XML Justice Laws)
├── provinces/                   # 12 adaptateurs provinciaux/territoriaux
│   ├── qc_law.py                # Québec : CCQ-1991, LPJ, etc. (HTML Légis Québec)
│   ├── on_law.py                # Ontario (API JSON e-Laws)
│   ├── bc_law.py                # Colombie-Britannique (HTML BC Laws)
│   ├── ab_law.py                # Alberta (PDF Imprimeur du Roi)
│   ├── sk_law.py                # Saskatchewan (publications.saskatchewan.ca)
│   ├── mb_law.py                # Manitoba (HTML web2.gov.mb.ca)
│   ├── ns_law.py                # Nouvelle-Écosse (PDF)
│   ├── nb_law.py                # Nouveau-Brunswick (HTML laws.gnb.ca)
│   ├── nl_law.py                # Terre-Neuve (HTML assembly.nl.ca)
│   ├── pe_law.py                # Île-du-Prince-Édouard (PDF)
│   ├── yt_law.py                # Yukon (PDF)
│   └── nt_law.py                # Territoires du Nord-Ouest (PDF)
└── youth_protection/            # Lois de protection de la jeunesse (13 juridictions)
    ├── qc_lpj.py                # Québec LPJ (P-34.1) — enveloppe provinces/qc_law.py ✓ VÉRIFIÉ
    ├── on_cyfsa.py              # Ontario CYFSA ✓ VÉRIFIÉ (art.1, 74, 101)
    ├── bc_cfcsa.py              # C.-B. CFCSA ✓ VÉRIFIÉ (art.4, 13, 30)
    ├── ab_cyfea.py              # Alb. CYFEA (ébauche → parseur PDF nécessaire)
    ├── sk_cfs.py                # Sask. CFS ✓ VÉRIFIÉ (art.11, 4, 17)
    ├── mb_cfs.py                # Man. CFS ✓ VÉRIFIÉ (art.17, 2.1, 21)
    ├── ns_cfs.py                # N.-É. CFSA ✓ VÉRIFIÉ (parseur PDF complet)
    ├── nb_fsa.py                # N.-B. LSF ✓ VÉRIFIÉ (art.31(1), 32(1), 1; abrogée 2023)
    ├── nl_cyfa.py               # T.-N.-L. LJEF ✓ VÉRIFIÉ (art.9, 10, 11)
    ├── pei_cpa.py               # Î.-P.-É. LPE ✓ VÉRIFIÉ (art.9, 2(2), 13; abrogée 2024)
    ├── yt_cfs.py                # Yukon CFSA ✓ VÉRIFIÉ (art.2, 21, 53)
    ├── nt_cfs.py                # T.N.-O. CFSA ✓ VERBATIM depuis PDF GNWT
    ├── nu_cfs.py                # Nunavut CFSA (ébauche → parseur HTML nécessaire)
    └── __init__.py              # Ré-exporte les adaptateurs fonctionnels
```

---

## Démarrage rapide

```bash
# Fédéral (Code criminel)
python -m scripts.ca_law C-46 266        # art. 266 Voies de fait
python -m scripts.ca_law C-46 145 --sub 4 --para a  # art. 145(4)(a)

# Québec (CCQ-1991, LPJ, etc.)
python -m scripts.provinces.qc_law CCQ-1991 1457   # CCQ art. 1457
python -m scripts.provinces.qc_law P-34.1 38       # LPJ art. 38 (motifs de compromission)

# Adaptateurs provinciaux
python -m scripts.provinces.on_law "Children's Law Reform Act" 21
python -m scripts.provinces.bc_law "Family Law Act" 37

# Protection de la jeunesse (services à l'enfance)
python -m scripts.youth_protection.qc_lpj 38          # LPJ art. 38
python -m scripts.youth_protection.nt_cfs 7           # T.N.-O. CFSA art. 7
python -m scripts.youth_protection.nt_cfs --search "intérêts supérieurs"
```

---

## Standard de vérification

Chaque adaptateur doit :
1. **Récupérer depuis la source gouvernementale officielle** (Justice Laws, Légis Québec, e-Laws, BC Laws, Imprimeur du Roi, etc.)
2. **Mettre en cache localement** (`~/.cache/ca_law/<adaptateur>/`)
3. **Retourner le texte verbatim** — pas de résumé, pas de réécriture
4. **Citer l'URL source** dans chaque réponse
5. **Exposer un CLI** avec `--search` et numéros de section

---

## Adaptateurs vérifiés (au 2026-10-31)

| Adaptateur | Source | Sections clés vérifiées |
|-----------|--------|------------------------|
| `ca_law.py` (fédéral) | laws.justice.gc.ca (XML) | C-46 art.266, 267, 145(4)(a) |
| `provinces/qc_law.py` | legisquebec.gouv.qc.ca | CCQ-1991 art.1457, LPJ P-34.1 art.2,3,38 |
| `provinces/on_law.py` | e-Laws Ontario | (structure vérifiée) |
| `provinces/bc_law.py` | bclaws.gov.bc.ca | (structure vérifiée) |
| `provinces/ab_law.py` | kings-printer.alberta.ca | (structure vérifiée) |
| `provinces/sk_law.py` | publications.saskatchewan.ca | (structure vérifiée) |
| `provinces/mb_law.py` | web2.gov.mb.ca | (structure vérifiée) |
| `provinces/ns_law.py` | nslegislature.ca | (structure vérifiée) |
| `provinces/nb_law.py` | laws.gnb.ca | (structure vérifiée) |
| `provinces/nl_law.py` | assembly.nl.ca | (structure vérifiée) |
| `provinces/pe_law.py` | princeedwardisland.ca | (structure vérifiée) |
| `provinces/yt_law.py` | laws.yukon.ca | (structure vérifiée) |
| `provinces/nt_law.py` | gnwt.ca | (structure vérifiée) |
| `youth_protection/qc_lpj.py` | legisquebec.gouv.qc.ca | LPJ art.2,3,38 ✓ |
| `youth_protection/on_cyfsa.py` | e-Laws Ontario | art.1 (intérêts supérieurs), 74 (définition protection), 101 (intervention) ✓ |
| `youth_protection/bc_cfcsa.py` | bclaws.gov.bc.ca | art.4 (intérêts supérieurs), 13 (définition protection), 30 (intervention) ✓ |
| `youth_protection/nb_fsa.py` | laws.gnb.ca | art.31(1) (mise en danger), 32(1) (protection), 1 (intérêts supérieurs) ✓ (abrogée 2023) |
| `youth_protection/nl_cyfa.py` | assembly.nl.ca | art.9 (intérêts supérieurs), 10 (définition protection), 11 (devoir signalement) ✓ |
| `youth_protection/pei_cpa.py` | princeedwardisland.ca | art.9 (définition protection), 2(2) (intérêts supérieurs), 13 (intervention) ✓ (abrogée 2024) |
| `youth_protection/sk_cfs.py` | publications.saskatchewan.ca | art.11 (définition protection), 4 (intérêts supérieurs), 17 (appréhension) ✓ |
| `youth_protection/yt_cfs.py` | laws.yukon.ca | art.2 (définition protection), 21 (devoir signalement), 53 (intérêts supérieurs) ✓ |
| `youth_protection/mb_cfs.py` | web2.gov.mb.ca | art.17 (définition protection), 2.1 (intérêts supérieurs), 21 (appréhension) ✓ |
| `youth_protection/ns_cfs.py` | nslegislature.ca | parseur PDF complet (art.1-109, historique amendements, proclamations) ✓ |
| `youth_protection/nt_cfs.py` | gnwt.ca (PDF) | art.3 (intérêts supérieurs), 7 (protection), 11 (appréhension) ✓ |

**Ébauches** (structure en place, parseurs nécessaires) : AB CYFEA, NU CFSA.

---

## Module protection de la jeunesse

Le module `youth_protection/` fournit une interface unifiée pour les lois de services à l'enfance dans les 13 juridictions. Les adaptateurs fonctionnels sont ré-exportés depuis `scripts.youth_protection` :

```python
from scripts.youth_protection import qc_get_article, qc_search, qc_key_articles
from scripts.youth_protection import nt_cfs  # a get_section(), search_keyword()
```

---

## Ajouter un nouvel adaptateur provincial

1. Créer `scripts/provinces/xx_law.py` en suivant le patron
2. Récupérer **uniquement** depuis le site gouvernemental officiel
3. Mettre en cache vers `~/.cache/ca_law/xx_law/`
4. Parser les sections verbatim
5. Exposer CLI : `python -m scripts.provinces.xx_law <loi> <section> | --search <mot-clé>`
6. Documenter dans ce README

---

## Licence

MIT — utilisez librement, mais **vérifiez toujours contre la source officielle** avant de vous fier aux résultats pour des questions juridiques.