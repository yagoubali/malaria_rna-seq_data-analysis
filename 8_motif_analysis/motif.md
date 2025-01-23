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
for  fasta in ${fasta_files[@]}; do
   out_dir=$(echo ${fasta} | sed -e 's/.fasta//g')
   echo ${out_dir}
   mkdir -p streme_out/${out_dir}

   streme --verbosity 1 --oc streme_out/${out_dir}  --dna \
   --totallength 4000000 --time 14400 --minw 8 --maxw 15 --thresh 0.05 \
    ‑‑with‑db=${motif_databases}    \
   --align center --p ${input_dir}/${fasta}
   done
```

