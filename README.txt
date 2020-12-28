Title: STRIDE (STevor RIfin iDEntifier)
Primary Author: Albert Zhou, albert.zhou@som.umaryland.edu
Contact: Mark Travassos, mtravass@som.umaryland.edu
Date Last Modified: December 28, 2020
Created Using: HMMERv3.3 & Perlv5.24.0
Overview: STRIDE is an HMM-based, command-line program that automates the identification and classification of RIFIN and STEVOR protein sequences.

----- After unzipping the tarball file for STRIDE, the parent directory should contain 6 items -----
1. This README file
2. A primary script to run the program (stride.sh) 
3. A secondary script for annotation (stride_ann.pl)
4. A directory where the merged and individual HMM profiles are located (Final_Profiles)
5. A directory with sets of FASTA files with which to test STRIDE (Test_Sequences)
6. When "stride.sh" is executed, two files are stored in the "results" directory (results)
	-One file will be a sorted, reorganized version of the raw file containing scores from the merged HMM profile (stride_raw.<fasta_file>.txt) 
	-The second file will contain the annotations for each sequence (annotated.<fasta_file>.txt)


----- TO INSTALL HMMERv3.3 DEPENDENCIES -----
	wget http://eddylab.org/software/hmmer/hmmer.tar.gz
	tar zxf hmmer.tar.gz
	cd hmmer-3.3.1
	./configure
	make
	make check
	make install


----- TO EXECUTE -----
While in the main directory (`pwd` */STRIDE), type the following command and the path to the file containing your protein (AMINO ACID) sequences in FASTA format. FASTA files begin with a '>' that gives a name/identifier for the sequence, followed by standard IUB/IUPAC amino acid codes. Hyphens/dashes "-" can represent gaps and asterisks "*" may represent stop codons or wildcards. Avoid other unsupported characters as it may return errors.
	
	./stride.sh <path/fasta_file>

The main script "stride.sh" conducts the search of the FASTA file against the HMM profile and sorts (by sequence ID) & reorganizes the raw output. These results will be placed in the "results" directory:
	
	1) stride_raw.<fasta_file>.txt
	2) annotated.<fasta_file>.txt

The secondary script "stride_ann.pl" automates annotations and is executed automatically by "stride.sh" using default thresholds. 
However, this script can also be run independently on the "stride_raw.<fasta_file>.txt" file with different thresholds. 
Default values are: RIFIN threshold = 200; RIFIN-A threshold = 250; RIFIN-B threshold = 250; STEVOR threshold = 145. 
Feel free to play around with these options to increase/decrease sensitivity & specificity.

	perl stride_ann.pl -input stride_raw.<fasta_file>.txt -output outputfile -Rifin threshold_score (optional) -RifinA threshold_score (optional) -RifinB threshold_score (optional) -Stevor threshold_score (optional)


"./stride -h" or "./stride -help" will bring up this README.

----- TO ANALYZE -----
stride_raw.<fasta_file>.txt (sorted, reorganized raw file):
	- Column 1 = HMM Profile
	- Column 2 = Sequence ID
	- Column 3 = E-value for whole sequence score
	- Column 4 = HMM Score for whole sequence
	- Column 5 = E-value for best domain score
	- Column 6 = HMM Score for best domain

annotated.<fasta_file>.txt (contains annotations for each sequence):
	- Column 1 = Sequence ID
	- Column 2 = STRIDE's Annotation of Sequence
	- Column 3 = HMM Score for the Whole Sequence
	* Any Sequences with HMM Whole Sequence Scores below 75 are often poor matches either due to divergence or pseudogene * 
	* Sequences that do not meet basic criteria are not annotated *


----- CONCLUDING REMARKS -----
If you use STRIDE in your work, please cite us. 
Email us if you encounter any problems, have questions, or want to leave a comment!
