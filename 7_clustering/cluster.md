# R packages
1. ggplot2
2. reshape2
3. readxl
4. factoextra
5. NbClust
6. pheatmap
7. viridis
8. RColorBrewer
9. ComplexHeatmap
10. circlize
11. digest
12. cluster

# Required files



# script
```R
library(ggplot2)
library(reshape2)
library(readxl)
library(factoextra)
library(NbClust)
library(pheatmap)
library(viridis)
library(RColorBrewer)
library(ComplexHeatmap)
library(circlize)
library(digest)
library(cluster)

# setwd("../7_clustering")

### Adam
#make volcano plots

##########################################
##        From Script DGE analysis      ##
##        DEGlist                       ##
##        resultlist                    ##
##########################################

for (i in 1:length(DEGlist)) {
volcano <- DEGlist[[i]] 

if(dim(volcano)[1] > 2){
png(paste0(as.character(names(DEGlist))[i] , "_volcano.png"), width = 6, height = 6, units = 'in', res = 500)
cat(i, "... ",dim(volcano), "\n")
s_title <- as.character(names(DEGlist))[i]
s_title <- gsub("res_", "", s_title)
s_title <- gsub("_", " ", s_title)
plot(
EnhancedVolcano(volcano,
                lab = rownames(volcano),
                x = 'log2FoldChange',
                y = 'padj', title = s_title,
                subtitle = "", pCutoff = 0.01,
                FCcutoff = 2,
                pointSize = 1.0, col=c('grey', 'grey', 'grey', 'red3'),
                labSize = 2.0, caption = "", legendPosition = 'none')

)
dev.off()
}

}

## plot all genes
#resultlist
system("mkdir allgenes")
for (i in 1:length(resultlist)) {
volcano <- resultlist[[i]] 

if(dim(volcano)[1] > 2){
png(paste0("allgenes/", as.character(names(resultlist))[i] , "_allgenes_volcano.png"), width = 6, height = 6, units = 'in', res = 500)
cat(i, "... ",dim(volcano), "\n")
s_title <- as.character(names(resultlist))[i]
s_title <- gsub("res_", "", s_title)
s_title <- gsub("_", " ", s_title)
plot(
EnhancedVolcano(volcano,
                lab = rownames(volcano),
                x = 'log2FoldChange',
                y = 'padj', title = s_title,
                subtitle = "", pCutoff = 0.01,
                FCcutoff = 2,
                pointSize = 1.0, col=c('grey', 'grey', 'grey', 'red3'),
                labSize = 2.0, caption = "", legendPosition = 'none')

)
dev.off()
}

}
```