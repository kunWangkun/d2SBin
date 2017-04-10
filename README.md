# Welcome to use d2SBin
  d2SBin is easy-to-use contig-binning improving tool, which adjusted the contigs among bins based on the output of any existing binning tools. The tool is taxonomy-free only on the k-tuples for single metagenomic sample.

d2SBin is based on the mechanism that relative sequence compositions are similar across different regions of the same genome, but differ between genomes. Current tools generally used the normalized frequency of k-tuple directly, which actually is the absolute instead of relative sequence composition. Therefore, we attempted to model the relative sequence composition and to measure the dissimilarity between contigs with d2S. We applied d2SBin to adjust the outputs of five widely-used contig-binning tools on six datasets. The experiments showed that d2sBin can improve the contig binning performance significantly. 

The d2SBin pipeline was developed with Python and run on the Unix and Linux platform, and the detail description of running is provided in the following. 

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
 	2.Enter your specified directory: *bash-3.2$ cd /home/user/d2SBin*  
 	3.Extract the tar file:  *bash-3.2$ tar -xvf d2SBin_SourceCode.tar*  
 	4.Enter the directory:  *bash-3.2$ cd /home/user/d2SBin/d2SBin_SourceCode*  
 	5.If your operating system has multiple Python version, please be sure your Python version at least 2.7 or above.  

## Running of d2SBin Pipeline  

### 1. Uesage of d2SBin  
  
   The command can be viewed by typing *python d2SBin.py -h* on the command line:

- Options:

	-h, --help: show this help message and exit.  
	-s, --inputList_withSeq: List of path and file name of contig files with sequence(Format1).  
	-c, --contig_Seq: The original contig sequences file.  
	-n, --inputList_noSeq: List of path and name of files with contig name (Format2).  
	-k, --kofKTuple: The value k of KTuple  
	-r, --order: The order of markov model(0,1,2,3,k-2)  
	-o,  --output: The output path of d2SBin results  

### 2. Input: The output of existing contig-binning tools.
The current output of contig-binning have the following two formats:
- Format1: fasta files with contigs sequence from the same bins. For example, *MaxBin.out.001.fasta*

	`>contig-1.0`  
	`GACACTTTTAGTGGGCGTAAACTTCATCTAGTGGATCT`  
	`>contig-1.1`  
	`CCATGTCAGAAGAAGTTGGTAATCGCCACATTAATTGTTTGTCGTTTGATCGA`  
	`…`  
- Format2: fasta files only with contig name from the same bins. For example, *MetaCluster.out.001.fasta*  

	`>contig-1.0`  
	`>contig-1.1`  
	`…`  
	
d2SBin is compatible to the two formats as the following commands:

- **Format1 input**

  In this case, d2SBin needs one list file as input.  
  
  First, get list file:   
  
	 *`$ ls /home/.../MaxBin.out*.fasta > input_file_list_fomat1.txt`*   
	
  Next, run d2SBin.py:   
  
	 *`$ python d2SBin.py -s input_file_list_fomat1.txt -k 6 -r 0 -o ../data/output/`*  
	
- **Format2 input**

	As for format2, d2SBin needs two input files: the original contig sequences file, e.g: *contigs.fasta*, and list file of other tools output.   
  
	First, get the list file:  	
  
	 *`$ ls /home/.../MetaCluster.out*.fa > input_file_list_fomat2.txt`*     
	
 	Next, run d2SBin.py:  
  
	 *`$ python d2SBin.py -n input_file_list_format2.txt -c contigs.fasta -k 6 -r 0 -o ../data/output`*     

### 3. Output 
d2SBin also has two formats of output:  
- Format 1: .txt file with contig name and binning result label. eg. *d2SBin.out.k6.r0.txt*  

	`>contig-1.1, 3`  
	`>contig-1.2, 0`  
	
- Format 2: fasta files with sequence from same bins. eg. *d2SBin.k6.r0.out0.fasta*  

	`>contig-1.0`  
	`GACACTTTTAGTGGGCGTAAACTTCATCTAGTGGATCT`  
	`>contig-1.1`  
	`CCATGTCAGAAGAAGTTGGTAATCGCCACATTAATTGTTTGTCGTTTGATCGA`   

### 4. Evaluate output   
we provide a script *evaluation.py* for computing performance of binning results. The usage can be viewed by typing *python evaluation.py -h* on the command line:
- Options:  

 	-h, --help: show this help message and exit  
  	-c, --binning_result: binning result file. e.g, d2SBin.k6.r0.txt  
 	-t, --ture_label: true label of contigs  
  	-e, --eva_output_dir: the path of evaluation file  
   e.g:
	*`$ python evaluation.py -c d2SBin.k6.r0.txt -t real_label.txt -e ./eva`*   



## The demo of d2SBin on testing dataset. 

Dataset1: 10 genomes-20×[(Testing dataset Download)](https://raw.githubusercontent.com/kunWangkun/d2SBin/master/testing_data.rar)  

The dataset was generated by MaxBin1.0[[1]](http://downloads.jbei.org/data/microbial_communities/MaxBin/MaxBin.html).For the 10 genomes metagenomes, the 5 millian paired-end reads were samples as 20× average coverage. The short reads were simulated by MetaSim and assembled to contigs by Velvet. 

1. The assembled contigs 20x.fasta were binned by any contig-binnning tools, such as MaxBin[[1]](http://downloads.jbei.org/data/microbial_communities/MaxBin/MaxBin.html) or MetaCluster3.0[[2]](http://i.cs.hku.hk/~alse/MetaCluster/download.html) as followed commands.  

	*`$ perl run_MaxBin.pl -contig 20x.fasta -abund 20x.abund -out myout`*    
	*`$ ./bin/metaCluster 20x.fasta`*  

	The contig-binning results from Maxbin is *myout.XXX.fasta*.     
	The contig-binning results from MetaCluster3.0 is *20x.fasta-outXXX.fa*.  
	XX are numbers, e.g. *myout.001.fasta, 20x.fasta-out001.fa*  
	They are two types of output formats.  

2. Get list file  

	*`$ ls /home/…/output_of_MaxBin/myout*.fasta > MaxBin_input_list.txt`*  
	*`$ ls /home/…/output_of_MetaCluster3/20x.fasta-out*.fa > MetaCluster_input_list.txt`*

3. d2SBin running  
- For Maxbin, d2SBin is applied to adjust the contig binning,  

	*`$ python d2SBin.py -s MaxBin_input_list.txt -k 6 -r 0 -o ../MaxBin_output`*
	
 The output are *./MaxBin_output/d2SBin.out.k6.r0.txt* and *d2SBin.k6.r0.outXX.fasta*.  

- For MetaCluster3.0, d2SBin is applied to adjust the contig binning,  

	*`$ python d2SBin.py -n MetaCluster_input_list.txt -c 20x.fasta -k 6 -r 0 -o ../MetaCluster_output`*  
	
 The output are *./MetaCluster_output/d2SBin.out.k6.r0.txt* and *d2SBin.k6.r0.outXX.fasta*.

4. The calculation of performance of d2SBin binning results  

	*`$ python evaluation.py -c ../MaxBin_output/d2SBin.out.k6.r0.txt –t 20x_real_label.loc –e ./output/eva`* 
	
	where the *20x_real_label.loc* is the file of real contig labels.   
	
    Result:  
    
	|contig_num    |genome_num    |bin_num    |Recall    |Precision   |ARI  
	---------------|--------------|-----------|----------|------------|----------
	|40812         |10            |3    	  |0.9690546 |0.8286553   |0.6921886


## References

*[1]Wu, Y.-W., et al., MaxBin: an automated binning method to recover individual genomes from metagenomes using an expectation-maximization algorithm. Microbiome, 2014. 2(1): p. 26.*

*[2]Leung, H.C., et al., A robust and accurate binning algorithm for metagenomic sequences with arbitrary species abundance ratio. Bioinformatics, 2011. 27(11): p. 1489-1495.*
