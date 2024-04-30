Methylation Prediction via GRNBoost2: \ 
download methylation_prediction_via_grnboost2, \
unzip hippie, \
install gseapy and mygene, \
run the following files \

preparing_data.ipynb: \
• in: matrix_methyla4on.tsv (cpgs), matrix_rna.tsv (genes) \
(from GDC TCGA Prostate Cancer (PRAD): \
hDps://xenabrowser.net/datapages/?host=hDps%3A%2F%2Fgdc.xenahubs.net&datas et=TCGA-PRAD.htseq_fpkm- uq.tsv&allSamples=true&removeHub=hDps%3A%2F%2Fxena.treehouse.gi.ucsc.edu% 3A443, \
hDps://xenabrowser.net/datapages/?dataset=TCGA- PRAD.methyla4on450.tsv&host=hDps%3A%2F%2Fgdc.xenahubs.net&removeHub=hD ps%3A%2F%2Fxena.treehouse.gi.ucsc.edu%3A443 ) \
• out: ex_matrix.tsv (expression matrix), W_names.tsv (gene names), target_names.tsv (cpg names) \
• (filters out protein coding genes) \
• (uses 5000 genes with the highest variance in the gene expression matrix) \
• converts gene names to names without version info \
• concatenates genes and cpgs together to the expression matrix (>>ex_matrix.tsv) \

GRNBoost2.ipynb: \
• in: ex_matrix.tsv, W_names.tsv, target_names.tsv \
• out: grn_output_i.tsv for i in [1,...,10] in the folder ouput \
• run GRNBoost2 10 times \

preparing_ranking.ipynb: \
• in: grn_output_i.tsv for i in [1,...,20] \
• out: combined_1.tsv, combined_2.tsv \
• concatenate the first 10 outputs from GRNBoost2 and the second 10 outputs from
GRNBoost2 to two dataframes (genename_cpgname as index and one column with the GRNBoost2 scores for each run) \

ranking.ipynb: \
• in: combined_1.tsv, combined_2.tsv \
• out: ranking_1.tsv, ranking_2.tsv \
• converts the dataframes with scores to dataframes with a ranking (by scores) using
scipy.stats.rankdata \

evaluate_ranking.ipynb: \
• in: ranking_1.tsv, ranking_2.tsv \
• out: aggr_rnk.tsv \
• uses kendall tau distance to evaluate the rankings \
• uses borda count to aggregate the different rankings \

filter_coding_TFs.ipynb: \
• in: aggr_rnk.tsv, gene_types \
• out: ens_ID_top30TFs.csv \
• filters out protein coding genes and saves the top30 \

analysis.ipynb: \
• in: hippie.txt (PPI network), ens_ID_top30TFs.csv \ 
• generate n random sets of k nodes from hippie PPI network with the same node degree as the TF genes \
• calculate pagerank for TF genes und all n sets of random k genes (start with equal probability in one of the DMTs (methyla4on genes)) \
• get the mean pagerank for the TF genes, and for all n sets of random k genes \
• calculate a p-value with: (sum_i [mean_random[i] >= mean_W] + 1) /(n+1) \
(>> p-value is around 0.07) \
