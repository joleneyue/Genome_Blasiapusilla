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




  
