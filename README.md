Data preparation and GLIPH output analysis are written in R. 
GLIPH is run using terminal (the perl scripts can be obtained from: https://github.com/immunoengineer/gliph/blob/master/gliph-1.0.tgz)

# Instruction: 
1. Data Preparation - cleaned the raw data which will be used as inputs in GLIPH and GLIPH outputs analysis. This process has 2 outputs: 'd7_active_all.txt' and 'all.tsv'.

2. GLIPH - run GLIPH using this command 'perl gliph-group-discovery.pl --tcr d7_active_all.txt'. This command will produced 4 output files. 

3. GLIPH output analysis - analyse the outputs produced by GLIPH. In this step, we will use of the following files:
- 'all.tsv' file to get the detailed information of the amino acids, 
- 'd7_active_all-convergence-groups.txt' to analyse the formed clusters, and 
- 'd7_active_all-kmer_resample_1000_minp0.001_ove10.txt' to analyse the found motifs. 
