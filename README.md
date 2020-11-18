Data preparation and GLIPH output analysis are written in R. 
GLIPH is run using terminal (the perl scripts can be obtained from: https://github.com/immunoengineer/gliph/blob/master/gliph-1.0.tgz)

# Instruction: 
1. Data Preparation - cleaned the raw data which will be used as inputs in GLIPH and GLIPH outputs analysis. This process has 2 outputs: 'd7_active_all.txt' and 'all.tsv'.

2. GLIPH - run GLIPH using this command 'perl gliph-group-discovery.pl --tcr d7_active_all.txt'. The 'd7_active_all.txt' file is provided in GLIPH/input file. This command will produced 4 output files. 

3. GLIPH output analysis - analyse the outputs produced by GLIPH. In this step, we will use of the following files (all files are provided in GLIPH output analysis/Input data):
- 'all.tsv' file to get the detailed information of the amino acids, 
- 'd7_act_convergence_groups.txt' to analyse the formed clusters, and 
- 'd7_act.txt' to analyse the found motifs. 




# More Detailed Preparation
## 1. Data Preparation
As per Jennifer’s Instructions the following steps were completed. These steps include
1)	Remove any row without any data in the aminoAcid column (these are your CDR3s).
2)	Remove any rows with an * in any part of the text in the aminoAcid column (points 1 and 2 remove what we call non-productive TCRs).
3)	Remove any rows with these 10 CDR3s (aminoAcid column; CASSFWGQGTDTQYF,CASSESSGRAILTDTQYF, CASSQAPPGQGVDIQYF,CASSTYRAALENEQFF,CASSQDRIHTEAFF,CASSHGSDEQYF,CASIHQGSTEAFF, CASSPGQGNYGYTF,CASSLDRNTEAFF,CASSLGTDTQYF).
4)	The column titled vGeneName has our family names for the variable gene. This is the resolution we want, however you will notice not every row has data. For those with NA, you will need to input the data from the vFamilyName column for that row.
5)	Now you can select the columns you need. These are aminoAcid (CDR3), count (templates/reads; this is the number of times each CDR3 was detected), and vGeneName (the modified one that should have a value in every cell).
6)	Check for any duplicates in CDR3 (some nucleotide sequences will produce the same amino acid sequence). If you find duplicates, add the count together and remove duplicate rows.
7)	Add in a new percentage column for each CDR3 using the new count column. You cannot use the original as we have removed rows and adjusted the overall count.

In addition to Jennifer’s Instructions, the following step was also included to fit into the format of GLIPH
-	Adding additional column: Patient 
-	Filling any rows with NA in jGeneName with jFamilyName
-	Checks duplicates based on vGeneName and jGeneName instead of just vGeneName

An example of the input data is as follows
aminoAcid		    vGeneName	  jGeneName	  patient		count
CASDPGPLHTEAFF	TCRBV02-01	TCRBJ01-01	Sbj_14		1
CASRVYGFSPEAFF	TCRBV02-01	TCRBJ01-01	Sbj_14		1
CASSEGDMITEAFF	TCRBV02-01	TCRBJ01-01	Sbj_14		1
CASTYRQAVNTEAFF	TCRBV02-01	TCRBJ01-01	Sbj_14		1

This needs to be in a tab-separated value (TSV) file or a txt with the data being tab separated. This data cleaning was performed in R and this file will be attached as well.


### OPTIONAL
Addition of HLA data
The format of the data needs to be in the following:

SUBJECT	CLASS	ALLELE		CLASS	ALLELE		CLASS	…	  CLASS	ALLELE
Sbj_1		A	    23:01:01G	A	    24:02:01G	B   	…	  DRB5	NP
Sbj_2		A	    02:01:01G	A	    11:01:01G	B	    …	  DRB5	NP
Sbj_3		A	    74:01:01G	A	    74:01:01G	B	    …	  DRB5	NP
*not including the headings

Like above, this file needs to be in a tab-separated value (TSV) file or a txt with the data being tab separated. The file can be manipulated via Microsoft Excel with each cell corresponding to each datapoint and then saved as a Tab-delimited Text (.txt) file.

## 2. Operating GLIPH
GLIPH is a program written in Perl and can be run on different operating systems.

MAC OS/Linux 
On Mac OS and Linux open Terminal.

Move to directory of where GLIPH is downloaded.
	Eg. If the file was in your Downloads folder, type in Terminal
  cd Downloads/

Unpack GLIPH
	tar -xzvf gliph-1.0.tgz

Changing directory to gliph folder
  cd gliph/

Running GLIPH
  bin/gliph-group-discovery.pl --tcr <FILENAME>

With HLA data 
  gliph-group-scoring.pl --convergence_file <FILENAME>-convergence-groups.txt --hla_file <HLAFILE>

NOTE: <FILENAME>-convergence-groups.txt is a file that is produced from running GLIPH

Running GLIPH can take a long time depending on your computer’s CPU/RAM as well as the length of the file. As a rough guide it took 15 minutes to 2 hours for some files.



## 3. Output Analysis via R
The R file assumes that the R file itself and the results are in the same directory(folder), if not please move the files into the same directory for the R file to run.

The 'all.tsv' file that is referred to in the file is a tab-separated value file that combines all 48 files that was initially given. Additional columns were added including Patient, Day, Year and Status.
