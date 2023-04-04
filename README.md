# BabyKraken <img src='images/logo.png' align="right" height="210" />

A compact (10MB) Kraken2 database designed for seamless integration with pipelines and testing purposes.

## Overview
Kraken2 is a widely used tool for determining the taxonomic content of FASTQ reads. It is often utilized in conjunction with the pre-built Minikraken database. However, the Minikraken database, at 5.5 GB, is too large to be bundled with pipelines effectively. This creates a barrier for new users looking to set up a Kraken2 database for their pipeline. BabyKraken aims to address this issue by providing a small, yet practical, database that can be easily integrated into your pipeline, offering a more convenient solution for both testing and initial setup.

## Key Features
- Compact size: At only 10MB, BabyKraken is significantly smaller than the standard Minikraken database, making it easier to bundle with pipelines and reducing resource consumption.
- Simplified setup: BabyKraken streamlines the setup process for new users, eliminating the need for time-consuming downloads or installations of larger databases.
- Improved testing experience: BabyKraken is designed to facilitate more efficient and effective testing, enabling you to quickly evaluate and refine your pipeline as needed.

## Downlaod 

```bash
curl -L https://github.com/Wytamma/babykraken/blob/master/dist/babykraken.tar.gz?raw=true | tar xz
```

## Run

```
kraken2 --db ./babykraken sequences.fa
```

## Data 

BabyKraken contains genomes of bacteria frequently encountered in public health labs (see species list below). A table of all the genome accession numbers can be found in `data/species_acc_map.csv`.

```
Abiotrophia defectiva
Achromobacter xylosoxidans
Acinetobacter baumannii
Acinetobacter nosocomialis
Acinetobacter pittii
Bacillus cereus
Bacillus thuringiensis
Burkholderia cenocepacia
Burkholderia cepacia
Burkholderia contaminans
Burkholderia multivorans
Campylobacter coli
Campylobacter jejuni
Campylobacter lari
Campylobacter upsaliensis
Citrobacter farmeri
Citrobacter freundii
Clostridioides difficile
Clostridium perfringens
Corynebacterium diphtheriae
Corynebacterium kroppenstedtii
Enterobacter asburiae
Enterobacter bugandensis
Enterobacter cloacae
Enterobacter hormaechei
Enterobacter kobei
Enterobacter roggenkampii
Enterococcus faecalis
Enterococcus faecium
Escherichia albertii
Haemophilus influenzae
Hafnia paralvei
Helicobacter pylori
Herbaspirillum seropedicae
Klebsiella aerogenes
Klebsiella michiganensis
Klebsiella oxytoca
Klebsiella pneumoniae
Klebsiella quasipneumoniae
Klebsiella variicola
Legionella anisa
Legionella longbeachae
Legionella pneumophila
Listeria monocytogenes
Listeria seeligeri
Moraxella osloensis
Morganella morganii
Mycobacterium avium
Mycobacterium intracellulare
Mycobacterium tuberculosis
Mycobacterium ulcerans
Mycobacteroides abscessus
Mycoplasma hominis
Neisseria gonorrhoeae
Neisseria meningitidis
Nocardia brasiliensis
Nocardia cyriacigeorgica
Nocardia farcinica
Nocardia nova
Proteus mirabilis
Providencia rettgeri
Pseudomonas aeruginosa
Pseudomonas azotoformans
Pseudomonas monteilii
Pseudomonas otitidis
Pseudomonas putida
Salmonella enterica
Serratia marcescens
Shigella boydii
Shigella dysenteriae
Staphylococcus aureus
Staphylococcus capitis
Staphylococcus epidermidis
Staphylococcus lugdunensis
Staphylococcus saprophyticus
Stenotrophomonas maltophilia
Streptococcus agalactiae
Streptococcus dysgalactiae
Streptococcus pneumoniae
Streptococcus pseudopneumoniae
Streptococcus pyogenes
Vibrio alginolyticus
Vibrio cholerae
Vibrio parahaemolyticus
Yersinia enterocolitica
Yersinia frederiksenii
Yersinia pseudotuberculosis
```

## Rebuild 

To rebuild the Babykraken download all of the genomes in `species_acc_map.csv` from NCBI and save them in `data/genomes/`. 

### Create a custom kraken2 DB

```bash
kraken2-build --download-taxonomy --db $DBNAME
```

### Add the genomes to the library
```bash
for file in data/genomes/*.fna
do
    kraken2-build --add-to-library $file --db $DBNAME
done
```

### Build the DB 

Setting `--max-db-size` (10MB is 10485760).

```bash
kraken2-build --build --db $DBNAME --max-db-size 10485760
```

### Inspect 

```bash
kraken2-inspect --db ./babykraken | head -n20
```
```
# Database options: nucleotide db, k = 35, l = 31
# Spaced mask = 11111111111111111111111111111111110011001100110011001100110011
# Toggle mask = 1110001101111110001010001100010000100111000110110101101000101101
# Total taxonomy nodes: 309
# Table size: 1831041
# Table capacity: 2621440
# Min clear hash value = 18175836915879815168
100.00  1831041 0       R       1       root
100.00  1831041 0       R1      131567    cellular organisms
100.00  1831041 248     D       2           Bacteria
```

### Remove unneeded files

```bash
kraken2-build --clean --db $DBNAME
```

### Compress

```bash
tar czvf ./dist/babykraken.tar.gz babykraken/
```