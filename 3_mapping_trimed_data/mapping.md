# Required tools
1. STAR mapping tools [This link](https://github.com/alexdobin/STAR)
2. wget 
3. gzip
4. git
# Required files
1. Plasmodium berghei ANKA01
2. Plasmodium falciparum ASM276v2
3. Plasmodium vivax  ASM241v2
# Parameters
1. To index  Reference genome
```
--limitBAMsortRAM 50000000000
```
1. To Map reads into  Reference genome
# Scripts
1. Download reference and annotation files 
```bash
cd project_base_dir  ## change this

mkdir reference 

# plasmodium_berghei ANKA01
mkdir -p reference/plasmodium_berghei 
wget -c https://ftp.ensemblgenomes.ebi.ac.uk/pub/protists/release-60/gff3/plasmodium_berghei/Plasmodium_berghei.PBANKA01.60.gff3.gz.sorted.gz -P  ./reference/plasmodium_berghei
wget -c https://ftp.ensemblgenomes.ebi.ac.uk/pub/protists/release-60/gff3/plasmodium_berghei/Plasmodium_berghei.PBANKA01.60.gff3.gz -P  ./reference/plasmodium_berghei
wget -c http://ftp.ensemblgenomes.org/pub/protists/release-60/fasta/plasmodium_berghei/dna/Plasmodium_berghei.PBANKA01.dna.toplevel.fa.gz -P  ./reference/plasmodium_berghei
gzip -dk  ./reference/plasmodium_berghei/Plasmodium_berghei.PBANKA01.60.gff3.gz.sorted.gz
gzip -dk ./reference/plasmodium_berghei/Plasmodium_berghei.PBANKA01.60.gff3.gz
gzip -dk  ./reference/plasmodium_berghei/Plasmodium_berghei.PBANKA01.dna.toplevel.fa.gz


#Plasmodium falciparum ASM276v2 
mkdir -p reference/plasmodium_falciparum 
wget -c  http://ftp.ensemblgenomes.org/pub/protists/release-60/fasta/plasmodium_falciparum/dna/Plasmodium_falciparum.ASM276v2.dna.toplevel.fa.gz -P ./reference/plasmodium_falciparum 
wget -c  https://ftp.ensemblgenomes.ebi.ac.uk/pub/protists/release-60/gff3/plasmodium_falciparum/Plasmodium_falciparum.ASM276v2.60.gff3.gz   -P ./reference/plasmodium_falciparum 
 
 
gzip -dk  ./reference/plasmodium_falciparum/Plasmodium_falciparum.ASM276v2.dna.toplevel.fa.gz 
gzip -dk  ./reference/plasmodium_falciparum/Plasmodium_falciparum.ASM276v2.60.gff3.gz 
 
 

#Plasmodium vivax  ASM241v2
mkdir -p reference/plasmodium_vivax 
wget -c http://ftp.ensemblgenomes.org/pub/protists/release-60/fasta/plasmodium_vivax/dna/Plasmodium_vivax.ASM241v2.dna.toplevel.fa.gz -P ./reference/plasmodium_vivax 
wget -c https://ftp.ensemblgenomes.ebi.ac.uk/pub/protists/release-60/gff3/plasmodium_vivax/Plasmodium_vivax.ASM241v2.60.gff3.gz -P ./reference/plasmodium_vivax 


gzip -dk ./reference/plasmodium_vivax/Plasmodium_vivax.ASM241v2.dna.toplevel.fa.gz
gzip -dk ./reference/plasmodium_vivax/Plasmodium_vivax.ASM241v2.60.gff3.gz

```

# Download STAR
```bash
cd project_base_dir  ## change this
git clone https://github.com/alexdobin/STAR.git
```

# Index reference genome
```bash
cd project_base_dir  ## change this
star_bin="./STAR/bin/Linux_x86_64"
reference_dir="./reference"

run_index(){
genome_dir=$1
genome_fasta=$2
gff3=$3

${star_bin}/STAR --runThreadN 6 \
--runMode genomeGenerate  \
--genomeDir ${genome_dir}   \
--genomeFastaFiles ${genome_dir}/${genome_fasta}  \
--sjdbGTFfile ${genome_dir}/${gff3} \
--limitBAMsortRAM 50000000000 \
--genomeSAindexNbases 11

}
run_index ${reference_dir}/plasmodium_berghei  Plasmodium_berghei.PBANKA01.dna.toplevel.fa Plasmodium_berghei.PBANKA01.60.gff3
run_index ${reference_dir}/plasmodium_falciparum Plasmodium_falciparum.ASM276v2.dna.toplevel.fa Plasmodium_falciparum.ASM276v2.60.gff3
run_index ${reference_dir}/plasmodium_vivax Plasmodium_vivax.ASM241v2.dna.toplevel.fa  Plasmodium_vivax.ASM241v2.60.gff3

```
