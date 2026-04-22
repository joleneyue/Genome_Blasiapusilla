## 1. polishing genome using racon and pilon

racon will use the long reads and the pilon will use the short reads

install racon

     git clone --recursive https://github.com/lbcb-sci/racon.git racon

     cd racon
     mkdir build
     cd build
     cmake -DCMAKE_BUILD_TYPE=Release -Dracon_enable_cuda=ON ..
     make


packaging:

     make package

install mekada through conda:
  
     conda create -n medaka -c conda-forge -c bioconda medaka 

     conda activate medaka

install pilon from website:
  
      wget https://github.com/broadinstitute/pilon/releases

pilon usage: at least 16 G to the JVM
  
     java -Xmx100G -jar 

in order to use racon , we need to map the long reads to the assembly: we use bwa for this task

     bwa index assembly.fasta

     bwa mem -t 14 -x pacbio assembly.fasta new_viridiplantae.fasta > mapping.sam

run racon :

     ~/Yuling_20June2022/Blasia_helsinki_rawdata_pacbio/racon/build/bin/./racon -m 8 -x -6 -g -8 -w 500 -t 14 new_viridiplantae.fasta mapping.sam assembly.fasta > racon.fasta


run quast and busco for the racon polishing mapping with bwa:

     ~/storage2/Yuling/executables/quast/./quast.py  racon.fasta  -r  assembly.fasta -o quast_output


     busco -i racon.fasta -o viridiplantae_output -m genome -l viridiplantae

     busco -i racon.fasta -o embyophyta_output -m genome -l embryophyta

     --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
     |C:78.3%[S:78.1%,D:0.2%],F:11.8%,M:9.9%,n:425     |
     |333    Complete BUSCOs (C)                       |
     |332    Complete and single-copy BUSCOs (S)       |
     |1      Complete and duplicated BUSCOs (D)        |
     |50     Fragmented BUSCOs (F)                     |
     |42     Missing BUSCOs (M)                        |
     |425    Total BUSCO groups searched               |
     --------------------------------------------------
  
     --------------------------------------------------
     |Results from dataset embryophyta_odb10           |
     --------------------------------------------------
     |C:55.8%[S:55.1%,D:0.7%],F:11.5%,M:32.7%,n:1614   |
     |902    Complete BUSCOs (C)                       |
     |890    Complete and single-copy BUSCOs (S)       |
     |12     Complete and duplicated BUSCOs (D)        |
     |186    Fragmented BUSCOs (F)                     |
     |526    Missing BUSCOs (M)                        |
     |1614   Total BUSCO groups searched               |
     --------------------------------------------------

the results are really bad.

## 2.another way of polishing genome : peter recommend me the blockpolish, it only use the long reads by using racon and minimap2

install depedencies

     pip3 install pyyaml editdistance python-Levenshtein biopython tensorboardX
     pip3 install torch
     pip3 install tqdm

     pip3 install http://download.pytorch.org/whl/cpu/torch-0.4.1-cp36-cp36m-linux_x86_64.whl
     pip3 install torchvision


test the torch:

     python3
     import torch
     x = torch.rand(5, 3)
      print(x)

     tensor([[0.9500, 0.1506, 0.9366],
        [0.9019, 0.9707, 0.5307],
        [0.2560, 0.0253, 0.1804],
        [0.6919, 0.4607, 0.5524],
        [0.5223, 0.1854, 0.5630]])

install blockpolish

     git clone https://github.com/huangnengCSU/BlockPolish.git
     cd BlockPolish
     python3 brnnctc_generate.py -h

step1:
run minimap2 for the mapping, map the long raw reads to the assembly:
with -a , the output file will be sam file, without -a, the output will be paf file.

     ~/storage2/Yuling/executables/minimap2-2.17_x64-linux/./minimap2 -x map-pb assembly.fasta new_viridiplantae.fasta -t 12 > reads2asm.paf

run racon:

      ~/Yuling_20June2022/Blasia_helsinki_rawdata_pacbio/racon/build/bin/./racon new_viridiplantae.fasta reads2asm.paf assembly.fasta -t 12 > racon_cons0.fasta
     

step2:
align the raw reads to the racon polished assembly:

     ~/storage2/Yuling/executables/minimap2-2.17_x64-linux/./minimap2 -ax map-pb racon_cons0.fasta new_viridiplantae.fasta -t 14 > reads2racon.sam

     samtools view -bS -@ 40 reads2racon.sam -o reads2racon.bam

     samtools sort -@ 40 reads2racon.bam -o reads2racon.sorted.bam

     samtools index -@ 40 reads2racon.sorted.bam

step3 : divide draft assembly and generate feature matrices:

install BPFGM:

     sudo apt-get -y install make gcc g++ zlib1g-dev

     wget https://github.com/Kitware/CMake/releases/download/v3.19.4/cmake-3.19.4-Linux-x86_64.sh && sudo mkdir /opt/cmake && sudo sh cmake-3.19.4-Linux-x86_64.sh --prefix=/opt/cmake --skip-license && sudo ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

     cmake --version

     git clone https://github.com/huangnengCSU/BPFGM.git 

     mkdir build
     cd build
      cmake ..
     make
     ./block

run block:

    
     ~/Yuling_20June2022/Blasia_helsinki_rawdata_pacbio/BlockPolish/BPFGM/build/./block -b reads2racon.sorted.bam -s trivial_features.txt -c complex_features.txt

step4: polishing trivial blocks and comple blocks

polishing trivial blocks with trivial config file `test_trivial_config.yaml` and trivial model file `trivial_model.chkpt`

     python3 brnnctc_generate.py -config config/test_trivial_config.yaml -model trivial_model.chkpt -data trivial_features.txt -output trivial_polished.txt


polishing complex blocks with complex config file `test_complex_config.yaml` and complex model file `complex_model.chkpt`

     python3 brnnctc_generate.py -config config/test_complex_config.yaml -model complex_model.chkpt -data complex_features.txt -output complex_polished.txt

merge polishing results of trivial blocks and complex blocks

     python3 needle.py --t trivial_polished.txt --c complex_polished.txt -output polished_assembly.fa

i do not remember why we did not choose to use blockpolish results eventually, i will check on that .


## 3. nextpolish 

install nextpolish, next polish can combine both long and short reads. 

     wget https://github.com/Nextomics/NextPolish/releases/download/v1.4.1/NextPolish.tgz

     pip3 install paralleltask

     tar -vxzf NextPolish.tgz 
     cd NextPolish 
     make

modify the nextpolish scripts: the first line from python to python3

test the nextpolish:

     ./nextPolish test_data/run.cfg

run nextpolish for the combined short and long reads

prepare the short reads

     ls All.Illumina.Runs.R1.fastq  All.Illumina.Runs.R2.fastq > sgs.fofn

prepare the long reads:

     ls new_viridiplantae.fasta > lgs.fofn

create a  run.cgf:

      [General]
     job_type = local
     job_prefix = nextPolish
     task = best
     rewrite = yes
     rerun = 3
     parallel_jobs = 6
     multithread_jobs = 5
     genome = ./assembly.fasta
     genome_size = auto
     workdir = ./01_rundir
     polish_options = -p {multithread_jobs}

     [sgs_option]
     sgs_fofn = ./sgs.fofn
     sgs_options = -max_depth 100 -bwa

     [lgs_option]
     lgs_fofn = ./lgs.fofn
     lgs_options = -min_read_len 1k -max_depth 100
     lgs_minimap2_options = -x map-ont


run the nextpolish

     ./nextPolish run.cfg

run quast and busco for the polished genome

     ~/storage2/Yuling/executables/quast/./quast.py  genome.nextpolish.fasta  -r  assembly.fasta -o quast_output

     busco -i genome.nextpolish.fasta -o viridiplantae_output -m genome -l viridiplantae

     busco -i genome.nextpolish.fasta -o embyophyta_output -m genome -l embryophyta

     --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
       --------------------------------------------------
      |C:90.6%[S:90.1%,D:0.5%],F:4.5%,M:4.9%,n:425      |
      |385    Complete BUSCOs (C)                       |
      |383    Complete and single-copy BUSCOs (S)       |
      |2      Complete and duplicated BUSCOs (D)        |
      |19     Fragmented BUSCOs (F)                     |
     |21     Missing BUSCOs (M)                        |
     |425    Total BUSCO groups searched               |
     --------------------------------------------------
  
      --------------------------------------------------
     |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
     |C:79.1%[S:77.4%,D:1.7%],F:5.1%,M:15.8%,n:1614    |
      |1277   Complete BUSCOs (C)                       |
     |1249   Complete and single-copy BUSCOs (S)       |
     |28     Complete and duplicated BUSCOs (D)        |
     |82     Fragmented BUSCOs (F)                     |
     |255    Missing BUSCOs (M)                        |
     |1614   Total BUSCO groups searched               |
     --------------------------------------------------

nextpolish results are almost as good as polca. 
will run the 3D-DNA analysis in the scaffolding session.

## 4. polca 

     tar -vxzf MaSuRCA-4.0.9.tar.gz 

     cd MaSuRCA-4.0.9

     ./install.sh

run polca, using the short reads:

     ~/Yuling_20June2022/Blasia_helsinki_rawdata_pacbio/MaSuRCA-4.0.9/bin/./polca.sh -a assembly.fasta -r 'All.Illumina.Runs.R1.fastq All.Illumina.Runs.R2.fastq' -t 14 -m 1G

run busco and quast

     ~/storage2/Yuling/executables/quast/./quast.py  assembly.fasta.PolcaCorrected.fa  -r assembly.fasta  -o quast_output

     busco -i assembly.fasta.PolcaCorrected.fa -o viridiplantae_output -m genome -l viridiplantae

     busco -i assembly.fasta.PolcaCorrected.fa -o embyophyta_output -m genome -l embryophyta

     --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
     |C:91.0%[S:90.1%,D:0.9%],F:4.5%,M:4.5%,n:425      |
     |387    Complete BUSCOs (C)                       |
     |383    Complete and single-copy BUSCOs (S)       |
     |4      Complete and duplicated BUSCOs (D)        |
     |19     Fragmented BUSCOs (F)                     |
     |19     Missing BUSCOs (M)                        |
     |425    Total BUSCO groups searched               |
     --------------------------------------------------
  
     --------------------------------------------------
     |Results from dataset embryophyta_odb10           |
     --------------------------------------------------
     |C:79.0%[S:77.0%,D:2.0%],F:5.3%,M:15.7%,n:1614    |
     |1275   Complete BUSCOs (C)                       |
     |1243   Complete and single-copy BUSCOs (S)       |
     |32     Complete and duplicated BUSCOs (D)        |
     |85     Fragmented BUSCOs (F)                     |
     |254    Missing BUSCOs (M)                        |
     |1614   Total BUSCO groups searched               |
     -------------------------------------------------- 

polca has the best busco scores. we decided to use polca polished version assembly for the downstream analysis. 

## 5. nextflow:

     git clone https://github.com/isugifNF/polishCLR.git

     cd polishCLR


install dependency through miniconda:

     [[ -d env ]] || mkdir env
     conda env create -f environment.yml -p ~/Yuling_20June2022/Blasia_helsinki_rawdata_pacbio/polishCLR/env/polishCLR_env

first map the pacbio reads to the assembly using minimap2:

     ~/storage2/Yuling/executables/minimap2-2.17_x64-linux/./minimap2 -ax map-pb assembly.fasta new_viridiplantae.fasta -t 14 > aligned.sam

     samtools view -bS -@ 40 aligned.sam -o aligned.bam

     samtools sort -@ 40 aligned.bam -o aligned.sorted.bam

run nextflow:

     nextflow run main.nf --primary_assembly "assembly.fasta" --illumina_reads "All.Illumina.Runs.R{1,2}.fastq" --pacbio_reads "aligned.sorted.bam" -resume


 



    




  










  
