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

# cluster_dir="/mnt/sdb3/adaobi/malaria_rna-seq_data-analysis/7_clustering/"
# setwd(cluster_dir)
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

## Cluster 

vst <- vst(dds)
Count_vst <- assay(vst)

stages <- Metadata$stage
stages_unq= unique(stages)

##  average expression per stage 
Countavrep=data.frame(ncol = 0, nrow = dim(Count_vst)[1])
for ( i in 1:length(stages_unq)){
   indexes=which(as.character(stage2) == as.character(stages_unq[i]))
   len= length(which(as.character(stage2) == as.character(stages_unq[i])))
   Countavrep <- cbind(Countavrep, rowSums(Count_vst[,indexes])/len)
   }



dim(Countavrep)
Countavrep=Countavrep[,-c(1,2)]
colnames(Countavrep) = stages_unq


all_countsavrep_scaled <- t(scale(t(Countavrep)))   
write.table(all_countsavrep_scaled, "sclustcountsavrep.csv", sep = ",", col.names = NA) 


#nbclustallupreg8 <- NbClust(data= allupreg8stages_countsavrep_scaled, 
 #                           distance = "euclidean", min.nc = 2,
 #                           max.nc = 7, method = "kmeans")
# Select Upregulated accross atages 

up_reg= Countavrep  %>% filter( Ookinete>=2, Sporozoite>=2, Liver_stages >=2,  Ring>=2,  Trophozoite>=2,  Schizont>=2,  Gametocyte>=2)
down_reg= Countavrep  %>% filter( Ookinete<=-2, Sporozoite<=-2, Liver_stages <=-2,  Ring<=-2,  Trophozoite<=-2,  Schizont<=-2,  Gametocyte<=-2)
dim(up_reg)
dim(down_reg)
allupregstages_countsavrep_scaled=t(scale(t(up_reg)))  
write.table(allupregstages_countsavrep_scaled, "up_sclustcountsavrep.csv", sep = ",", col.names = NA) 
# Perform initial clustering
myCol <- colorRampPalette(c('dodgerblue', 'black', 'yellow'))(100)
myBreaks <- seq(-3, 3, length.out = 100)

pamClustersallupreg <- cluster::pam( allupregstages_countsavrep_scaled, k = 3) # pre-select k = 3 centers
pamClustersallupreg$clustering <- paste0('Cluster ', pamClustersallupreg$clustering)

# fix order of the clusters to have 1 to 3, top to bottom
pamClustersallupreg$clustering <- factor(pamClustersallupreg$clustering,
                                          levels = c('Cluster 1', 'Cluster 2', 'Cluster 3'))

# Specify the number of sub-clusters for each initial cluster
num_sub_clusters <- c(4, 5, 6)

# Initialize a list to store sub-cluster results
sub_cluster_labels <- list()




# Iterate through the initial clusters
for (i in 1:3) {
  # Extract genes in the current initial cluster
  cluster_genes <- allupregstages_countsavrep_scaled[pamClustersallupreg$clustering == paste0('Cluster ', i), ]
  
  # Perform sub-clustering using k-means with the specified number of sub-clusters
  sub_cluster_labels[[i]] <- kmeans(cluster_genes, centers = num_sub_clusters[i])$cluster
  
  # Modify sub-cluster labels to distinguish from the initial clusters
  sub_cluster_labels[[i]] <- paste0('Cluster ', i, '.Sub', sub_cluster_labels[[i]])
}

# Combine the sub-cluster labels into a single vector
sub_cluster_labels <- unlist(sub_cluster_labels)

# Modify the labels of the original clustering to include sub-cluster labels
pamClustersallupreg$clustering <- sub_cluster_labels

# Create the heatmap with modified clustering labels
hmapallupreg <- Heatmap(allupregstages_countsavrep_scaled,
                         
                         # split the genes / rows according to the PAM clusters
                         split = pamClustersallupreg$clustering,
                         cluster_row_slices = FALSE,
                         
                         name = 'Gene\nZ-\nscore',
                         
                         col = colorRamp2(myBreaks, myCol),
                         
                         # parameters for the colour-bar that represents gradient of expression
                         heatmap_legend_param = list(
                           color_bar = 'continuous',
                           legend_direction = 'vertical',
                           legend_width = unit(8, 'cm'),
                           legend_height = unit(5.0, 'cm'),
                           title_position = 'topcenter',
                           title_gp=gpar(fontsize = 6, fontface = 'bold'),
                           labels_gp=gpar(fontsize = 6, fontface = 'bold')),
                         
                         # row (gene) parameters
                         #cluster_rows = TRUE,
                         show_row_dend = TRUE,
                         #row_title = 'Statistically significant genes',
                         #row_title_side = 'left',
                         row_title_gp = gpar(fontsize = 4,  fontface = 'bold'),
                         row_title_rot = 0,
                         show_row_names = FALSE,
                         row_names_gp = gpar(fontsize = 4, fontface = 'bold'),
                         #row_names_side = 'left',
                         #row_dend_width = unit(25,'mm'),
                         row_labels=NULL,
                       
                         
                         # column (sample) parameters
                         cluster_columns = TRUE,
                         show_column_dend = TRUE,
                         column_title = 'Stages',
                         column_title_side = 'bottom',
                         column_title_gp = gpar(fontsize = 12, fontface = 'bold'),
                         column_title_rot = 0,
                         show_column_names = TRUE,
                         column_names_gp = gpar(fontsize = 10, fontface = 'bold'),
                         column_names_max_height = unit(10, 'cm'),
                         column_dend_height = unit(25,'mm'),
                         
                         # cluster methods for rows and columns
                         clustering_distance_columns = function(x) as.dist(1 - cor(t(x))),
                         clustering_method_columns = 'ward.D2',
                         clustering_distance_rows = function(x) as.dist(1 - cor(t(x))),
                         clustering_method_rows = 'ward.D2')

# Draw the heatmap
pdf("heatmap_pam.pdf")
draw(hmapallupreg #,
     #heatmap_legend_side = 'left' #,
     #row_sub_title_side = 'left'
     )
dev.off()
```