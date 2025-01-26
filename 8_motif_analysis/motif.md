# Required Software
1. [PlasmoDB](https://plasmodb.org/plasmo/app/fasta-tool/transcript)
2. [meme-suite](https://meme-suite.org/meme/tools/streme)
3. meme-5.5.7.tar.gz

# Required files
1.  5’UTR sequences 1000 bp upstream of the start codons of the genes were extracted from PlasmoDB

# Scripts


## 1. Download motifs
```bash
mkdir motifDB
cd motifDB/
wget -c https://meme-suite.org/meme/meme-software/Databases/motifs/motif_databases.12.25.tgz
wget -c https://meme-suite.org/meme/meme-software/Databases/gomo/gomo_databases.3.2.tgz
wget -c https://meme-suite.org/meme/meme-software/Databases/tgene/tgene_databases.1.0.tgz
tar -xvzf motif_databases.12.25.tgz
tar -xvzf gomo_databases.3.2.tgz
tar -xvzf tgene_databases.1.0.tgz
```

```bash
fasta_files=("Cluster1.Sub1.fasta"  "Cluster2.Sub2.fasta"  "Cluster3.Sub2.fasta" \
             "Cluster1.Sub2.fasta"  "Cluster2.Sub3.fasta"  "Cluster3.Sub3.fasta"  \
             "Cluster1.Sub3.fasta"  "Cluster2.Sub4.fasta"  "Cluster3.Sub4.fasta"  \
             "Cluster1.Sub4.fasta"  "Cluster2.Sub5.fasta"  "Cluster3.Sub5.fasta"  \
             "Cluster2.Sub1.fasta"  "Cluster3.Sub1.fasta"  "Cluster3.Sub6.fasta")

input_dir="UTR_1000"
motif_databases="/mnt/sdb3/adaobi/motifDB/motif_databases"
# streme
for  fasta in ${fasta_files[@]}; do
   out_dir=$(echo ${fasta} | sed -e 's/.fasta//g')
   echo ${out_dir}
   mkdir -p streme_out/${out_dir}

   streme --verbosity 1 --oc streme_out/${out_dir}  --dna \
   --totallength 4000000 --time 14400 --minw 8 --maxw 15 --thresh 0.05 \
    ‑‑with‑db=${motif_databases}    \
   --align center --p ${input_dir}/${fasta}
   done
# meme

for  fasta in ${fasta_files[@]}; do
   out_dir=$(echo ${fasta} | sed -e 's/.fasta//g')
   echo ${out_dir}
   mkdir -p meme_out_15/${out_dir}

   meme ${input_dir}/${fasta}  -dna -oc  meme_out_15/${out_dir}  \
   -nostatus -time 14400 -mod zoops -nmotifs 15 -minw 6 -maxw 50 -objfun classic -revcomp -markov_order 0
   done

# meme-chip

for  fasta in ${fasta_files[@]}; do
   out_dir=$(echo ${fasta} | sed -e 's/.fasta//g')
   echo ${out_dir}
   mkdir -p meme_chip_out/${out_dir}

   meme-chip -oc meme_chip_out/${out_dir}  -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
    -db ${motif_databases}/PROKARYOTE/prodoric_2021.9.meme \
    -db ${motif_databases}/PROKARYOTE/collectf.meme \
    -db ${motif_databases}/PROKARYOTE/regtransbase.meme \
    -meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
    -streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
    -centrimo-score 5.0 -centrimo-ethresh 10.0 ${input_dir}/${fasta}

   done
```

