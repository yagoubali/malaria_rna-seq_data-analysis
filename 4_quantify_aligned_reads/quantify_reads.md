# Required tools
- Subread [Release 2.0.8, 4 November 2024](https://subread.sourceforge.net/)
# Required files
- NA

# Parameters
- [featureCounts documentation] (https://subread.sourceforge.net/featureCounts.html)
```
# Perform strand-specific read counting (use '-s 2' if reversely stranded):
featureCounts -s 1 -t exon -g gene_id -a annotation.gtf -o counts.txt mapping_results_SE.bam
# Summarize paired-end reads and count fragments (instead of reads):
featureCounts -p --countReadPairs -t exon -g gene_id -a annotation.gtf -o counts.txt mapping_results_PE.bam
```
# Scripts
1. Install Subread
```bash
cd project_base_dir  ## change this

wget -c https://sourceforge.net/projects/subread/files/subread-2.0.8/subread-2.0.8-Linux-x86_64.tar.gz
tar -xvzf subread-2.0.8-Linux-x86_64.tar.gz
```

2. Reads quantification 
```bash
cd project_base_dir  ## change this
#PE Truseq  Pb
ERP004740=("ERR435801" "ERR435802" "ERR435803" "ERR435804" "ERR435805" "ERR435806" "ERR435807" "ERR435808")

#PE Nextera PbANKA
ERP105548=("ERR2216044" "ERR2216045" "ERR2216046" "ERR2216047" "ERR2216048" "ERR2216049" "ERR2216050" "ERR2216051" "ERR2216052" "ERR2216053" "ERR2216054" "ERR2216055" "ERR2216056" "ERR2216057")

#SE Truseq  Pb
SRP027529=("SRR935544" "SRR935549" "SRR935553" "SRR935554" "SRR935555" "SRR935557" "SRR935558" "SRR935559" "SRR935560")

#PE TruSeq  Pv
SRP046739=("SRR1571697" "SRR1571698" "SRR1571699" "SRR1571700" "SRR1571701" "SRR1571702" "SRR1571703" "SRR1571704" "SRR1571705" "SRR1571706" "SRR1571707" "SRR1571708" "SRR1571709" "SRR1571710" "SRR1571711" "SRR1571712" "SRR1571713" "SRR1571714" "SRR1571715") 

#PE   Truseq  Pf K1
SRP048710=("SRR1605330" "SRR1605331" "SRR1605332" "SRR1605333" "SRR1605334" "SRR1605335" "SRR1605336" "SRR1605337" "SRR1605338" "SRR1605339" "SRR1605340" "SRR1605341" "SRR1605342" "SRR1605343" "SRR1605344" "SRR1605345" "SRR1605346" "SRR1605347")

#SE  TruSeq  Pf 3D7
SRP069075=("SRR3134662" "SRR3134663" "SRR3134664" "SRR3134665" "SRR3134666" "SRR3134667")


#PE NEBNext PbANKA
SRP073801=("SRR3437887" "SRR3437888" "SRR3437889" "SRR3437890" "SRR3437891" "SRR3437892" "SRR3437893" "SRR3437894" "SRR3437895" "SRR3437896" "SRR3437897" "SRR3437898")

#SE  Truseq   Pf NF54 and 3D7
SRP090611=("SRR4302519" "SRR4302520" "SRR4302521" "SRR4302522" "SRR4302523" "SRR4302524" "SRR4302525" "SRR4302526" "SRR4302527" "SRR4302528" "SRR4302529" "SRR4302530" "SRR4343872" "SRR4343873" "SRR4343875" "SRR4343876" "SRR4343877" "SRR4343878" "SRR4343879" "SRR4343880" "SRR5146153" "SRR5146154" "SRR5146155" "SRR5146214" "SRR5146215" "SRR5146216" "SRR5146217" "SRR5146218" "SRR5146305" "SRR5146306" "SRR5146307" "SRR5146308" "SRR5146379" "SRR5146380" "SRR5150405" "SRR5150407" "SRR5150408" "SRR5150409" "SRR5150410") 

#PE ABS (mixed) PbANKA
SRP099925=("SRR5260544" "SRR5260545" "SRR5260546" "SRR5260547" "SRR5260548" "SRR5260549" "SRR5260550" "SRR5260551" "SRR5260552") 

#SE TruSeq  Pv
SRP100893=("SRR5298122" "SRR5298123" "SRR5298124" "SRR5298125" "SRR5298126" "SRR5298127" "SRR5298128" "SRR5298129" "SRR5298130" "SRR5298131" "SRR5298132" "SRR5298133" "SRR5298134" "SRR5298135") 

#SE Pf 3D7  Truseq
SRP142460=("SRR7059090" "SRR7059091" "SRR7059092" "SRR7059093" "SRR7059094" "SRR7059095" "SRR7059096" "SRR7059097" "SRR7059098" "SRR7059099" "SRR7059100")

#SE/PE Kapa PbANKA
SRP197607=("SRR10438730" "SRR10438731" "SRR10438732" "SRR10438733" "SRR9041555" "SRR9041557" "SRR9041558" "SRR9041559" "SRR9041560" "SRR9041561" "SRR9041562" "SRR9041563" "SRR9041564" "SRR9041565" "SRR9041566" "SRR9041567" "SRR9041568" "SRR9041569" "SRR9041570")


#PE  KAPA AGATCGGAAGAGCAAGATCGGAAGAGC Pf NF54  
SRP211863=("SRR11247594" "SRR11247595" "SRR11247596" "SRR11247597" "SRR11247598" "SRR9592126" "SRR9592127" "SRR9592128" "SRR9592129" "SRR9592130" "SRR9592131" "SRR9592132" "SRR9592133" "SRR9592134" "SRR9592135" "SRR9592136" "SRR9592137" "SRR9592138" "SRR9592139" "SRR9592140" "SRR9592141" "SRR9592142" "SRR9592143" "SRR9592144" "SRR9592145" "SRR9592146" "SRR9592147" "SRR9592148" "SRR9592149" "SRR9592150" "SRR9592151" "SRR9592152" "SRR9592153" "SRR9592154" "SRR9592155" "SRR9592156" "SRR9592157" "SRR9592158" "SRR9592159" "SRR9592160" "SRR9592161" "SRR9592162" "SRR9592163" "SRR9592164" "SRR9592165" "SRR9974581" "SRR9974582" "SRR9974583" "SRR9974584" "SRR9974585" "SRR9974586" "SRR9974587" "SRR9974588" "SRR9974589" "SRR9974590")

#SE SMART-Seq  PbANKA
SRP250329=("SRR11142813" "SRR11142814" "SRR11142815" "SRR11142816" "SRR11142817" "SRR11142818" "SRR11142819" "SRR11142820" "SRR11142821" "SRR11142822" "SRR11142823" "SRR11142824" "SRR11142825" "SRR11142826" "SRR11142827" "SRR11142828" "SRR11142829" "SRR11142830" "SRR11142831" "SRR11142832" "SRR11142833" "SRR11142834" "SRR11142835" "SRR11142836" "SRR11142837" "SRR11142838" "SRR11142839" "SRR11142840" "SRR11142841" "SRR11142842" "SRR11142843" "SRR11142844" "SRR11142845" "SRR11142846" "SRR11142847")

data_dir="./datasets/raw_data_mapped/"
raw_data="./datasets/raw_data"
scratch_dir="./raw_data_quantification"
out_dir="./datasets/quantification"
subread_bin="./subread-2.0.8-Linux-x86_64/bin"
gff_pb="./reference/plasmodium_berghei/Plasmodium_berghei.PBANKA01.60.gff3"
gff_pf="./reference/plasmodium_falciparum/Plasmodium_falciparum.ASM276v2.60.gff3"
gff_pv="./reference/plasmodium_vivax/Plasmodium_vivax.ASM241v2.60.gff3"

suffix="Aligned.sortedByCoord.out.bam"

mkdir -p ${out_dir}
mkdir -p ${scratch_dir}


quantify.slurm(){
    all_arguments=( "$@" )
    type=${all_arguments[0]} 
    gff=${all_arguments[1]}
    out_prefix=${all_arguments[2]}
  echo " ${scratch_dir}/${out_prefix}"
  mkdir -p ${scratch_dir}/${out_prefix}
  if  [[ "${type}" == "SE" ]]; then
    #-F 'GTF'  -t exon 
        ${subread_bin}/featureCounts -s 1 -t gene -g ID -F 'GTF' -a ${gff}  --tmpDir ${scratch_dir}/${out_prefix} \
        -o ${out_dir}/${out_prefix}_SE.txt ${all_arguments[@]:3}

        ${subread_bin}/featureCounts -s 2 -t gene -g ID -F 'GTF' -a ${gff}  --tmpDir ${scratch_dir}/${out_prefix} \
        -o ${out_dir}/${out_prefix}_SE_reverse.txt ${all_arguments[@]:3}
        
    else 
       ${subread_bin}/featureCounts -p --countReadPairs -t gene -g ID  -F 'GTF' -a ${gff}  --tmpDir ${scratch_dir}/${out_prefix} \
       -o ${out_dir}/${out_prefix}_PE.txt ${all_arguments[@]:3} 
    fi

}

 # ${!ERP004740@} "PE" "Truseq"
ERP004740=($(echo ${ERP004740[@]/#/${data_dir}}))
ERP004740_files=$(echo ${ERP004740[@]/%/${suffix}})
quantify.slurm   "PE"  ${gff_pb}  "SRP100893" ${ERP004740_files[@]}



# ${!ERP105548@} "PE" "Nextera"
ERP105548=($(echo ${ERP105548[@]/#/${data_dir}}))
ERP105548_files=$(echo ${ERP105548[@]/%/${suffix}})
quantify.slurm  "PE"   ${gff_pb} "ERP105548"  ${ERP105548_files[@]} 

     
     
 # ${!SRP027529@} "SE" "Truseq"
SRP027529=($(echo ${SRP027529[@]/#/${data_dir}}))
SRP027529_files=$(echo ${SRP027529[@]/%/${suffix}})     
quantify.slurm   "SE"   ${gff_pb} "SRP027529" ${SRP027529_files[@]}



#${!SRP046739@} "PE" "Truseq"
SRP046739=($(echo ${SRP046739[@]/#/${data_dir}}))
SRP046739_files=$(echo ${SRP046739[@]/%/${suffix}})              
quantify.slurm   "PE"   ${gff_pv}  "SRP046739" ${SRP046739_files[@]} 



#${!SRP048710@} "PE" "Truseq"
SRP048710=($(echo ${SRP048710[@]/#/${data_dir}}))
SRP048710_files=$(echo ${SRP048710[@]/%/${suffix}})      
quantify.slurm  "PE"    ${gff_pf} "SRP048710" ${SRP048710_files[@]} 



#${!SRP069075@} "SE" "Truseq"
SRP069075=($(echo ${SRP069075[@]/#/${data_dir}}))
SRP069075_files=$(echo ${SRP069075[@]/%/${suffix}})    
quantify.slurm  "SE"   ${gff_pf} "SRP069075"  ${SRP069075_files[@]} 


#${!SRP073801@} "PE" "Truseq"  ## check this again 
SRP073801=($(echo ${SRP073801[@]/#/${data_dir}}))
SRP073801_files=$(echo ${SRP073801[@]/%/${suffix}})       
quantify.slurm  "PE"   ${gff_pb} "SRP073801"  ${SRP073801_files[@]}


#${!SRP090611@} "SE" "Truseq"
SRP090611=($(echo ${SRP090611[@]/#/${data_dir}}))
SRP090611_files=$(echo ${SRP090611[@]/%/${suffix}})   
quantify.slurm  "SE"   ${gff_pf} "SRP090611" ${SRP090611_files[@]} 


# ${!SRP099925@} "PE" "Truseq"  ## check this again    
SRP099925=($(echo ${SRP099925[@]/#/${data_dir}}))
SRP099925_files=$(echo ${SRP099925[@]/%/${suffix}}) 
quantify.slurm   "PE"   ${gff_pb}  "SRP099925" ${SRP099925_files[@]}

     
#${!SRP100893@} "SE" "Truseq"
SRP100893=($(echo ${SRP100893[@]/#/${data_dir}}))
SRP100893_files=$(echo ${SRP100893[@]/%/${suffix}})      
quantify.slurm   "SE"   ${gff_pv}  "SRP100893"  ${SRP100893_files[@]}

     
#${!SRP142460@} "SE" "Truseq"
SRP142460=($(echo ${SRP142460[@]/#/${data_dir}}))
SRP142460_files=$(echo ${SRP142460[@]/%/${suffix}})      
quantify.slurm   "SE"  ${gff_pf} "SRP142460"  ${SRP142460_files[@]}


# ${!SRP197607@} "mixed" "Kapa"  ---->
SRP197607_PE=()
SRP197607_SE=()
for run in ${SRP197607[*]}; do
      files=($(ls ${raw_data}/SRP197607/${run}*fastq))
      if [[ "${files[0]}" =~  "_1.fastq"  ]]; then
      SRP197607_PE+=("${run}")
         #sbatch --job-name="${run}" quantify.slurm  ${run} "PE"    ${gff_pb}
     else 
      SRP197607_SE+=("${run}")
       #sbatch --job-name="${run}" quantify.slurm  ${run}  "SE"   ${gff_pb}
     fi
     done
SRP197607_PE=($(echo ${SRP197607_PE[@]/#/${data_dir}}))
SRP197607_PE_files=$(echo ${SRP197607_PE[@]/%/${suffix}})
SRP197607_SE=($(echo ${SRP197607_SE[@]/#/${data_dir}}))
SRP197607_SE_files=$(echo ${SRP197607_SE[@]/%/${suffix}})
quantify.slurm   "PE"    ${gff_pb} "SRP197607" ${SRP197607_PE_files[@]}
quantify.slurm    "SE"   ${gff_pb}  "SRP197607" ${SRP197607_SE_files[@]}


             
 # ${!SRP211863@} "PE" "Kapa" 
SRP211863=($(echo ${SRP211863[@]/#/${data_dir}}))
SRP211863_files=$(echo ${SRP211863[@]/%/${suffix}})      
quantify.slurm   "PE"   ${gff_pf}  "SRP211863" ${SRP211863_files[@]}

     
# ${!SRP250329@} "SE" "Kapa"
SRP250329=($(echo ${SRP250329[@]/#/${data_dir}}))
SRP250329_files=$(echo ${SRP250329[@]/%/${suffix}})   
quantify.slurm   "SE"   ${gff_pb}  "SRP250329"  ${SRP250329_files[@]}



#clean out files
#cd quantification
#sed -i 's/\/cbio\/projects\/022\/adam\/project_adaobi\/datasets\/raw_data_mapped\///g' *txt*
#sed -i 's/Aligned.sortedByCoord.out.bam//g' *txt*
#sed -i '1d'  *.txt
```