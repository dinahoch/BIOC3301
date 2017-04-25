# excludes sample 16.17.3 to create filtered biom table
filter_samples_from_otu_table.py -i merged_otu_table.biom -m map.tsv_corrected.txt --sample_id_fp ids.txt --negate_sample_id_fp -o filtered_otu_table.biom
# splits biom table by year
split_otu_table.py -i merged_otu_table.biom -m map.tsv_corrected.txt -f Year -o phyla_otu.txt
# filters taxa from the biom table to include only the phyla Acidobacteria, Actinobacteria, Bacteroidetes, Proteobacteria, Planctomycetes and Verrucomicrobia
filter_taxa_from_otu_table.py -i filtered_otu_table.biom -o filtered_taxa.biom -p p__Acidobacteria,p__Actinobacteria,p__Bacteroidetes,p__Proteobacteria,p__Planctomycetes,p__Verrucomicrobia
# a workflow script for performing taxonomy summaries and plots
summarize_taxa_through_plots.py -i filtered_otu_table.biom -o taxa_summary -m map.tsv_corrected.txt
# compare phyla frequencies in both years to determine whether differences are statistically significant
group_significance.py -i filtered_otu_table_L2.biom -m map.tsv_corrected.txt -c Year -o para_tt_phyla.txt -s parametric_t_test 
group_significance.py -i filtered_otu_table_L2.biom -m map.tsv_corrected.txt -c Year -o nonpara_tt_phyla.txt -s nonparametric_t_test
