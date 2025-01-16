# Merge data sets
To merge all datasets for each species, we follow these steps:

1. Transform the gene IDs of *P. falciparum* and *P. vivax* into *P. berghei* gene IDs by using the BioMart tool in the Ensembl Protists genome browser (release 60).
2. Access the BioMart tool at the following URL: https://protists.ensembl.org/biomart
3. Select "Protein Coding" as the gene type.
4. Optionally, choose "Protein Coding" as the transcript type.
5. Under "MULTI SPECIES COMPARISONS," apply the homologue filters to include only orthologous genes to *P. berghei*, ensuring to select unique results only.
6. Source of the gene should be "ena."

## Genes annotations files:
The downloaded annotation file for each for each species has attached as follows: 
1. p breghei  --> martquery_0116140127_444.txt.gz.
2. P.  falciparum 3D7 genes (ASM276v2) --> martquery_0116135016_882.txt.gz
3.  Plasmodium vivax genes (ASM241v2)  --> reference is martquery_0116140813_393.txt.gz. 

## Final gene IDs.
- Retain genes with 1:1:1 orthologues across the three species.