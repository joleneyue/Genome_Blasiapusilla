## 1. genome assembly after cleaning using flye. 

re-ssemble the reads again 
assemble the  mapped reads using flye: 

      ~/storage2/Yuling/executables/Flye/bin/flye --pacbio-raw new_viridiplantae.fasta --out-dir out_PACBIO --threads 8

busco

     busco -i assembly.fasta -o viridiplantae_output -m genome -l viridiplantae

     busco -i assembly.fasta -o embyophyta_output -m genome -l embryophyta

     --------------------------------------------------
     |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:89.2%[S:87.8%,D:1.4%],F:5.9%,M:4.9%,n:425      |
     |379    Complete BUSCOs (C)                       |
     |373    Complete and single-copy BUSCOs (S)       |
     |6      Complete and duplicated BUSCOs (D)        |
     |25     Fragmented BUSCOs (F)                     |
     |21     Missing BUSCOs (M)                        |
     |425    Total BUSCO groups searched               |
     --------------------------------------------------
  
     --------------------------------------------------
     |Results from dataset embryophyta_odb10           |
     --------------------------------------------------
     |C:73.5%[S:71.9%,D:1.6%],F:7.2%,M:19.3%,n:1614    |
     |1186   Complete BUSCOs (C)                       |
     |1160   Complete and single-copy BUSCOs (S)       |
     |26     Complete and duplicated BUSCOs (D)        |
     |116    Fragmented BUSCOs (F)                     |
     |312    Missing BUSCOs (M)                        |
     |1614   Total BUSCO groups searched               |
     --------------------------------------------------

quast
  
      ~/storage2/Yuling/executables/quast/./quast.py  assembly.fasta  -r  ISAR.hipmer.final_assembly.fa -o quast_mapped_output


## 2. genome assembly after cleaning using nextdenovo:

     wget https://github.com/Nextomics/NextDenovo/releases/download/v2.5.0/NextDenovo.tgz

     pip install paralleltask

     tar -vxzf NextDenovo.tgz && cd NextDenovo

     ./nextDenovo test_data/run.cfg

run nextdenovo:

     ls new_viridiplantae.fasta > input.fofn

prepare the config file:

     [General]
     job_type = local # here we use SGE to manage jobs
     job_prefix = nextDenovo
     task = all
     rewrite = yes
     deltmp = yes
     parallel_jobs = 22
     input_type = raw
     read_type = clr # clr, ont, hifi
     input_fofn = ./input.fofn
     workdir = ./01_rundir

     [correct_option]
     read_cutoff = 1k
     genome_size = 400000000 # estimated genome size
     sort_options = -m 50g -t 30
     minimap2_options_raw = -t 8
     pa_correction = 5
     correction_options = -p 30

     [assemble_option]
     minimap2_options_cns = -t 8
     nextgraph_options = -a 1

     ./nextDenovo  run.cfg

the 128 G machine is out of memory, i requested 256 G memory to finish the run on the science cluster:

data transfer:

     scp new_viridiplantae.fasta yyue@cluster.s3it.uzh.ch:data

     job.sh:
  
     #!/usr/bin/env bash
      #SBATCH --cpus-per-task=32
      #SBATCH --mem=250000
      #SBATCH --time=72:00:00
      #SBATCH --output=job.out
  
     module load hpc
     conda activate base
     /home/cluster/yyue/data/NextDenovo/./nextDenovo /home/cluster/yyue/data/NextDenovo/run.cfg

change the input file path in the run.cfg with full path.
  
     sbatch job.sh # run the job
     squeue -u yyue # check the job

the log file is in the job.out

stats:

     Type           Length (bp)            Count (#)
     N10               693834                  35
     N20               481644                  92
     N30               383117                 167
     N40               307309                 261
     N50               243077                 378
     N60               197236                 525
     N70               161335                 704
     N80               126028                 932
     N90                93651                1226

     Min.               10574                   -
     Max.             1558193                   -
     Ave.              188555                   -
     Total          322618869                1711

run busco and quast

     
     ~/storage2/Yuling/executables/quast/./quast.py  nd.asm.fasta -o quast_output
     
     busco -i nd.asm.fasta -o viridiplantae_output -m genome -l viridiplantae

     busco -i nd.asm.fasta -o embyophyta_output -m genome -l embryophyta


     --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
     |C:68.5%[S:68.0%,D:0.5%],F:8.0%,M:23.5%,n:425     |
     |291    Complete BUSCOs (C)                       |
     |289    Complete and single-copy BUSCOs (S)       |
     |2      Complete and duplicated BUSCOs (D)        |
     |34     Fragmented BUSCOs (F)                     |
     |100    Missing BUSCOs (M)                        |
     |425    Total BUSCO groups searched               |
     --------------------------------------------------
  
     --------------------------------------------------
     |Results from dataset embryophyta_odb10           |
     --------------------------------------------------
     |C:51.0%[S:50.4%,D:0.6%],F:8.0%,M:41.0%,n:1614    |
     |823    Complete BUSCOs (C)                       |
     |814    Complete and single-copy BUSCOs (S)       |
     |9      Complete and duplicated BUSCOs (D)        |
     |129    Fragmented BUSCOs (F)                     |
     |662    Missing BUSCOs (M)                        |
     |1614   Total BUSCO groups searched               |
     --------------------------------------------------


so the busco scores are very low, we decided to use flye assembly for the downstream analysis.



https://github.com/tbenavi1/genomescope2.0?tab=readme-ov-file]]. 


     mkdir kmc_tmp

     git clone --recurse-submodules https://github.com/refresh-bio/kmc.git
     cd kmc
     make -j32
 
# error message, re install all dependency

     pip3 install Cython
     pip3 install numpy
     pip3 install --upgrade setuptools

     sudo apt-get install gcc
     sudo apt-get install python3-dev
     sudo apt-get install zlib1g-dev

     pip3 install --upgrade pybind11
    sudo apt-get install --reinstall python3-dev

     make -j32


# genomescope github says the short ares are more accurate. so i will use the short reads in this case. 

# then run with the short reads from gaurav

     ls *.fastq > FILES

     ~/USERS/Yuling/Blasia_Genome/counting_kmer/kmc/bin/./kmc -k21 -t10 -m64 -ci1 -cs10000 @FILES reads ~/USERS/Yuling/Blasia_Genome/counting_kmer/kmc_tmp/ 

 ~/USERS/Yuling/Blasia_Genome/counting_kmer/kmc/bin/./kmc_tools transform ~/USERS/Yuling/Yuling_20June2022/Blasia_helsinki_rawdata_pacbio/Polca/reads histogram reads.histo -cx10000

     Stage 1: 100%
    Stage 2: 100%
    1st stage: 4336.78s
    2nd stage: 1266.27s
    Total    : 5603.05s
    Tmp size : 146252MB

    Stats:
    No. of k-mers below min. threshold :            0
    No. of k-mers above max. threshold :            1
    No. of unique k-mers               :   6899548350
    No. of unique counted k-mers       :   6899548349
    Total no. of k-mers                : 121494176863
    Total no. of reads                 :    881330776
    Total no. of super-k-mers          :  17762376590

    Stage 1: 100%
Stage 2: 100%
1st stage: 491.045s
2nd stage: 102.98s
Total    : 594.024s
Tmp size : 22211MB

Stats:
   No. of k-mers below min. threshold :            0
   No. of k-mers above max. threshold :            1
   No. of unique k-mers               :   1252013166
   No. of unique counted k-mers       :   1252013165
   Total no. of k-mers                :  19451264885
   Total no. of reads                 :    150818422
   Total no. of super-k-mers          :   2659871141



# counting kmer for the female blasia:

ls /home/ubuntu/USERS/Yuling/C/Blasia_890/HIC/*.fq > file_list.txt


~/USERS/Yuling/D/counting_kmer/kmc/bin/kmc \
  -k21 -t10 -m64 -ci1 -cs10000 \
  @file_list.txt \
  reads ~/USERS/Yuling/D/counting_kmer/kmc_tmp/


# long reads 
*********************************************
Stage 1: 100%
Stage 2: 100%
1st stage: 2328.72s
2nd stage: 1397.34s
Total    : 3726.06s
Tmp size : 65649MB

Stats:
   No. of k-mers below min. threshold :            0
   No. of k-mers above max. threshold :            0
   No. of unique k-mers               :  40013835798
   No. of unique counted k-mers       :  40013835798
   Total no. of k-mers                :  55669236518
   Total no. of reads                 :      2971204
   Total no. of super-k-mers          :   7916980311


# hic reads : short reads ?


*********************************************************************************************************************
Stage 1: 33%
Stage 1: 54%
Stage 1: 70%
Stage 1: 76%
Stage 1: 100%

Stage 2: 83%
Stage 2: 100%
1st stage: 6283.5s
2nd stage: 1751.21s
Total    : 8034.71s
Tmp size : 129379MB

Stats:
   No. of k-mers below min. threshold :            0
   No. of k-mers above max. threshold :            0
   No. of unique k-mers               :  12722584679
   No. of unique counted k-mers       :  12722584679
   Total no. of k-mers                : 105654261464
   Total no. of reads                 :    812884974
   Total no. of super-k-mers          :  15792188552

/home/ubuntu/USERS/Yuling/D/counting_kmer/kmc/bin/kmc_tools transform reads histogram reads.histo

    

## After you have the histogram file, you can run GenomeScope within the online web tool, or at the command line.

# the result is not so good. peter suggested to map the raw reads to the genome assembly and get the mapped reads to do this again.

## using bowtie2 to map the reads to the genome:

     #!/bin/bash

    # Set variables
     REFERENCE_GENOME="assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta"
     READS_1="All.Illumina.Runs.R1.fastq"
     READS_2="All.Illumina.Runs.R2.fastq "
     ALIGNED_SAM="aligned_reads.sam"
     ALIGNED_BAM="aligned_reads.bam"
     SORTED_BAM="aligned_reads.sorted.bam"
     MAPPED_READS_1="mapped_reads_1.fastq"
     MAPPED_READS_2="mapped_reads_2.fastq"

# Step 1: Index the reference genome
     bwa index $REFERENCE_GENOME

# Step 2: Align the paired-end reads to the reference genome
     bwa mem $REFERENCE_GENOME $READS_1 $READS_2 > $ALIGNED_SAM

# Step 3: Convert SAM to BAM
     samtools view -Sb $ALIGNED_SAM > $ALIGNED_BAM

# Step 4: Sort the BAM file
     samtools sort $ALIGNED_BAM -o $SORTED_BAM

# Step 5: Extract mapped reads to FASTQ format
     samtools fastq -F 4 -1 $MAPPED_READS_1 -2 $MAPPED_READS_2 $SORTED_BAM

# Cleanup intermediate files if needed
     rm $ALIGNED_SAM
     rm $ALIGNED_BAM

     echo "Mapping and extraction completed."

## with the new mapped reads, i wll re-run the kmer counting again. 


# the results of the genome scope is pretty good :
# need to use version 2.0, it has haploiid choice 


GenomeScope version 2.0
input file = user_uploads/3RJXmuc4VD94olyGHBsC
output directory = user_data/3RJXmuc4VD94olyGHBsC
p = 1
k = 21

property                      min               max               
Homozygous (a)                100%              100%              
Genome Haploid Length         379,499,316 bp    382,852,798 bp    
Genome Repeat Length          170,495,993 bp    172,002,597 bp    
Genome Unique Length          209,003,323 bp    210,850,201 bp    
Model Fit                     47.3745%          88.4734%          
Read Error Rate               0.640819%         0.640819%    



# female blasia genomescope

GenomeScope version 2.0
input file = user_uploads/afBEfg7J0gSZsjkopdbu
output directory = user_data/afBEfg7J0gSZsjkopdbu
p = 1
k = 21

property                      min               max               
Homozygous (a)                100%              100%              
Genome Haploid Length         553,513,697 bp    Inf bp            
Genome Repeat Length          216,530,882 bp    Inf bp            
Genome Unique Length          336,982,814 bp    Inf bp            
Model Fit                     55.0644%          6.19016%          
Read Error Rate               0.303887%         0.303887% 


# rerun the kmc with different parameter: filter low count to reduce noise 

~/USERS/Yuling/D/counting_kmer/kmc/bin/kmc \
  -k21 -t10 -m64 -ci2 -cs10000 \
  @file_list.txt \
  reads ~/USERS/Yuling/D/counting_kmer/kmc_tmp/



  
