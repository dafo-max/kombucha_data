# PilotLab Zealand – kombucha-undersøgelse 2025–2026

Dette repository indeholder anonymiserede rådata fra PilotLab Zealands undersøgelse af danske kombuchaer og SCOBY'er, foretaget på Zealand – Sjællands Erhvervsakademi i forbindelse med projektet "Undersøgelse af indholdet af probiotiske mikroorganismer og alkohol i kommercielle, dansk-producerede, upasteuriserede kombucha".

Data ligger til grund for artiklen *PilotLab Zealand undersøger kombucha*.

## Indhold

| Fil | Beskrivelse |
|---|---|
| `data/kemiske_data.csv` | Kemiske målinger på 33 kombuchaer og 7 SCOBY'er. Én række pr. bestemmelse (2–4 bestemmelser pr. prøve). |
| `data/ITS_gaer_reads.csv` | Antal sekvenser pr. svampetaxon (ITS) for 20 prøver. |
| `data/16S_bakterier_reads.csv` | Antal sekvenser pr. bakterietaxon (16S) for 13 prøver. |

## Anonymisering

Producenterne er anonymiseret og angivet med bogstaverne A–H. Produktnavne, smagsvarianter, opskrifts- og procesoplysninger, prøvedatoer og producenternes egne målinger er fjernet. Prøverne er angivet med et løbenummer (K1–K40), som går igen på tværs af alle tre filer.

## Metoder

### Kemiske analyser

Densitet, alkoholindhold (% vol., ABV), ekstrakt (°Plato) og sukkerindhold (Brix) blev målt på Anton Paar DMA 4500 med Alcolyzer Plus Beer.

Sukkerindholdet er beregnet som g sukrose/100 mL (Brix × densitet). Hvis der er andre sukkerarter til stede, f.eks. glukose eller fruktose, vil tallet afvige en smule.

Totalt syreindhold blev bestemt ved titrering med 0,100 M natriumhydroxid (NaOH) på Titrator EasyPlus Easy pH (Mettler Toledo). Syreindholdet er beregnet ud fra mængden af tilsat NaOH og angivet som g eddikesyre/100 mL, under forudsætning af at al syre i produktet er eddikesyre. Dette er en tilnærmelse, da der typisk også vil være andre organiske syrer til stede. pH blev målt på samme instrument.

Alle målinger er udført som dobbeltbestemmelse (n = 2) eller flere.

### Mikrobiologiske analyser

DNA blev oprenset med Quick-DNA™ Fungal/Bacterial Miniprep Kit (Zymo Research). PCR målrettet ITS-regionen (gær/svampe) eller 16S-regionen (bakterier) blev udført med VWR® TEMPase Hot Start DNA Polymerase (VWR). Sekventeringen blev kørt på en MinION Flow Cell (FLO-MIN114) på MinION Mk1D (Oxford Nanopore Technologies).

Den taksonomiske klassificering blev udført med Snakemake-pipelinen emu-smk (AU-ENVS-Bioinformatics, https://github.com/AU-ENVS-Bioinformatics/emu-smk), som kører EMU (Curry et al., 2022). EMU estimerer den taksonomiske sammensætning med en expectation-maximization-algoritme, der er udviklet til fuld-længde amplikonsekvenser fra Oxford Nanopore. Antal sekvenser pr. taxon i datafilerne er derfor EMU's estimerede antal og kan være decimaltal. Der blev brugt EMU v3.4.4 med standardindstillinger. Referencedatabaser: SILVA v138.1 (16S, bakterier; https://www.arb-silva.de/) og UNITE general FASTA release v8.3, fungi (ITS, svampe; https://unite.ut.ee/repository.php, https://dx.doi.org/10.15156/BIO/1280049). Bemærk, at EMU's udviklere angiver, at UNITE-databasen endnu ikke er testet eller valideret til brug med EMU.

Relativ forekomst, som vist i artiklen, er beregnet som antal sekvenser for et taxon divideret med det samlede antal klassificerede sekvenser i prøven (ekskl. rækken `unassigned`) × 100.

En del af de modtagne prøver var filtrerede, og der kunne derfor ikke påvises DNA i dem. Det er grunden til, at ikke alle prøver indgår i sekventeringsdata.

## Kolonnebeskrivelse

### kemiske_data.csv

| Kolonne | Enhed | Beskrivelse |
|---|---|---|
| `proeve_id` | – | Prøvenummer (K1–K40) |
| `producent` | – | Anonymiseret producent (A–H) |
| `proevetype` | – | `kombucha` eller `SCOBY` |
| `filtreret` | – | Oplyst af producent: `ja`/`nej`. Tom = ikke oplyst |
| `pasteuriseret` | – | Oplyst af producent: `ja`/`nej`. Tom = ikke oplyst |
| `temp_C` | °C | Måletemperatur i Alcolyzer |
| `ABV_pct` | % vol. | Alkoholindhold (mL ethanol/100 mL) |
| `plato_original` | °P | Original ekstrakt |
| `plato_real` | °P | Reelt ekstrakt |
| `densitet_g_mL` | g/mL | Densitet |
| `sg_20_20` | – | Massefylde (specific gravity, 20/20 °C) |
| `brix_g_100g` | g/100 g | Sukker som g sukrose/100 g |
| `sukker_g_100mL` | g/100 mL | Sukker som g sukrose/100 mL (Brix × densitet) |
| `NaOH_mL` | mL | Forbrug af 0,100 M NaOH ved titrering |
| `TA_g_eddikesyre_100mL` | g/100 mL | Totalt syreindhold beregnet som eddikesyre |
| `pH` | – | pH |

Tomme felter betyder, at målingen ikke blev udført for den pågældende bestemmelse.

### ITS_gaer_reads.csv og 16S_bakterier_reads.csv

Kolonnerne `tax_id` til `species` angiver taksonomien (NCBI taxonomy ID, rige, række, klasse, orden, familie, slægt og art). Prøvekolonnerne (K…) angiver antal sekvenser tilskrevet det pågældende taxon. `NA` betyder, at taxonet ikke kunne bestemmes på det niveau. Rækken `unassigned` angiver sekvenser, der ikke kunne klassificeres.

## Referencer

Curry, K. D., Wang, Q., Nute, M. G., Tyshaieva, A., Reeves, E., Soriano, S., Wu, Q., Graeber, E., Finzer, P., Mendling, W., Savidge, T., Villapol, S., Dilthey, A., & Treangen, T. J. (2022). Emu: species-level microbial community profiling of full-length 16S rRNA Oxford Nanopore sequencing data. *Nature Methods*, 19, 845–853. https://doi.org/10.1038/s41592-022-01520-4

Quast, C., Pruesse, E., Yilmaz, P., Gerken, J., Schweer, T., Yarza, P., Peplies, J., & Glöckner, F. O. (2013). The SILVA ribosomal RNA gene database project: improved data processing and web-based tools. *Nucleic Acids Research*, 41(D1), D590–D596.

Nilsson, R. H., Larsson, K.-H., Taylor, A. F. S., Bengtsson-Palme, J., Jeppesen, T. S., Schigel, D., Kennedy, P., Picard, K., Glöckner, F. O., Tedersoo, L., Saar, I., Kõljalg, U., & Abarenkov, K. (2019). The UNITE database for molecular identification of fungi: handling dark taxa and parallel taxonomic classifications. *Nucleic Acids Research*, 47(D1), D259–D264.

UNITE Community (2021). UNITE general FASTA release for Fungi. Version 10.05.2021. UNITE Community. https://doi.org/10.15156/BIO/1280049

## Forbehold

PilotLab Zealand er ikke et akkrediteret laboratorium, og resultaterne er ikke fagfællebedømt. Metoderne er dog anerkendte i den videnskabelige litteratur, og udstyret er kalibreret efter gældende forskrifter.

## Kontakt

PilotLab Zealand: pilotlab@zealand.dk

## Licens

Data er udgivet under licensen [Creative Commons Navngivelse 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/deed.da). Data må frit deles og bearbejdes, også kommercielt, så længe der gives kildeangivelse.

Kildeangivelse: PilotLab Zealand, Zealand – Sjællands Erhvervsakademi (2026). Rådata: PilotLab Zealand undersøger kombucha. https://github.com/dafo-max/kombucha-data
