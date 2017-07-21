# Welcome to use d2SBin
  d2SBin is easy-to-use contig-binning improving tool, which adjusted the contigs among bins based on the output of any existing binning tools. The tool is taxonomy-free only on the k-tuples for single metagenomic sample.

d2SBin is based on the mechanism that relative sequence compositions are similar across different regions of the same genome, but differ between genomes. Current tools generally used the normalized frequency of k-tuple directly, which actually is the absolute instead of relative sequence composition. Therefore, we attempted to model the relative sequence composition and to measure the dissimilarity between contigs with d2S. We applied d2SBin to adjust the outputs of five widely-used contig-binning tools on six datasets. The experiments showed that d2SBin can improve the contig binning performance significantly. 

The d2SBin pipeline was developed with Python and run on the Unix and Linux platform, and the detail description of running is provided [here](https://github.com/kunWangkun/d2SBin#package-installation-and-configuration). 

## Version Release Notes

- Version 1.0  
	1.This is the first version of d2SBin pipeline. [(Source code Download )](https://raw.githubusercontent.com/kunWangkun/d2SBin/master/d2SBin_SourceCode.rar)  
	2.An demo of d2SBin running is given [here](https://github.com/kunWangkun/d2SBin/blob/master/README.md#the-demo-of-d2sbin-on-testing-dataset).  
## Development Team
The whole source code was developed by Ying Wang's group, Automation Department, Xiamen University, P.R.China. All the suggestions and questions are welcome to *wangying AT xmu.edu.cn*.
 
## Package installation and configuration
- Pre-install running environment   
	1.Unix or Linux operating system.  
 	2.Python 2.7 or above.  
- Detailed steps  
        1.Download the source code to your directory, e.g: *’/home/user/d2SBin’*.  
 	2.Enter your specified directory: *$ cd /home/user/d2SBin*  
 	3.Extract the tar file:  *$ unrar x d2SBin_SourceCode.rar*  
 	4.Enter the directory:  *$ cd /home/user/d2SBin/d2SBin_SourceCode*  
 	5.If your operating system has multiple Python version, please be sure your Python version at least 2.7 or above.  

## Running of d2SBin Pipeline  

### 1. Usage of d2SBin  
  
- The main running command is *d2SBin.py* with following options:    

	-h, --help: show the help message.  
	-s, --inputList_withSeq: List of path and file name of contig files with sequence(for Format1).  
	-c, --contig_Seq: The original fasta file including total contig sequences(for Format2).  
	-n, --inputList_noSeq: List of path and name of files with contig ID (for Format2).  
	-k, --kofKTuple: The value k of KTuple (default is k=6)  
	-r, --order: The order of markov model(0,1,2,3,k-2) (default is r=0)  
	-i, --iteraNum: The maximum number of kmeans iteration (default is i=5)  
	-o, --output: The output path of d2SBin results  
 

### 2. Input: The output of existing contig-binning tools  

The input of d2SBin is the output of existing contig-binning tools. The output of current contig-binning tools has the following two formats:
- d2SBin_input_format1: .fasta files with contigs sequence from the same bins, such as the outputs from tools MaxBin, MetaWatt and SCIMM. Their outputs include bins-number of fasta files. Each fasta file includes the contigs ID and sequence clustered in the same bin. For example, the outputs from MaxBin are *MaxBin.out.001.fasta...MaxBin.out.00X.fasta*, where *X* is the bins number by MaxBin. The *MaxBin.out.001.fasta* is as follows.

	`>contig-1.0`  
	`GACACTTTTAGTGGGCGTAAACTTCATCTAGTGGATCT`  
	`>contig-1.2`  
	`CCATGTCAGAAGAAGTTGGTAATCGCCACATTAATTGTTTGTCGTTTGATCGA`  
	`…`  
- d2SBin_input_format2: .fa files only with contigs name from the same bins, such as the outputs from tool MetaCluster. Its outputs include bins-number of fasta files. Each fasta file only includes the contigs ID in the same bin, so the orginal fasta file including all the sequences of total contigs is also required. For example, the outputs from MetaCluster are *MetaCluster.out.001.fa … MetaCluster.out.00Y.fa*, where *Y* is the bins number by MetaCluster. The *MetaCluster.out.001.fa* is as follows  

	`>contig-1.0`  
	`>contig-1.1`  
	`…`  
	
   The original file include total contigs and theire sequences *contigs.fasta* is as follows:   
	
	`>contig-1.0`   
	`GACACTTTTAGTGGGCGTAAACTTCATCTAGTGGATCT`   
	`>contig-1.1`    
	`TGGTAATCGCCACATTAAAGAAGTTGGTAA`  
	`>contig-1.2`    
	`CCATGTCAGAAGAAGTTGGTAATCGCCACATTAATTGTTTGTCGTTTGATCGA`    
	`…`  

d2SBin is compatible to the two formats as the following commands:

- **d2SBin_input_format1**

  In this case, d2SBin needs one list file as input.  
  
  First, get list file:   
  
	 *`$ ls /home/.../MaxBin.out*.fasta > input_file_list_fomat1.txt`*   
	
  where */home/.../* means the absolute path of your fasta files.
	
  Next, run d2SBin.py:   
  
	 *`$ python d2SBin.py -s input_file_list_fomat1.txt -k 6 -r 0 -i 5 -o ../data/output/`*  
	
- **d2SBin_input_format2**

	As for format2, d2SBin needs two input files: the original contig sequences file, e.g: *contigs.fasta*, and list file of other tools output.   
  
	First, get the list file:  	
  
	 *`$ ls /home/.../MetaCluster.out*.fa > input_file_list_fomat2.txt`*     
	
 	Next, run d2SBin.py:  
  
	 *`$ python d2SBin.py -n input_file_list_format2.txt -c contigs.fasta -k 6 -r 0 -i 5 -o ../data/output`*     

### 3. Output  

d2SBin also has two formats of output:  
- d2SBin_output_format1: txt file with contigs ID and binning result label. eg. *d2SBin.out.k6.r0.txt*  

	`>contig-1.1, 0`  
	`>contig-1.2, 3`  
	`…`   	
	
- d2SBin_output_format2: fasta files with sequence from same bins. eg. *d2SBin.k6.r0.out0.fasta,...,d2SBin.k6.r0.out9.fasta*  

	`>contig-1.0`  
	`GACACTTTTAGTGGGCGTAAACTTCATCTAGTGGATCT`  
	`>contig-1.2`  
	`CCATGTCAGAAGAAGTTGGTAATCGCCACATTAATTGTTTGTCGTTTGATCGA`   
	`…`   

### 4. Evaluate output   
We provide a script *evaluation.py* for computing performance of binning with the two formats of output mentioned above. The usage can be viewed by typing *python evaluation.py -h* on the command line:  

- **Options**  

 	-h, --help: show this help message and exit.   
  	-c, --binning_result: binning result file.    
	-l, --list of binning result files.    
 	-t, --true_label: true label of contigs.   
  	-e, --eva_output_dir: the path of evaluation file.    
- **Usage**  

	**(1)  Evaluate the output of d2SBin**   	

	For the .txt output file of d2SBin, you can use the option *-c* to evaluate the result. e.g:
 
     *`$ python evaluation.py -c d2SBin.k6.r0.txt -t real_label.txt -e ./eva`*       
	
	**(2)  Evaluate the original output of binning tools**  

	For d2SBin_input_format1(.fasta files with contigs ID and sequences from same bins), you can create a list file and use the option *-l*. e.g:  
	
	*`$ ls /home/.../Binning.output*.fasta > output_fasta_file_list.txt`*    
		
	*`$ python evaluation.py -l output_fasta_file_list.txt -t real_label.txt -e ./eva`*   
	
	As for d2SBin_input_format2(.fa files only with contigs ID from the same bins):

	*`$ ls /home/.../Binning.output*.fa > output_fa_file_list.txt`*     
		
	*`$ python evaluation.py -l output_fa_file_list.txt -t real_label.txt -e ./eva`*   
	
- **Format of true label file**

	The format of true label file is not fixed. It can works as long as each line of the true label file has the contig ID on the first column and the genome information(such as genome name, Gene ID) on the second field. e.g:   
   
  	`NODE_1_length_83949_cov_75.226173       256653503       Acetobacter pasteurianus`  
	`NODE_2_length_45147_cov_75.717964       256653503       Acetobacter pasteurianus`
	

## The demo of d2SBin on testing dataset. 

Dataset1: 10 genomes-80×[(Testing dataset Download)](https://raw.githubusercontent.com/kunWangkun/d2SBin/master/testing_data.rar)  

The dataset was generated by MaxBin1.0[1](You can download MaxBin [here](http://downloads.jbei.org/data/microbial_communities/MaxBin/MaxBin.html)). For the 10 genomes metagenomes, the 20 millian paired-end reads were samples as 80× average coverage. The short reads were simulated by MetaSim and assembled to contigs by Velvet. 

#### Run d2SBin after MaxBin 

1. The assembled contigs *80x.fasta* were binned by MaxBin[1].  

	*`$ perl run_MaxBin.pl -contig 80x.fasta -abund 80x.abund -out ./output_of_MaxBin_80x/myout`*   
	
    The contig-binning results from Maxbin are *myout.001.fasta, ..., myout.010.fasta*.  
	
2. Get the list file of MaxBin output   

	*`$ ls ./output_of_MaxBin_80x/myout*.fasta > MaxBin_80x_input_list.txt`*   

3. Evaluate the original binning result of MaxBin

	*`$ python evaluation.py -l MaxBin_80x_input_list.txt –t 80x_real_label.loc`* 
	
4. Run d2SBin to adjust the contigs binning  

	Create a new folder to put output files.  
	
	*`$ mkdir ../d2SBin_MaxBin_80x_output`* 
	
	Run d2SBin.py
	
	*`$ python d2SBin.py -s MaxBin_80x_input_list.txt -k 6 -r 0 -i 5 -o ../d2SBin_MaxBin_80x_output`*   
	
	The output are *./d2SBin_MaxBin_80x_output/d2SBin.k6.r0.out0.fasta, ...,d2SBin.k6.r0.out9.fasta* and *d2SBin.out.k6.r0.txt*.    	
5. Calculate the performance of d2SBin binning results  

	*`$ python evaluation.py -c ../d2SBin_MaxBin_80x_output/d2SBin.out.k6.r0.txt -t 80x_real_label.loc`* 
	
    where the *80x_real_label.loc* is the file of real contig labels.   
 

#### Run d2SBin after MetaCluster

1. The assembled contigs *80x.fasta* were binned by MetaCluster[2] (You can download MetaCluster [here](http://i.cs.hku.hk/~alse/MetaCluster/download.html)) as followed commands. 
	   
	*`$ ./bin/metaCluster 80x.fasta`*   

    The contig-binning results from MetaCluster are *80x.fasta-out000.fa, ..., 80x.fasta-out009.fa*.  

2. Get list file  
	
	*`$ ls ./80x.fasta-out*.fa > MetaCluster_80x_input_list.txt`*

3. Evaluate the original binning result of MetaCluster

	*`$ python evaluation.py -l ./MetaCluster_80x_input_list.txt -t 80x_real_label.loc`* 

4. Run d2SBin  

	Create a new folder to put output files.  
	
	*`$ mkdir ../d2SBin_MetaCluster_80x_output`* 
	
	For MetaCluster, d2SBin needs two input files: *MetaCluster_80x_input_list.txt* and *80x.fasta*

	*`$ python d2SBin.py -n MetaCluster_80x_input_list.txt -c 80x.fasta -k 6 -r 0 -i 5 -o ../d2SBin_MetaCluster_80x_output`*  
	
 	The output are *./d2SBin_MetaCluster_80x_output/d2SBin.k6.r0.out0.fasta, ...,d2SBin.k6.r0.out9.fasta* and *d2SBin.out.k6.r0.txt*.

5. Calculate the performance of d2SBin  

	*`$ python evaluation.py -c ../d2SBin_MetaCluster_80x_output/d2SBin.out.k6.r0.txt -t 80x_real_label.loc`* 
	
   where the *80x_real_label.loc* is the file of real contig labels.   
  
## Time and Memory  
Tested on a server with 128G memory and Intel(R) Xeon(R) CPU E5-2620 v2 @ 2.10GHz with 6 CPU cores at 2.10 GHz, it takes 16min to finish the adjustment of contig binning for *d2SBin* on 6-tuples for 8022 contigs of 10 bins with 4000bp length on average and the peak memory is 6.7GB.

## References

*[1]Wu, Y.-W., et al., MaxBin: an automated binning method to recover individual genomes from metagenomes using an expectation-maximization algorithm. Microbiome, 2014. 2(1): p. 26.*

*[2]Leung, H.C., et al., A robust and accurate binning algorithm for metagenomic sequences with arbitrary species abundance ratio. Bioinformatics, 2011. 27(11): p. 1489-1495.*
