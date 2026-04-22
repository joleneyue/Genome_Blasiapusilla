# since the repeat masker only detect the repeats , not only the TE elements, 
# also includes some micro satelites.

# using EDTA to annotate the TE elements.


     gff2bed < BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff > BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.bed


# download the EDTA

     git clone https://github.com/oushujun/EDTA.git

     conda install -c conda-forge mamba python=3.6

     mamba install -c conda-forge -c bioconda edta python=3.6 tensorflow=1.14 'h5py<3'

     conda activate EDTA
     perl EDTA.pl

     git clone https://github.com/oushujun/LTR_retriever.git


# give the LTR_retriever ath to the EDTA.pl


     cd ./EDTA/test

     perl ../EDTA.pl --genome genome.fa --cds genome.cds.fa --curatedlib ../database/rice6.9.5.liban --exclude genome.exclude.bed --overwrite 1 --sensitive 1 --anno 1 --evaluate 1 --threads 10


     cd genome

# no curated library

     perl ../EDTA.pl --genome assembly.fasta.PolcaCorrected.FINAL.fasta --cds  helsinki_sex_gene_appended_cds.fasta  --exclude BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed.bed --overwrite 1 --sensitive 1 --anno 1 --evaluate 1 --threads 10


# the genome i am using is the full scaffolds genome.
# run the same for the genome 9 scaffolds

     perl ../EDTA.pl --genome assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta --cds  helsinki_9scaffolds_sex_gene_appended_cds.fasta  --exclude BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.bed --overwrite 1 --sensitive 1 --anno 1 --threads 10


# do the same for marchanthia

     conda update --all

     conda create -n EDTA python=3.6

     conda activate EDTA

     conda install -c conda-forge mamba python=3.6

     mamba install -c conda-forge -c bioconda edta python=3.6 tensorflow=1.14 'h5py<3'



     cd test

     perl ../EDTA.pl --genome genome.fa --cds genome.cds.fa --curatedlib ../database/rice6.9.5.liban --exclude genome.exclude.bed --overwrite 1 --sensitive 1 --anno 1 --evaluate 1 --threads 10


     git clone https://github.com/oushujun/LTR_retriever.git


# copy matchanthia file to the server 

     scp -i /Users/jolene/Desktop/PSC_course/bio226_keyfile/bio226 -r marchanthia_genome_9scaffolds ubuntu@172.23.50.148:/home/ubuntu/USERS/Yuling/Blasia_Genome/Blasia_genome_annotation/comparative_genomics/EDTA/.


     perl ../EDTA.pl --genome MpTak_v6.1r2.genome_9chroms.fasta --cds  MpTak_v6.1r2_cds.fa --exclude MpTak_v6.1r2_9chromos.bed --overwrite 1 --sensitive 1 --anno 1 --force 1 --threads 10 &> EDTA.log



# re-run EDTA for the blasia female and male mix:



# since the repeat masker only detect the repeats , not only the TE elements, 
# also includes some micro satelites.

# using EDTA to annotate the TE elements.



    gff2bed < 890_ragtag_new_sorted_geneadded.gff  > 890_ragtag_new_sorted_geneadded.bed


tar xvf RepeatMasker-4.1.9.tar 

cd RepeatMasker

perl ./configure

TRF_dir=/home/ubuntu/USERS/Yuling/executables/RepeatMasker/TRF.pm
rmblast_dir=/home/ubuntu/USERS/Yuling/executables/rmblast-2.14.1/bin


cd repeatModeler

  perl ./configure -rscout_dir /home/ubuntu/USERS/Yuling/executables/RepeatScout -recon_dir /home/ubuntu/USERS/Yuling/executables/RECON-1.08/bin -cdhit_dir /home/ubuntu/USERS/Yuling/executables/cdhit/ -genometools_dir /home/ubuntu/USERS/Yuling/executables/genometools/bin -ltr_retriever_dir /home/ubuntu/USERS/Yuling/D/comparative_genomics/Liuyang/EDTA_ragtag_genome/EDTA/LTR_retriever-2.9.0 -mafft_dir /home/ubuntu/miniconda3/bin -ninja_dir /home/ubuntu/USERS/Yuling/executables/NINJA-0.98-cluster_only/NINJA -repeatmasker_dir /home/ubuntu/USERS/Yuling/executables/RepeatMasker/ -rmblast_dir /home/ubuntu/USERS/Yuling/executables/rmblast-2.14.1/bin -trf_dir /home/ubuntu/USERS/Yuling/executables/TRF-4.09.1 -ucsctools_dir /home/ubuntu/USERS/Yuling/executables/ucsc 

# download the EDTA

conda create -n EDTA
conda activate EDTA 

git clone https://github.com/oushujun/EDTA.git
perl ./EDTA/EDTA.pl

 mamba install -c bioconda annosine2
 mamba install tir-learner
 mamba install HelitronScanner
 mamba install LTR_FINDER_parallel
mamba install ltr_retriever
mamba install RepeatModeler
mamba install mdust

mamba install mdust


     cd ./EDTA/test

     perl ../EDTA.pl --genome genome.fa --cds genome.cds.fa --curatedlib ../database/rice6.9.5.liban   --exclude genome.exclude.bed --overwrite 1 --sensitive 1 --anno 1 --evaluate 1 --threads 10


# need to activate the conda EDTA env to run EDTA perl file

   perl /home/ubuntu/USERS/Yuling/D/comparative_genomics/Liuyang/EDTA_ragtag_genome/EDTA/EDTA.pl \
  --genome ragtag.new.fasta \
  --cds cds.fa \
  --exclude 890_ragtag_new.bed \
  --overwrite 1 \
  --sensitive 1 \
  --step all \
  --anno 1 \
  --threads 10



ln -s /home/ubuntu/miniconda3/envs/EDTA/bin/LTR_retriever .

# This creates a shortcut (symlink) to LTR_retriever in the current directory, which is exactly what EDTA wants.

perl ../EDTA.pl --genome ragtag.new.fasta --cds cds.fa --exclude 890_ragtag_new.bed --overwrite 1 --sensitive 1 --anno 1 --threads 10 --force 1


  # run the test

     perl ../EDTA.pl --genome genome.fa --cds genome.cds.fa --curatedlib ../database/rice6.9.5.liban  --exclude genome.exclude.bed --overwrite 1 --sensitive 1 --anno 1 --evaluate 1 --threads 10
  

perl ../EDTA.pl --genome ragtag.new.fasta --cds ragtag_cds.fa --exclude 890_ragtag_new_sorted_geneadded.bed --overwrite 1 --sensitive 1 --anno 1 --threads 6  --force 1 > EDTA_run.log 2>&1 


perl ../EDTA.pl --genome MpTak_v6.1r2.genome_9chroms.fasta --cds MpTak_v6.1r2_cds.fa --exclude MpTak_v6.1r2_9chromos.bed --overwrite 1 --sensitive 1 --anno 1 --threads 6   > EDTA_run.log 2>&1 

perl ../EDTA.pl --genome assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta --cds helsinki_9scaffolds_sex_gene_appended_cds.fasta --exclude BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.bed --overwrite 1 --sensitive 1 --anno 1 --threads 6 > EDTA_run.log 2>&1 



# run helitronscanner from EDTA individually.

java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanHead -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/head.lcvs  -bs 1000000 -g scaffold_9.fasta -th 6 -o head.helitronscanner_Bp9.out


# scan for the 3' end using the default training set (1MB slices and 16 threads)

java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanTail -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/tail.lcvs  -bs 1000000 -g assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta -th 6 -o tail.helitronscanner_male.out


# scan for the 5' end using the default training set(1MB slices and 16 threads)


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanHead -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/head.lcvs  -bs 1000000 -g assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta -th 6 -o head.helitronscanner_male.out

ragtag.scaffold.fasta


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanTail -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/tail.lcvs  -bs 1000000 -g ragtag.scaffold.fasta -th 6 -o tail.helitronscanner_female.out


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanHead -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/head.lcvs  -bs 1000000 -g ragtag.scaffold.fasta -th 6 -o head.helitronscanner_female.out



MpTak_v6.1r2.genome_9chroms.fasta

java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanTail -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/tail.lcvs  -bs 1000000 -g MpTak_v6.1r2.genome_9chroms.fasta -th 6 -o tail.helitronscanner_marchanthia.out


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  scanHead -lf /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/TrainingSet/head.lcvs  -bs 1000000 -g MpTak_v6.1r2.genome_9chroms.fasta -th 6 -o head.helitronscanner_marchanthia.out




#you can mess with the -hlr option to increase or decrease the min and max length of predictions

java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  pairends -hs head.helitronscanner_male.out -ts tail.helitronscanner_male.out -hlr 200:20000 -o paired_male.helitrons


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar draw -p paired_male.helitrons -g assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta -o draw_helitrons_pure --pure


less draw_helitrons_pure.hel.fa|bioawk -c fastx '{print length($seq)}' | /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/summary.sh


# do this for blasia female 

java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  pairends -hs head.helitronscanner_female.out -ts tail.helitronscanner_female.out -hlr 200:20000 -o paired_female.helitrons


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar draw -p paired_female.helitrons -g  ragtag.scaffold.fasta -o draw_helitrons_female_pure --pure


less draw_helitrons_pure.hel.fa|bioawk -c fastx '{print length($seq)}' | summary.sh


# do the same for marchanthia :


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar  pairends -hs head.helitronscanner_marchanthia.out -ts tail.helitronscanner_marchanthia.out -hlr 200:20000 -o paired.helitrons


java -jar /home/ubuntu/USERS/Yuling/D/comparative_genomics/EDTA/bin/HelitronScanner/HelitronScanner.jar draw -p paired.helitrons -g MpTak_v6.1r2.genome_9chroms.fasta -o draw_helitrons_pure --pure


less draw_helitrons_pure.hel.fa|bioawk -c fastx '{print length($seq)}' | summary.sh


bioawk -c fastx '{ print length($seq) }' draw_helitrons_pure.hel.fa | \
awk '{ sum+=$1; count++ } END { print "Helitron count:", count, "\nTotal Helitron length (bp):", sum }'

bioawk -c fastx '{ print length($seq) }' draw_helitrons_female_pure.hel.fa | \
awk '{ sum+=$1; count++ } END { print "Helitron count:", count, "\nTotal Helitron length (bp):", sum }'




awk 'BEGIN{OFS="\t"} $3 == "helitron" {print $1, $4-1, $5, $9, ".", $7}' helitrons.gff > helitrons.bed


bedtools getfasta -fi assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta -bed helitrons.bed -s -name -fo helitrons_extracted.fa


# blast helitrons old male results against the helitron database from dfam :



wget https://dfam.org/releases/current/families/Dfam-RepeatMasker.lib.gz
gunzip Dfam-RepeatMasker.lib.gz


awk '/^>/ {f = /Helitron/} f {print}' Dfam-RepeatMasker.lib > dfam_helitrons.fa

makeblastdb -in dfam_helitrons.fa -dbtype nucl -out dfam_helitrons_db


 blastn -query helitrons_clean.fa -db dfam_helitrons_db \
  -task blastn-short \
  -word_size 7 \
  -evalue 1e-3 \
  -max_target_seqs 5 \
  -outfmt 6 \
  -num_threads 8 \
  -out helitron_vs_dfam_relaxed.tsv


query_id  subject_id  %identity  alignment_length  mismatches  gap_opens  q.start  q.end  s.start  s.end  evalue  bit_score


grep "TE" helitron_vs_dfam_relaxed.tsv| wc -l
   75709



# do the same for the blasia new EDTA helitrons:


sed -E 's/^>.*ID=([^;]+).*/>\1/' helitrons_extracted.fa > helitrons_clean.fa



 blastn -query helitrons_extracted.fa -db dfam_helitrons_db \
  -task blastn-short \
  -word_size 7 \
  -evalue 1e-3 \
  -max_target_seqs 5 \
  -outfmt 6 \
  -num_threads 8 \
  -out helitron_vs_dfam_relaxed.tsv


  # do the same for the female :

grep "helitron" ragtag.new.fasta.mod.EDTA.TEanno.gff3 > helitron.gff3

awk 'BEGIN{OFS="\t"} $3 == "helitron" {print $1, $4-1, $5, $9, ".", $7}' helitron.gff3 > helitrons.bed


bedtools getfasta -fi ragtag.new.fasta -bed helitrons.bed -s -name -fo helitrons_extracted.fa


sed -E 's/^>.*ID=([^;]+).*/>\1/' helitrons_extracted.fa > helitrons_clean.fa

(base) jolene@jolenedeMacBook-Air new_EDTA_blasia_female % grep "TE" helitron_vs_dfam_relaxed.tsv| wc -l
   31213




   awk 'BEGIN{OFS="\t"}
   {if ($1 == "1") $1="Bp_6";
  else if ($1 == "2") $1="Bp_3";
  else if ($1 == "3") $1="Bp_1";
  else if ($1 == "4") $1="Bp_5";
  else if ($1 == "5") $1="Bp_4";
  else if ($1 == "6") $1="Bp_2";
  else if ($1 == "7") $1="Bp_8";
  else if ($1 == "8") $1="Bp_7";
  else if ($1 == "9") $1="Bp_9";
  print}' assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.mod.EDTA.TEanno.gff3 > assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.mod.EDTA.TEanno_reordered.gff3



 # re-run old EDTA v2.1.3 for the femal egnome :

 using python3.6 

 absl-py==0.9.0

 tensorflow 1.14.0 

 h5py==2.10.0

repeatmoderler==2.0.3


 conda activate /home/ubuntu/USERS/Yuling/condaEnvs/EDTA213

# run the test:

     perl ../EDTA.pl --genome genome.fa --cds genome.cds.fa --curatedlib ../database/rice6.9.5.liban  --exclude genome.exclude.bed --overwrite 1 --sensitive 1 --anno 1  --threads 10


perl ../EDTA.pl --genome ragtag.new.fasta --cds cds.fa --exclude 890_ragtag_new_sorted_geneadded.bed --overwrite 1 --sensitive 1 --anno 1 --threads 6  > EDTA_run.log 2>&1 


perl ../EDTA.pl --genome assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta --cds helsinki_9scaffolds_sex_gene_appended_cds.fasta --exclude BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.bed --overwrite 1 --sensitive 1 --anno 1 --threads 6 > EDTA_run.log 2>&1 

perl ../EDTA.pl --genome MpTak_v6.1r2.genome_9chroms.fasta --cds MpTak_v6.1r2_cds.fa --exclude MpTak_v6.1r2_9chromos.bed  --overwrite 1 --sensitive 1 --anno 1 --threads 6   > EDTA_run.log 2>&1 

## 1. comparative annotation using augustus:

### 1. cactus

install cactus
   
       git clone https://github.com/ComparativeGenomicsToolkit/cactus.git --recursive
   
       cd cactus
      virtualenv -p python3.7 cactus_env
      echo "export PATH=$(pwd)/bin:\$PATH" >> cactus_env/bin/activate
      echo "export PYTHONPATH=$(pwd)/lib:\$PYTHONPATH" >> cactus_env/bin/activate
      source cactus_env/bin/activate
      python3 -m pip install -U setuptools pip
      python3 -m pip install -U .
      python3 -m pip install -U -r ./toil-requirement.txt
   
install dependencies: 
   
solve the docker permission problem:
   
      sudo groupadd docker
      whoami
      sudo usermod -aG docker ubuntu
      sudo chown ubuntu:docker /var/run/docker.sock
      sudo chmod g+rwx /var/run/docker.sockpip install cwltool==3.1.20210922203925
 
align genomes:
   
      mkdir /vol3/genome_alignment/
     
      vi blasia_accessions.seqfile
   
       cactus jobStore blasia_accessions.seqfile alignment.hal --maxCores 10 2>&1 > cactus.out

here i am encoutering the sementation fault and core dumped.
i asked the cactus developers, and they send me the replies and i have to check that out.

#alexander shared with me his way of installing and running cactus.:

this is the email from him:
Dear Yuling,

the cactus pipeline running on bio226 has successfully finished. The output hal alignment file is saved in ~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation/alignment.hal

I tried many different things to get cactus running properly, but in the end a rather simple solution fixed all problems. The developers of cactus provide a complete docker instance with everything properly set up. You can start this container in interactive mode with the following command:

      docker run -v $(pwd):/data --rm -it quay.io/comparative-genomics-toolkit/cactus:v2.4.3 bash

You can always put the container into the background by pressing Ctrl + p and Ctrl + q to continue work in the normal terminal and also reattach to the running container with „docker attach <name of the container>“

      docker ps

       docker attach " container ID"

When the docker container is running, the cactus pipeline can be started without further setup. Here’s the command I used to start the pipeline with your data:

       cactus ./tmp_dir blasia_accessions.seqfile_modified alignment.hal --workDir ./cactus_run/

The run took roughly 12 hours to complete and the results are stored in the hal alignment file.

I changed the paths in blasia_accesion.seqfile to relative paths, otherwise cactus won’t find them when using the docker container. Also the workDir (/cactus_run) has to be created before running cactus in the docker container.

I hope this is helpful for you. If you need more info or have other problems, I’m happy to help.

Have a nice weekend! All the best,

Alex

2. extracting alignment statistics with haltools  

now continue to run the comparative analysis :


      git clone https://github.com/ComparativeGenomicsToolkit/hal.git

      sudo apt install libhdf5-dev
      

      wget http://www.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.1/src/hdf5-1.10.1.tar.gz 
      
      tar xzf  hdf5-1.10.1.tar.gz

      cd hdf5-1.10.1

      ./configure --enable-cxx 

       make && make install

      export PATH=/hdf5-1.10.1/bin:${PATH} 


        git clone https://github.com/ComparativeGenomicsToolkit/sonLib.git

      pushd sonLib && make && popd

      #the rest sofwares are optional:

      wget http://www.netlib.org/clapack/clapack.tgz

      tar -xvzf clapack.tgz
      mv CLAPACK-3.2.1 clapack
      cd clapack 
      export CLAPACKPATH=~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation/clapack/
      cd ..

install phast:
       
       git clone https://github.com/CshlSiepelLab/phast.git
      cd phast
       git checkout 85f7ed179dd097a86ba4added22d571785cc3e1d
      cd src && make
      cd ../..
       export ENABLE_PHYLOP=1

       cd hal/
      make
       export PATH=~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation/hal/bin:${PATH}
       export PYTHONPATH=~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation:${PYTHONPATH}


now start to run the halstats:
      #get the percentage identical of polca genome -9 scaffolds with feywei genome -9 scaffolds:

  ~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation/hal/bin/halStats --percentID Polca alignment.hal

      # softmasked genome version:

      Genome, % ID, numID, numSites
Polca, 1, 365790845, 365790845
Anc0, 0.995534, 317315452, 318738992
Feywei, 0.994275, 314216692, 316025817


      #get percent identical of Feywei genome with other genomes

      halStats --percentID  Feywei alignment.hal


#softmasked genome:

Genome, % ID, numID, numSites
Feywei, 1, 338112429, 338112429
Anc0, 0.995293, 319384937, 320895445
Polca, 0.994275, 314216692, 316025817


       halStats --sequenceStats Feywei alignment.hal

       SequenceName, Length, NumTopSegments, NumBottomSegments
HiC_scaffold_1, 25368466, 73021, 0
HiC_scaffold_2, 28638123, 60266, 0
HiC_scaffold_3, 40294081, 109671, 0
HiC_scaffold_4, 56421293, 140496, 0
HiC_scaffold_5, 30066339, 75631, 0
HiC_scaffold_6, 31673694, 74331, 0
HiC_scaffold_7, 61595479, 133583, 0
HiC_scaffold_8, 42775773, 94230, 0
HiC_scaffold_9, 26213469, 60118, 0

      halStats --sequenceStats Polca  alignment.hal
     SequenceName, Length, NumTopSegments, NumBottomSegments
HiC_scaffold_1, 32808566, 97560, 0
HiC_scaffold_2, 46960569, 113908, 0
HiC_scaffold_3, 68046419, 170639, 0
HiC_scaffold_4, 33940421, 87011, 0
HiC_scaffold_5, 44059094, 134931, 0
HiC_scaffold_6, 61757168, 179749, 0
HiC_scaffold_7, 29732083, 75023, 0
HiC_scaffold_8, 31024484, 75309, 0
HiC_scaffold_9, 29542758, 95562, 0



      #get coverage information for all genomes, further analysis in R

      halStats --allCoverage alignment.hal > alignment_allCoverage.txt


#Evaluation blasia whole genome alignment Polca accession vs. feywei accession

#input files created with haltools halStats on alignment file

      #read-in coverage table

      cov <- read.table("alignment_allCoverage.txt", sep = ",")

      #fix header
       names(cov) <- cov[1,]
      cov <- cov[-1,]

      #load libraries
      library(tidyverse)

       #make suitable data frame
      cov <- as.data.frame(t(cov))

      names(cov) <- c("PolcaPolca","FeyweiPolca","PolcaFeywei","FeyweiFeywei")

      cov <- cov[-(1:2),]

      cov$times_covered <- seq(1,9,1)

      row.names(cov) <- c()


      cov <- transform(cov, PolcaPolca = as.numeric(PolcaPolca), FeyweiPolca = as.numeric(FeyweiPolca), PolcaFeywei = as.numeric(PolcaFeywei), FeyweiFeywei = as.numeric(FeyweiFeywei))


      #make histogram

      ggplot(cov, aes(times_covered, FeyweiPolca)) +
      geom_point() +
      scale_y_log10() +
      labs(x = "aligned x times", y = "number of sites (log-transformed)") +
      ggtitle("How often do Feywei sites align to Polca") +
      theme_bw()

      ggplot(cov, aes(times_covered, PolcaFeywei)) +
       geom_point() +
      scale_y_log10() +
      labs(x = "aligned x times", y = "number of sites (log-transformed)") +
       ggtitle("How often do Polca sites align to Feywei") +
       theme_bw()


image.png


creating .wig files for alignments to check alignment depth

#create wig file for Polca aligned to Feywei

      halAlignmentDepth --targetGenomes Feywei alignment.hal Polca > alignment_depth_PolcaVSfeywei.wig

#create wig file for Feywei aligned to Polca

       halAlignmentDepth --targetGenomes Polca alignment.hal Feywei > alignment_depth_feyweiVSpolca.wig
 
calculate coverage by hand from wig files

##polca
#aligned sites
      grep -v "chrom=" alignment_depth_PolcaVSfeywei.wig | grep "1" | wc -l
      
      
      329456189

#not aligned sites

       grep -v "chrom=" alignment_depth_PolcaVSfeywei.wig| grep "0" | wc -l

       48415373
       

      #48415373 / (48415373+329456189)= 0.1281265326



       grep -v "chrom=" alignment_depth_feyweiVSpolca.wig | grep "1" | wc -l

      325059315


       grep -v "chrom=" alignment_depth_feyweiVSpolca.wig | grep "0" | wc -l

   17987402

        17987402/(17987402+325059315)=0.05243426
        343046717

        halSummarizeMutations alignment.hal > mutations.txt

        GenomeName, ParentName, BranchLength, GenomeLength, ParentLength, Subtitutions, Transitions, Transversions, Matches, GapInsertions, GapInsertedBases, GapDeletions, GapDeletedBases, Insertions, InsertionBases, Deletions, DeletionBases, Inversions, InvertedBases, Duplications, DuplicatedBases, Transpositions, TranspositionBases, Other

         Feywei, Anc0, 0, 343046717, 325545100, 3700590, 598582, 1214416, 323172338, 72092, 308711, 4026, 53542, 16901, 12946406, 252, 48331, 24, 646784, 6917, 1793705, 48, 386421, 0
       Polca, Anc0, 0, 377871562, 325545100, 2528340, 698656, 1380889, 330440386, 145163, 529343, 278, 3704, 21663, 34946480, 28, 2486, 38, 224756, 36993, 7636155, 67, 138151, 0
      Total, ,0, 720918279, 651090200, 6228930, 1297238, 2595305, 653612724, 217255, 838054, 4304, 57246, 38564, 47892886, 280, 50817, 62, 871540, 43910, 9429860, 115, 524572, 0
      Average, ,0, 360459139, 325545100, 3114465, 648619, 1297652, 326806362, 108627, 419027, 2152, 28623, 19282, 23946443, 140, 25408, 31, 435770, 21955, 4714930, 57, 262286, 0

SNPs
To compute the point mutations (SNPs) between a given pair of genomes in the HAL graph, halSnps can be used:

 halSnps alignment.hal Feywei Polca --bed feywei_polca_snps.bed


to calculate per chromosome coverage, one can split the file by chromosomes (chrom= line) and do above calculations for each split file; 

3. convert hal alignment to maf using the perl script from augustus:
here my augustus is conda version, so i will used it from conda bin:

      export PATH=~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation/hal/bin:${PATH}

       /home/ubuntu/miniconda3/bin/hal2maf_split.pl --halfile alignment.hal --refGenome Polca --cpus 14 --chunksize 1000000 --overlap 25000 --outdir maf 

4. Loading genomes into SQLite database

       for f in $PWD/genomes_softmasked/*.fasta.masked ; do echo -ne "$(basename $f .fasta.masked)\t$f\n"; done > genomes.tbl

      while read line
      do
      species=$(echo "$line" | cut -f 1)
      genome=$(echo "$line" | cut -f 2)
      /home/ubuntu/miniconda3/bin/load2sqlitedb --chunksize 1000000 --noIdx --species=$species --dbaccess=blasia.db $genome
      done < genomes.tbl
      /home/ubuntu/miniconda3/bin/load2sqlitedb --makeIdx --dbaccess=blasia.db

test sqlite database

       sqlite3 -header -column blasia.db "SELECT speciesname, sum(end-start+1) AS 'genome length', count(*) AS '# chunks', count(distinct seqnr) AS '# seqs' FROM genomes natural join speciesnames GROUP BY speciesname;"



      speciesname  genome length  # chunks    # seqs    
       -----------  -------------  ----------  ----------
       Feywei       343046717      348         9         
      Polca        377871562      382         9   

5. Load RNA-Seq Hints into SQLite DB

prepare the intro hints file and exon parts hints file:

i followed this page :
http://bioinf.uni-greifswald.de/bioinf/wiki/pmwiki.php?n=IncorporatingRNAseq.GSNAP

#make the hint files:

      samtools merge merged.bam *.bam
  
      samtools sort -o merged.sorted.bam --output-fmt BAM  merged.bam

      samtools sort -n polca_merged.sorted.bam -o polca_merged.sorted.sorted.bam

      
       /home/ubuntu/miniconda3/bin/filterBam --uniq --paired --pairwiseAlignment --in polca_9_merged.sorted.sorted.bam --out polca_9_merged.sorted.sorted.filter.bam
 
      -------------------------------------
      Processed alignments: 1943024020

      Summary of filtered alignments:
      -------------------------------------
      unmapped        : 105984946
      percent identity: 219
      coverage        : 0
      not paired (our criterion)      : 548309
      quantiles of unspliced insert lengths:
      q[10%]=191,q[20%]=218,q[30%]=241,q[40%]=267,q[50%]=311,q[60%]=419,q[70%]=597,q[80%]=833,q[90%]=1326,
      not unique      : 82198796
       singleton       : 22017762
      on diff. target : 3589398
       -------------------------------------

      samtools view -H polca_9_merged.sorted.sorted.filter.bam > header.txt

      samtools sort polca_9_merged.sorted.sorted.filter.bam -o polca_9_merged.sorted.sorted.filter.sorted.bam


      /home/ubuntu/miniconda3/bin/bam2hints --intronsonly --in=polca_9_merged.sorted.sorted.filter.sorted.bam --out=hints.gff

# create the exon parts

      /home/ubuntu/miniconda3/bin/bam2wig polca_9_merged.sorted.sorted.filter.sorted.bam> polca_9_merged.sorted.sorted.filter.sorted.wig

       cat polca_9_merged.sorted.sorted.filter.sorted.wig | /home/ubuntu/miniconda3/bin/wig2hints.pl --width=10 --margin=10 --minthresh=2 --minscore=4 --prune=0.1 --src=W --type=ep \ 
       --UCSC=unstranded.track --radius=4.5 --pri=4 --strand="." > hints.exonparts.gff

# join the intron hints file and the exonparts hints file 

       cat hints.gff hints.exonparts.gff > Polca_9_hints.gff


       for f in $PWD/hints/*.gff; do echo -ne "$(basename $f .hints.gff)\t$f\n"; done > hints.tbl


#make backup of db without hints for future runs with different evidence

       cp blasia.db blasia_rnaseq.db

       while read line
       do
       species=$(echo "$line" | cut -f 1)
       hints=$(echo "$line" | cut -f 2)
       /home/ubuntu/miniconda3/bin/load2sqlitedb --noIdx --species=$species --dbaccess=blasia_rnaseq.db $hints
      done < hints.tbl
       /home/ubuntu/miniconda3/bin/load2sqlitedb --makeIdx --dbaccess=blasia_rnaseq.db


test the database:

       sqlite3 -header -column blasia_rnaseq.db "SELECT count(*) AS '#hints',typename,speciesname FROM (hints as H join featuretypes as F on H.type=F.typeid) natural join speciesnames GROUP BY speciesid,typename;"

      #hints      typename    speciesname
       ----------  ----------  -----------
      7577599     exonpart    Polca   
      411858      intron      Polca 


6. make the cfg file

       cp /home/ubuntu/miniconda3/config/extrinsic/extrinsic-cgp.cfg extrinsic-rnaseq.cfg
#replace "none" with the corresponding species name in relevant groups

#generateing the tree.newwick file:

      ~/Blasia_Genome/Blasia_genome_annotation/augustus_comparative_annotation/hal/bin/halStats --tree alignment.hal > tree.nwk

trre file distance is 0.0, the augustuscgp doe not like it, it requires the positive value, so i changed it to Polca 0.1 and Feywei 0.2 

      /home/ubuntu/miniconda3/bin/augustus: ERROR
        Branch lengths must be > 0 in ../tree.nwk.

change the tree file manually


7. Run comparative Augustus with RNAseq hints from Zurich

       mkdir augCGP_rnaseq_softmasked
       cd augCGP_rnaseq_softmasked


/home/ubuntu/miniconda3/bin/augustus: ERROR
        Branch lengths must be > 0 in ../tree.nwk.

# create here softlinks to the alignment chunks for convenience

       num=1; for f in ../maf/*.maf; do ln -s $f $num.maf; ((num++)); done


      for ali in *.maf
      do
      id=${ali%.maf} # remove .maf suffix
      /home/ubuntu/miniconda3/bin/augustus \
      --species=BlasiaPusilla\
      --treefile=../tree.nwk \
      --alnfile=$ali \
      --dbaccess=../blasia_rnaseq.db \
      --speciesfilenames=../genomes.tbl \
      --alternatives-from-evidence=0 \
      --dbhints=1 \
      --allow_hinted_splicesites=atac \
      --extrinsicCfgFile=../extrinsic-rnaseq.cfg \
      --/CompPred/outdir=pred$id > aug$id.out 2> err$id.out 
      done


# merge gene predictions from parrel runs:

      mkdir joined_pred

      while read line
      do
      species=$(echo "$line" | cut -f 1)
       find pred* -name "${species}.cgp.gff" >${species}_gtfs.lst;
      /home/ubuntu/miniconda3/bin/joingenes -f ${species}_gtfs.lst -o joined_pred/$species.gff
      done < ../genomes.tbl


      mkdie joined_pred_alternatives

      while read line
      do
       species=$(echo "$line" | cut -f 1)
       find pred* -name "${species}.cgp.gff" >${species}_gtfs.lst;
      /home/ubuntu/miniconda3/bin/joingenes -f ${species}_gtfs.lst -a -c --priorities=2,1 -o joined_pred_alternatives/$species.gff
      done < ../genomes.tbl

      2b. Softmasking of feywei genome and alignment with softmasked version of polca genome (same process as described in 3 to 7)? 


~/Blasia_Genome/Blasia_genome_annotation/busco-5.4.3/bin/./busco -i transcripts.fa -o viridiplantae_output -m transcriptome -l viridiplantae

    --------------------------------------------------
        |Results from dataset viridiplantae_odb10         |
        --------------------------------------------------
        |C:77.2%[S:75.8%,D:1.4%],F:10.1%,M:12.7%,n:425    |
        |328    Complete BUSCOs (C)                       |
        |322    Complete and single-copy BUSCOs (S)       |
        |6      Complete and duplicated BUSCOs (D)        |
        |43     Fragmented BUSCOs (F)                     |
        |54     Missing BUSCOs (M)                        |
        |425    Total BUSCO groups searched               |
        --------------------------------------------------


~/Blasia_Genome/Blasia_genome_annotation/busco-5.4.3/bin/./busco -i transcripts.fa -o embyophyta_output -m transcriptome -l embryophyta
 --------------------------------------------------
        |Results from dataset embryophyta_odb10           |
        --------------------------------------------------
        |C:66.1%[S:65.0%,D:1.1%],F:11.0%,M:22.9%,n:1614   |
        |1066   Complete BUSCOs (C)                       |
        |1049   Complete and single-copy BUSCOs (S)       |
        |17     Complete and duplicated BUSCOs (D)        |
        |178    Fragmented BUSCOs (F)                     |
        |370    Missing BUSCOs (M)                        |
        |1614   Total BUSCO groups searched               |
        --------------------------------------------------

## 1. comparative annotation on coge

load genome, gff files and wig file in coge 

geting the wig files from bam file:

merge all the bam files:
  
     samtools merge merged.bam *.bam
  
     samtools sort -o merged.sorted.bam --output-fmt BAM  merged.bam
  
     samtools index -@ 14 merged.sorted.bam


# after running these two analysis, upload the wig files into coge, it does not seems seperate.

peter recommend me to use some other way to do it, i will check that out later:

Here is what I used to use. This is for paired-end data:

Separate plus and minus strand reads from TOMOAKIS data

STEP1 (these are the R1 one reads mapping on the forward strand)

       samtools view -@5 -bh -f 99 merged.sorted.bam > merged.sorted.bamF1 

       samtools index merged.sorted.bamF1 

STEP2 (these are R2 reads mapping on the forward)

       samtools view -@5 -bh -f 147 merged.sorted.bam > merged.sorted.bamF2

       samtools index merged.sorted.bamF2 

STEP3 merge and index them. Then simply convert it to a wig file. (using bam to wig).

       samtools merge -f merged.sorted.bamF merged.sorted.bamF1 merged.sorted.bamF2 

       samtools index merged.sorted.bamF 

simply convert the bam files in wig:

       ~/Yuling_20June2022/Blasia_genome_annotation/bam2wig/bam2wig merged.sorted.bamF > merged.sorted.bamF.wig

#rev strand THE SAME AS ABOVE BUT WITH DIFF FLAG STATS

       samtools view -@8 -bh -f 83  merged.sorted.bam > merged.sorted.bamR1 

       samtools index merged.sorted.bamR1 

       samtools view -@5 -bh -f 163 merged.sorted.bam > merged.sorted.bamR2 

      samtools index merged.sorted.bamR2 

      samtools merge -f merged.sorted.bamR merged.sorted.bamR1 merged.sorted.bamR2 

      samtools index merged.sorted.bamR 

simply convert the bam files in wig:

       ~/Yuling_20June2022/Blasia_genome_annotation/bam2wig/bam2wig merged.sorted.bamR > merged.sorted.bamR.wig
        

upload all the files to coge: wig file( combined bam file, shows the coverage) and the genome annotation files from tsebra.( combined braker 1+2)

# run liftoff on the files produced from tsebra:

feywei_braker: tsebra_filter_singleexon
Polca_braker: tsebra_filter_singleexon-- using this version to add the lifted models -using this version to add UTR on 

running liftoff 

## install liftoff:

git clone https://github.com/agshumate/Liftoff liftoff 
cd liftoff
python3 setup.py install

     pip3 install liftoff
  

#using the bedtools to find the structure differences between the original genome and the liftoff genome gff file.

# since peter did not like the idea to use blastp as he does before, i decided to re-run the liftoff with the original gff annotation between the two genomes.

# this output is wrritten in the directory liftoff_bedtools 

conda activate base


      liftoff -g feywei_singleexonremoved_renamed.gff -f feature_type.txt -o polca_lifted_annotation.gff -m ~/Blasia_Genome/Blasia_genome_annotation/minimap2/./minimap2 -u polca_unmapped_features.txt -dir polca_intermediate polca_genome.fasta feywei_genome.fasta


     liftoff -g Polca_singleexonremoved_renamed.gff -f feature_type.txt -o feywei_lifted_annotation.gff -m ~/Blasia_Genome/Blasia_genome_annotation/minimap2/./minimap2 -u feywei_unmapped_features.txt -dir feywei_intermediate feywei_genome.fasta polca_genome.fasta


#the unmatched geneids are written in the ...unmatched_features.txt

# grep the gene into the new file and run the bedtools only with gene

# Step 1: Convert GFF files to BED format

convert2bed --input=gff < Polca_singleexonremoved_renamed_onlygene.gff > Polca_singleexonremoved_renamed_onlygene.bed

convert2bed --input=gff < polca_lifted_annotation_onlygene.gff > polca_lifted_annotation_onlygene.bed

# here it comes the feywei version

convert2bed --input=gff < feywei_singleexonremoved_renamed_onlygene.gff > feywei_singleexonremoved_renamed_onlygene.bed 

convert2bed --input=gff < feywei_lifted_annotation_onlygene.gff  > feywei_lifted_annotation_onlygene.bed

# Step 2: Find overlapping genes between accessions


bedtools intersect -a Polca_singleexonremoved_renamed_onlygene.bed -b polca_lifted_annotation_onlygene.bed -wao -s  > Polca_overalpping.txt


bedtools intersect -a feywei_singleexonremoved_renamed_onlygene.bed  -b feywei_lifted_annotation_onlygene.bed -wao -s  > feywei_overalpping.txt


#move the files to csv and caculate the percentage of the overlap length

#choose longer gene models over the shorter gene models :

# Open the Bedtools intersect output file for reading

import csv

# Open the Bedtools intersect output file for reading
with open('Polca_overlapping.csv', 'r') as f, open('longer_genes.csv', 'w', newline='') as out_file:
    
     # Write the header to the output file
    writer = csv.writer(out_file)
    writer.writerow(['gene_id1', 'gene_id2', 'gene_len_H', 'gene_len_A', 'overlap_len', 'overlap_rate'])
    
    # Skip the first line (header) of the file
    next(f)
    for line in f:
        # Split the current line by comma,
        fields = line.strip().split(',')
        # Extract the gene ID and gene length from the line
        gene_id1 = fields[9]
        gene_id2 = fields[13]
        gene_len_H = int(fields[2]) - int(fields[1]) + 1
        gene_len_A = int(fields[12]) - int(fields[11]) + 1 
        overlap_len = int(fields[20])
        overlap_rate = float(fields[22])
        
        
        if overlap_rate==0 :
            print(gene_id1 +","+gene_id2+","+str(gene_len_H)+","+str(gene_len_A-1)+","+str(overlap_len)+","+str(overlap_rate))
            out_file.write(gene_id1 +","+gene_id2+","+str(gene_len_H)+","+str(gene_len_A-1)+","+str(overlap_len)+","+str(overlap_rate)+'\n')
            
        else:
            if gene_len_H >= gene_len_A:
                print(gene_id1 + ","+ "NA" +"," + str(gene_len_H)+","+" "+","+str(overlap_len)+","+str(overlap_rate))
                out_file.write(gene_id1 + "," +"NA"+',' + str(gene_len_H) +","+" "+","+str(overlap_len)+","+str(overlap_rate)+'\n')
            else:
                print(gene_id1+","+gene_id2 + "," + "NA"+","+str(gene_len_A)+","+str(overlap_len)+","+str(overlap_rate))
                out_file.write(gene_id1+","+gene_id2 + ',' +"NA"+","+ str(gene_len_A) +","+str(overlap_len)+","+str(overlap_rate)+'\n')
        
            

and use the bash below to get the exons and cds out from the orginal annotation as the new file only contains genes.

#! /bin/bash
fileA=polca_9scaffolds_lifted_annotation.csv
fileB=polca_9scaffolds_lifted_annotation_smallerthan0.8_tabs.gff

outFile=xxx_smaller.gff

rm $outFile 
while IFS= read -r line || [ -n "$line" ]; do
       
        id=$(echo $line | sed "s/^.* ID=//" | sed "s/;.*//")
        echo $line  >> $outFile
        grep "Parent=$id;" $fileA >> $outFile
done < $fileB

sed  -ie "s/\r$//" $outFile

#upload the files onto coge.                         
   

# run  liftoff for the marchanthia proteome: 

# 19287 protein ids in marchanthia polymorpha
# feywei_9scaffolds: 13696 
# feywei: 14202
# polca: 14559
# polca_9scaffolds: 14331

#before the filtering: 
feywei_9scaffolds : 13317
feywei: 13419
polca: 13388
polca_9scafffolds:13362

#after the filtering: identity >90 and coverage > 80:

# only 871 protein ids are matched between feywei_9scaffolds and marpoly
# only 901 protein ids are matched between feywei and marpoly
# only 885 protein ids are matched between polca_9scaffolds and  marpoly
# only 901 protein ids are matched between polca and marpoly


# after discussing with peter, we decided to run the liftoff directly without blastp. 
  
#reference is polca_9scaffolds
#target is marpoly genome and gff file

 liftoff -g polca_9scaffolds_braker1+2_combined.gff -f feature_type.txt -o Marpoly_lifted_annotation.gff -m ~/Blasia_Genome/Blasia_genome_annotation/blast_proteome/minimap2/./minimap2 -u Marpoly_unmapped_features.txt -dir Marpoly_intermediate MpTak_v6.1.genome.fasta  polca_9scaffolds_genome.fasta 

 #reference is polca
 #target is marpoly

 liftoff -g polca_braker1+2_combined.gff -f feature_type.txt -o Marpoly_lifted_annotation_aginst_full_polca.gff -m ~/Blasia_Genome/Blasia_genome_annotation/blast_proteome/minimap2/./minimap2 -u marpoly_full_polca_unmapped_features.txt -dir marpoly_full_polca_intermediate MpTak_v6.1.genome.fasta  polca_genome.fasta

# using feywei genome :

#reference is feywei_9scaffolds
#target is marpoly genome and gff file

 liftoff -g feywei_9scaffolds_braker1+2_combined_tsebra_viridiplantaeAndBryophytes.gff -f feature_type.txt -o Marpoly_lifted_annotation_against_feywei_9scaffolds.gff -m ~/Blasia_Genome/Blasia_genome_annotation/blast_proteome/minimap2/./minimap2 -u Marpoly_against_feywei_9scaffolds_unmapped_features.txt -dir Marpoly_against_feywei_9scaffolds_intermediate MpTak_v6.1.genome.fasta  feywei_9scaffolds_genome.fasta 

 #reference is polca
 #target is marpoly

 liftoff -g feywei_braker1+2_combined_tsebra_viridiplantaeAndBryophytes.gff -f feature_type.txt -o Marpoly_lifted_annotation_aginst_full_feywei.gff -m ~/Blasia_Genome/Blasia_genome_annotation/blast_proteome/minimap2/./minimap2 -u marpoly_full_feywei_unmapped_features.txt -dir marpoly_full_feywei_intermediate MpTak_v6.1.genome.fasta  feywei_genome.fasta




## 1. 4 genome assembly 's transcriptome assembly using stringtie and evaluation 

polca polished genome and feywei genome assembly annotation:

     ISAR.hipmer.final_assembly.FINAL.fasta  385 Mb

     assembly.fasta.PolcaCorrected.FINAL.fasta  396Mb 

busco analysis for the first 9 scaffolds: 

      busco -i ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta -o viridiplantae_output -m genome -l viridiplantae

      --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:90.1%[S:89.4%,D:0.7%],F:4.2%,M:5.7%,n:425      |
      |383    Complete BUSCOs (C)                       |
      |380    Complete and single-copy BUSCOs (S)       |
      |3      Complete and duplicated BUSCOs (D)        |
      |18     Fragmented BUSCOs (F)                     |
      |24     Missing BUSCOs (M)                        |
      |425    Total BUSCO groups searched               |
      --------------------------------------------------

      busco -i ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta  -o embyophyta_output -m genome -l embryophyta

      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:76.8%[S:75.3%,D:1.5%],F:5.9%,M:17.3%,n:1614    |
      |1239   Complete BUSCOs (C)                       |
      |1215   Complete and single-copy BUSCOs (S)       |
      |24     Complete and duplicated BUSCOs (D)        |
      |96     Fragmented BUSCOs (F)                     |
      |279    Missing BUSCOs (M)                        |
       |1614   Total BUSCO groups searched               |
      --------------------------------------------------
  

      busco -i assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta -o Polca_viridiplantae_output -m genome -l viridiplantae

        --------------------------------------------------
       |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:90.3%[S:90.1%,D:0.2%],F:4.2%,M:5.5%,n:425      |
      |384    Complete BUSCOs (C)                       |
      |383    Complete and single-copy BUSCOs (S)       |
      |1      Complete and duplicated BUSCOs (D)        |
      |18     Fragmented BUSCOs (F)                     |
      |23     Missing BUSCOs (M)                        |
      |425    Total BUSCO groups searched               |
      -------------------------------------------------

      busco -i assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta  -o Polca_embyophyta_output -m genome -l embryophyta


      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:78.9%[S:77.3%,D:1.6%],F:5.2%,M:15.9%,n:1614    |
      |1274   Complete BUSCOs (C)                       |
      |1248   Complete and single-copy BUSCOs (S)       |
      |26     Complete and duplicated BUSCOs (D)        |
      |84     Fragmented BUSCOs (F)                     |
      |256    Missing BUSCOs (M)                        |
      |1614   Total BUSCO groups searched               |
      --------------------------------------------------

polca polished genome:

genome annotation
mapping the RNA-seq reads to the genome

check if genome is already indexed, if not. index the genome

      hisat2-build -f -p 8 assembly.fasta.PolcaCorrected.FINAL.fasta hisat2indexPolca

      hisat2-build -f -p 8 assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta  
      hisat2indexPolca_9scaffolds

      hisat2-build -f -p 8 ISAR.hipmer.final_assembly.FINAL.fasta  hisat2index_feywei

      hisat2-build -f -p 8  ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta 
      hisat2index_feywei_9scaffolds

mapping the reads to the genome index using hisat2

make a loop for both hisat2 and samtools

      #!/bin/bash

      #specify the index file
      mapping_index="hisat2index_feywei"

      #define input and outout files

      mkdir feywei_bam_dir
      mkdir feywei_sam_dir

      for i in $(ls *day* -d)
      do
      echo $i

      cd $i

      fwd_input_file=${i}_1.fq.gz
      rev_input_file=${i}_2.fq.gz

      echo $fwd_input_file
      echo $rev_input_file

      sam_files="${i}.sam"
      bam_files="${i}.bam"
      echo ${sam_files}

      ##hisat2 is using for the mapping and samtools for conversion of the sam files to bam files

      hisat2 -p 4 --dta --rna-strandness RF -x ../${mapping_index} -1 ${fwd_input_file} -2 ${rev_input_file} -S ${sam_files}

      ~/storage/software/samtools-1.9/samtools sort -@ 4 -o ${bam_files} ${sam_files}

      mv ${sam_files} ../feywei_sam_dir
      mv ${bam_files} ../feywei_bam_dir/ 
  
      cd ..
      done

stringtie 
generate transcript hints with stringtie (first run stringtie with mapped reads, then merge into single gtf, then create hints file)
extract transcripts with stringtie using already generated .bam files as sample,run the bash in to the bam_dir 

       #!/bin/bash

      #define input and output files

      mkdir gtf_dir

       for i in $(ls *day* -d)
      do
      echo $i

      gtf_files="${i}.gtf"
      bam_files="${i}"

      echo ${gtf_files}
     
      #extract transcripts with stringtie using already generated .bam files as sample

      ~/storage2/Yuling/executables/tools/necklace/tools/stringtie-2.0.3/stringtie ${bam_files} --rf -l ${i} -o ${gtf_files}

      mv ${gtf_files} gtf_dir/
  
      done

Create unified set of transcripts with stringtie --merge, run the sh file in the gtf dir

      #!/bin/bash
  
      mkdir merge_dir

      for i in $(ls *day* -d)
      do
      echo $i

      gtf_files="${i}"

      #created unified set of transcripts with stringtie --merge
      #merge assemblies

      ~/storage2/Yuling/executables/tools/necklace/tools/stringtie-2.0.3/stringtie --merge -p 12  ${gtf_files} -o out_gtf

      mv out_gtf merge_dir/
  
       done

Create hints file

convert .gtf file to .gff file

      gffread -E Stringtie_merged.gtf -o- > transcript_hints_stringtie.gff

extract the transcripts file.

      gffread -w transcripts.fa -g ~/Yuling_20June2022/Blasia_genome_annotation/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta transcript_hints_stringtie.gff 

using transdcoder to decode the transcripts.fa to get the cds file and gene file

      TransDecoder.LongOrfs -t transcripts.fa

      TransDecoder.Predict -t transcripts.fa 

output file is .bed .cds .pep .gff3.
transcripts.fasta.transdecoder.pep : peptide sequences for the final candidate ORFs; all shorter candidates within longer ORFs were removed.
transcripts.fasta.transdecoder.cds  : nucleotide sequences for coding regions of the final candidate ORFs
transcripts.fasta.transdecoder.gff3 : positions within the target transcripts of the final selected ORFs
transcripts.fasta.transdecoder.bed  : bed-formatted file describing ORF positions, best for viewing using GenomeView or IGV.

evaluation of the transcriptome assembly

evaluate the gtf with eval
   
    
     
      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/eval-2.2.8/./get_general_stats.pl -g Stringtie_merged.gtf > eval_general_stats

Assessing the Read Content of the Transcriptome Assembly
First, build a bowtie2 index for the transcriptome:

      bowtie2-build transcripts.fa transcripts.fa 

Then perform the alignment to just capture the read alignment statistics.
for paired-end reads:

       bowtie2 -p 10 -q --no-unal -k 20 -x transcripts.fa -1 ~/Yuling_20June2022/Blasia_genome_annotation/Cday10_2/Cday10_2_1.fq.gz -2 ~/Yuling_20June2022/Blasia_genome_annotation/Cday10_2/Cday10_2_2.fq.gz  2>align_stats.txt|~/storage/software/samtools-1.9/samtools view -@10 -Sb -o bowtie2.bam

        cat 2>&1 align_stats.txt

      20538334 reads; of these:
      20538334 (100.00%) were paired; of these:
      2779825 (13.53%) aligned concordantly 0 times
      10757543 (52.38%) aligned concordantly exactly 1 time
      7000966 (34.09%) aligned concordantly >1 times
      ----
      2779825 pairs aligned concordantly 0 times; of these:
      431109 (15.51%) aligned discordantly 1 time
      ----
      2348716 pairs aligned 0 times concordantly or discordantly; of these:
      4697432 mates make up the pairs; of these:
      3460439 (73.67%) aligned 0 times
      397085 (8.45%) aligned exactly 1 time
      839908 (17.88%) aligned >1 times
      91.58% overall alignment rate

feywei:

      20538334 reads; of these:
       20538334 (100.00%) were paired; of these:
      2768897 (13.48%) aligned concordantly 0 times
      10995266 (53.54%) aligned concordantly exactly 1 time
      6774171 (32.98%) aligned concordantly >1 times
      ----
      2768897 pairs aligned concordantly 0 times; of these:
      448228 (16.19%) aligned discordantly 1 time
      ----
      2320669 pairs aligned 0 times concordantly or discordantly; of these:
      4641338 mates make up the pairs; of these:
      3377049 (72.76%) aligned 0 times
      425582 (9.17%) aligned exactly 1 time
      838707 (18.07%) aligned >1 times
      91.78% overall alignment rate

feywei_9 scaffolds:

      20538334 reads; of these:
      20538334 (100.00%) were paired; of these:
      3343736 (16.28%) aligned concordantly 0 times
      10739167 (52.29%) aligned concordantly exactly 1 time
      6455431 (31.43%) aligned concordantly >1 times
      ----
      3343736 pairs aligned concordantly 0 times; of these:
      425453 (12.72%) aligned discordantly 1 time
      ----
      2918283 pairs aligned 0 times concordantly or discordantly; of these:
      5836566 mates make up the pairs; of these:
      4616828 (79.10%) aligned 0 times
      421585 (7.22%) aligned exactly 1 time
      798153 (13.68%) aligned >1 times
      88.76% overall alignment rate

Visualize read support using IGV

Sort the alignments by coordinate

      samtools sort bowtie2.bam -o bowtie2.coordSorted.bam

Index the coordinate-sorted bam file

      samtools index bowtie2.coordSorted.bam

Index the Trinity.fasta file

      samtools faidx transcripts.fa

View the aligned reads along the Trinity assembly reference contigs.
note, you can do this by using the various graphical menu options in IGV (load genome 'Trinity.fasta', load file 'bowtie2.coordSorted.bam'), or you can use the command-line tool like so:

      igv.sh -g transcripts.fa bowtie2.coordSorted.bam
    
Transcriptome Contig Nx and ExN50 stats
The 'Gene' Contig Nx Statistic

      ~/storage2/Yuling/executables/trinityrnaseq-Trinity-v2.8.4/util/TrinityStats.pl transcripts.fa

Error, cannot decipher gene identifier from acc: MSTRG.2.1

      ################################
      ## Counts of transcripts, etc.
      ################################
      Total trinity 'genes':	17804
      Total trinity transcripts:	17804
      Percent GC: 48.08

      ########################################
      Stats based on ALL transcript contigs:
      ########################################

      Contig N10: 5926
      Contig N20: 4737
      Contig N30: 4081
      Contig N40: 3579
      Contig N50: 3157

      Median contig length: 2443.5
      Average contig: 2730.25
      Total assembled bases: 48609320


note: not reporting gene-based longest isoform info since couldn't parse Trinity accession info.

feywei:
  
      ################################
      ## Counts of transcripts, etc.
      ################################
      Total trinity 'genes':	18516
      Total trinity transcripts:	18516
      Percent GC: 48.04

      ########################################
      Stats based on ALL transcript contigs:
      ########################################

	  Contig N10: 5874
	 Contig N20: 4682
	 Contig N30: 4039
	 Contig N40: 3547
	 Contig N50: 3123

	 Median contig length: 2355
	 Average contig: 2628.83
	 Total assembled bases: 48675362

feywei_9 scafoolds:

Error, cannot decipher gene identifier from acc: MSTRG.1.1


      ################################
      ## Counts of transcripts, etc.
      ################################
      Total trinity 'genes':	17742
      Total trinity transcripts:	17742
      Percent GC: 48.06

      ########################################
      Stats based on ALL transcript contigs:
      ########################################

	  Contig N10: 5911
	 Contig N20: 4715
	 Contig N30: 4070
	 Contig N40: 3572
	 Contig N50: 3146

	 Median contig length: 2416
	 Average contig: 2694.57
	 Total assembled bases: 47807062

run BUSCO analysis

run busco with two datasets

      busco -i transcripts.fa -o viridiplantae_output -m transcriptome -l viridiplantae

      --------------------------------------------------
        |Results from dataset viridiplantae_odb10         |
        --------------------------------------------------
        |C:94.1%[S:48.0%,D:46.1%],F:1.9%,M:4.0%,n:425     |
        |400    Complete BUSCOs (C)                       |
        |204    Complete and single-copy BUSCOs (S)       |
        |196    Complete and duplicated BUSCOs (D)        |
        |8      Fragmented BUSCOs (F)                     |
        |17     Missing BUSCOs (M)                        |
        |425    Total BUSCO groups searched               |
        --------------------------------------------------

      busco -i transcripts.fa -o embyophyta_output -m transcriptome -l embryophyta

     --------------------------------------------------
        |Results from dataset embryophyta_odb10           |
        --------------------------------------------------
        |C:83.3%[S:46.3%,D:37.0%],F:3.3%,M:13.4%,n:1614   |
        |1344   Complete BUSCOs (C)                       |
        |747    Complete and single-copy BUSCOs (S)       |
        |597    Complete and duplicated BUSCOs (D)        |
        |53     Fragmented BUSCOs (F)                     |
        |217    Missing BUSCOs (M)                        |
        |1614   Total BUSCO groups searched               |
        
        
feywei:
        
        --------------------------------------------------
        |Results from dataset viridiplantae_odb10         |
        --------------------------------------------------
        |C:94.6%[S:47.5%,D:47.1%],F:2.1%,M:3.3%,n:425     |
        |402    Complete BUSCOs (C)                       |
        |202    Complete and single-copy BUSCOs (S)       |
        |200    Complete and duplicated BUSCOs (D)        |
        |9      Fragmented BUSCOs (F)                     |
        |14     Missing BUSCOs (M)                        |
        |425    Total BUSCO groups searched               |
        --------------------------------------------------



        --------------------------------------------------
        |Results from dataset embryophyta_odb10           |
        --------------------------------------------------
        |C:81.9%[S:45.4%,D:36.5%],F:4.3%,M:13.8%,n:1614   |
        |1322   Complete BUSCOs (C)                       |
        |733    Complete and single-copy BUSCOs (S)       |
        |589    Complete and duplicated BUSCOs (D)        |
        |70     Fragmented BUSCOs (F)                     |
        |222    Missing BUSCOs (M)                        |
        |1614   Total BUSCO groups searched               |
        --------------------------------------------------

feywei_9 scaffolds:

        --------------------------------------------------
        |Results from dataset viridiplantae_odb10         |
        --------------------------------------------------
        |C:93.2%[S:46.4%,D:46.8%],F:2.1%,M:4.7%,n:425     |
        |396    Complete BUSCOs (C)                       |
        |197    Complete and single-copy BUSCOs (S)       |
        |199    Complete and duplicated BUSCOs (D)        |
        |9      Fragmented BUSCOs (F)                     |
        |20     Missing BUSCOs (M)                        |
        |425    Total BUSCO groups searched               |
        --------------------------------------------------
        
         --------------------------------------------------
        |Results from dataset embryophyta_odb10           |
        --------------------------------------------------
        |C:80.6%[S:44.3%,D:36.3%],F:4.0%,M:15.4%,n:1614   |
        |1301   Complete BUSCOs (C)                       |
        |715    Complete and single-copy BUSCOs (S)       |
        |586    Complete and duplicated BUSCOs (D)        |
        |64     Fragmented BUSCOs (F)                     |
        |249    Missing BUSCOs (M)                        |
        |1614   Total BUSCO groups searched               |
        --------------------------------------------------

## 2. De novo_ Repeat Identification: softmask the genome for braker

run the repeatmodeler for helsinki polca _9scaffolds genome:

generate repeatmodeler database for genome

   
      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/fasta_files/softmasked_genome/RepeatModeler/BuildDatabase -name blasia_repeatmodelerDB assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta


run repeatmodeler run long time (more than 2 day)

     
      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/fasta_files/softmasked_genome/RepeatModeler/RepeatModeler -pa 8 -database blasia_repeatmodelerDB


de novo Repeat annotation with RepeatMasker(~1h), mask the repeat region as small letters.
    
      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/fasta_files/softmasked_genome/RepeatMasker/RepeatMasker -xsmall -pa 14 -lib consensi.fa ~/Yuling_20June2022/Blasia_genome_annotation/polca_9scaffolds_repeatmodeler/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta


# do the same for the female blasia :


BuildDatabase -name blasia_repeatmodelerDB ragtag.scaffold.fasta

# run this over 2 days 

RepeatModeler -database blasia_repeatmodelerDB


de novo Repeat annotation with RepeatMasker(~1h), mask the repeat region as small letters.
    
      RepeatMasker -xsmall -pa 14 -lib ~/USERS/Yuling/D/comparative_genomics/Liuyang/repeats_annotation/RM_2757219.ThuMay221524182025/consensi.fa ragtag.scaffold.fasta

~/USERS/Yuling/D/comparative_genomics/Liuyang/repeats_annotation/RepeatMasker/util/buildSummary.pl ragtag.scaffold.fasta.out  > repeatmasker_summary.txt



Total Sequences: 14
Total Length: 389911372 bp
Class                  Count        bpMasked    %masked
=====                  =====        ========     =======
Unspecified            536110       242052482    62.08%
                      ---------------------------------
    total interspersed 536110       242052482    62.08%

Simple_repeat          45507        2211475      0.57%
---------------------------------------------------------
Total                  581617       244263957    62.65%



# run the repeatmodeler for feywei's assembly:
  
generate repeatmodeler database for genome
     
      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/fasta_files/softmasked_genome/RepeatModeler/BuildDatabase -name blasia_repeatmodelerDB ISAR.hipmer.final_assembly.FINAL.fasta

run repeatmodeler run long time (more than 2 day)

      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/fasta_files/softmasked_genome/RepeatModeler/RepeatModeler -pa 8 -database blasia_repeatmodelerDB


de novo Repeat annotation with RepeatMasker(~1h), mask the repeat region as small letters.

      ~/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/fasta_files/softmasked_genome/RepeatMasker/RepeatMasker -xsmall -pa 14 -lib consensi.fa ~/Yuling_20June2022/Blasia_genome_annotation/feywei_repeatmodeler/ISAR.hipmer.final_assembly.FINAL.fasta



## 3. genome annotation for both feyweis assembly and polca polished assembly using braker 1/2

run braker1/2 for the ananotation
    
run braker with bam files and softmasking genome
give the path to all the environmental variables
augustus version are different, the one in the miniconda3 bin is 3.3.3
the one in the excutables is 3.4.0, they are not compatible
so i decided to use all scripts, config and bin with version 3.3.3

braker1: run braker with RNA bam files(RNA hints)
braker2: run braker with the protein files(prot hints): from viridiplantae database or evolutionary close species

installl prohints for braker2:

      cpan MCE::Mutex threads YAML Thread::Queue Math::Utils

download the protein file from the orthoDB

       wget https://v100.orthodb.org/download/odb10_arthropoda_fasta.tar.gz
      tar xvf odb10_arthropoda_fasta.tar.gz

and concatenate proteins from all species into a single file:
  
      cat plants/Rawdata/* > proteins.fasta

install splan: align the protein sequences to the genome
  
      git clone https://github.com/ogotoh/spaln.git
      cd src
      ./configure --use_zlib=1
      make
      make install
      make clearall

      export PATH=$PATH:/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/spaln/bin

install some dependency:
     
      sudo cpanm File::Spec::Functions
      sudo cpanm Hash::Merge
      sudo cpanm List::Util
      sudo cpanm MCE::Mutex
      sudo cpanm Module::Load::Conditional
      sudo cpanm Parallel::ForkManager
      sudo cpanm POSIX
      sudo cpanm Scalar::Util::Numeric
      sudo cpanm YAML
      sudo cpanm Math::Utils
      sudo cpanm File::HomeDir
      sudo cpanm threads
      sudo cpanm biopython

install augustus:
  
      sudo apt install augustus augustus-data augustus-doc
  
  it turns out braker 1 does not have the braker.gtf file, and the augustus gtf file has too much genes. 70k~, this is not correct, i decided to use braker 2 to run both RNA bam file and protein file.

give the path before run braker2
  
      conda activate base

      export AUGUSTUS_BIN_PATH=/home/ubuntu/miniconda3/bin/
      export AUGUSTUS_CONFIG_PATH=/home/ubuntu/miniconda3/config/
      export AUGUSTUS_SCRIPTS_PATH=/home/ubuntu/miniconda3/bin/
      export PYTHON3_PATH=/usr/bin/
      export CDBTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export SAMTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export BAMTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export DIAMOND_PATH=/home/ubuntu/miniconda3/bin/
      export PROTHINT_PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/ProtHint/bin
      export ALIGNMENT_TOOL_PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/gth-1.7.3-Linux_i386-32bit/bin/
      export GENEMARK_PATH=/home/ubuntu/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/gmes_linux_64/
      
      #add to ~/.bashrc to include BRAKER dir into PATH
  
      export PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/BRAKER/scripts:$PATH

here is sth wrong in the Polca version genome, that fix broken genes do not run properly. so that i skip fixing the broken genes.

braker2: run braker with the protein files(prot hints): from database 

polca_genome_braker2 using the viridiplantae protein datasets
  
      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL.fasta.masked --prot_seq=proteins.fasta --softmasking --species=Blasiapusilla --epmode --useexisting --skip_fixing_broken_genes


feywei_genome_braker2 for the viridiplantae protein files: 

      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL.fasta.masked --prot_seq=proteins.fasta --softmasking --species=Blasia1 --epmode 

       ------------------------------------------------
       |Results from dataset viridiplantae_odb10         |
       --------------------------------------------------
       |C:92.3%[S:88.5%,D:3.8%],F:3.1%,M:4.6%,n:425      |
      |392    Complete BUSCOs (C)                       |
      |376    Complete and single-copy BUSCOs (S)       |
      |16     Complete and duplicated BUSCOs (D)        |
      |13     Fragmented BUSCOs (F)                     |
      |20     Missing BUSCOs (M)                        |
      |425    Total BUSCO groups searched               |
      --------------------------------------------------
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:79.6%[S:75.3%,D:4.3%],F:5.2%,M:15.2%,n:1614    |
      |1286   Complete BUSCOs (C)                       |
      |1216   Complete and single-copy BUSCOs (S)       |
      |70     Complete and duplicated BUSCOs (D)        |
      |84     Fragmented BUSCOs (F)                     |
      |244    Missing BUSCOs (M)                        |
      |1614   Total BUSCO groups searched               |
      --------------------------------------------------

run braker 2 for the polca assembly_9 scaffolds for the viridiplantae protein files:
  
       braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.masked --prot_seq=proteins.fasta --softmasking --species=blasia_Polca_9sca --epmode 

       --------------------------------------------------
       |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:91.3%[S:86.1%,D:5.2%],F:2.8%,M:5.9%,n:425      |
      |388    Complete BUSCOs (C)                       |
      |366    Complete and single-copy BUSCOs (S)       |
      |22     Complete and duplicated BUSCOs (D)        |
      |12     Fragmented BUSCOs (F)                     |
      |25     Missing BUSCOs (M)                        |
      |425    Total BUSCO groups searched               |
  
  
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:82.1%[S:76.8%,D:5.3%],F:3.5%,M:14.4%,n:1614    |
      |1324   Complete BUSCOs (C)                       |
      |1239   Complete and single-copy BUSCOs (S)       |
      |85     Complete and duplicated BUSCOs (D)        |
      |57     Fragmented BUSCOs (F)                     |
      |233    Missing BUSCOs (M)                        |
      |1614   Total BUSCO groups searched               |
       --------------------------------------------------

run braker 2 for the feywei's asssembly_9 scaffolds for the viridiplantae protein file:

       braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta.masked --prot_seq=proteins.fasta --softmasking --species=blasia_feywei_9sca --epmode 

run braker 2 for the RNA bam files:
  
      export AUGUSTUS_BIN_PATH=/home/ubuntu/miniconda3/bin/
      export AUGUSTUS_CONFIG_PATH=/home/ubuntu/miniconda3/config/
      export AUGUSTUS_SCRIPTS_PATH=/home/ubuntu/miniconda3/bin/
      export PYTHON3_PATH=/usr/bin/
      export CDBTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export SAMTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export BAMTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export DIAMOND_PATH=/home/ubuntu/miniconda3/bin/
      export GENEMARK_PATH=/home/ubuntu/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/gmes_linux_64 
      
      #add to ~/.bashrc to include BRAKER dir into PATH

      export PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/BRAKER/scripts:$PATH

run braker 2 for the RNA bam files for feywei_9scaffolds assembly:

      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta.masked --species=1  --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam --softmasking --useexisting

       --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
       --------------------------------------------------
       |C:91.3%[S:71.1%,D:20.2%],F:4.0%,M:4.7%,n:425     |
      |388	Complete BUSCOs (C)                       |
      |302	Complete and single-copy BUSCOs (S)       |
      |86	Complete and duplicated BUSCOs (D)        |
      |17	Fragmented BUSCOs (F)                     |
      |20	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
       --------------------------------------------------
  
       --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
       --------------------------------------------------
      |C:77.9%[S:59.4%,D:18.5%],F:5.4%,M:16.7%,n:1614   |
      |1257	Complete BUSCOs (C)                       |
      |959	Complete and single-copy BUSCOs (S)       |
      |298	Complete and duplicated BUSCOs (D)        |
      |87	Fragmented BUSCOs (F)                     |
      |270	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |

for the feywei assembly

       braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL.fasta.masked  --species=4  --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam --softmasking   

       --------------------------------------------------
       |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:92.0%[S:72.5%,D:19.5%],F:4.7%,M:3.3%,n:425     |
      |391	Complete BUSCOs (C)                       |
      |308	Complete and single-copy BUSCOs (S)       |
      |83	Complete and duplicated BUSCOs (D)        |
      |20	Fragmented BUSCOs (F)                     |
      |14	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
      --------------------------------------------------
  
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:78.6%[S:61.0%,D:17.6%],F:6.3%,M:15.1%,n:1614   |
      |1268	Complete BUSCOs (C)                       |
      |984	Complete and single-copy BUSCOs (S)       |
      |284	Complete and duplicated BUSCOs (D)        |
      |101	Fragmented BUSCOs (F)                     |
      |245	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |

polca polished assembly:

      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL.fasta.masked --species=2  --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam --softmasking   

      --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:92.0%[S:71.5%,D:20.5%],F:4.2%,M:3.8%,n:425     |
      |391	Complete BUSCOs (C)                       |
      |304	Complete and single-copy BUSCOs (S)       |
      |87	Complete and duplicated BUSCOs (D)        |
      |18	Fragmented BUSCOs (F)                     |
      |16	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
      --------------------------------------------------
  
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
       --------------------------------------------------
      |C:80.1%[S:60.5%,D:19.6%],F:5.3%,M:14.6%,n:1614   |
      |1293	Complete BUSCOs (C)                       |
      |977	Complete and single-copy BUSCOs (S)       |
      |316	Complete and duplicated BUSCOs (D)        |
      |86	Fragmented BUSCOs (F)                     |
      |235	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |
       --------------------------------------------------

polca polished assembly 9scaffolds

      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.masked  --species=3  --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam --softmasking   

      --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |--------------------------------------------------
      --------------------------------------------------
      --------------------------------------------------
      |C:91.1%[S:70.4%,D:20.7%],F:3.8%,M:5.1%,n:425     |
      |387	Complete BUSCOs (C)                       |
      |299	Complete and single-copy BUSCOs (S)       |
      |88	Complete and duplicated BUSCOs (D)        |
      |16	Fragmented BUSCOs (F)                     |
      |22	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
      --------------------------------------------------
  
      Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:79.7%[S:60.3%,D:19.4%],F:4.8%,M:15.5%,n:1614   |
      |1287	Complete BUSCOs (C)                       |
      |974	Complete and single-copy BUSCOs (S)       |
      |313	Complete and duplicated BUSCOs (D)        |
      |78	Fragmented BUSCOs (F)                     |
      |249	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |

  
running the RNA seq together with the viridiplantae protein file locally in one run with etpmode
  
      conda activate base

      export AUGUSTUS_BIN_PATH=/home/ubuntu/miniconda3/bin/
      export AUGUSTUS_CONFIG_PATH=/home/ubuntu/miniconda3/config/
      export AUGUSTUS_SCRIPTS_PATH=/home/ubuntu/miniconda3/bin/
      export PYTHON3_PATH=/usr/bin/
      export CDBTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export SAMTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export BAMTOOLS_PATH=/home/ubuntu/miniconda3/bin/
      export DIAMOND_PATH=/home/ubuntu/miniconda3/bin/
      export PROTHINT_PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/ProtHint/bin
      export ALIGNMENT_TOOL_PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/gth-1.7.3-Linux_i386-32bit/bin/
       export GENEMARK_PATH=/home/ubuntu/storage2/Yuling/Blasia_starvation_RNAseq_31Aug2021_Novogene/Raw_data_NOVOGENE/braker/gmes_linux_64/

      #add to ~/.bashrc to include BRAKER dir into PATH
  
       export PATH=/home/ubuntu/Yuling_20June2022/Blasia_genome_annotation/Polca_braker/BRAKER/scripts:$PATH
  
four run: polca , polca 9 scaffolds, feywei, feywei 9 scaffolds in the --etpmode with both protein and RNA bam files.

      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL.fasta.masked --species=5 --prot_seq=proteins.fasta --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam  --softmasking  --etpmode 
  
      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.masked  --species=6 --prot_seq=proteins.fasta --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam  --softmasking  --etpmode 
  

      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta.masked --species=7 --prot_seq=proteins.fasta --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam  --softmasking  --etpmode 
  

      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL.fasta.masked  --species=8 --prot_seq=proteins.fasta --bam=./bam_files/Cday1_1.bam,./bam_files/Cday1_2.bam,./bam_files/Cday1_3.bam,./bam_files/Cday2_1.bam,./bam_files/Cday2_2.bam,./bam_files/Cday2_3.bam,./bam_files/Cday3_3.bam,./bam_files/Cday3_1.bam,./bam_files/Cday3_2.bam,./bam_files/Cday7_1.bam,./bam_files/Cday7_2.bam,./bam_files/Cday7_3.bam,./bam_files/Cday8_1.bam,./bam_files/Cday8_2.bam,./bam_files/Cday8_3.bam,./bam_files/Cday9_1.bam,./bam_files/Cday9_2.bam,./bam_files/Cday9_3.bam,./bam_files/Cday10_1.bam,./bam_files/Cday10_2.bam,./bam_files/Cday10_3.bam,./bam_files/Sday1_1.bam,./bam_files/Sday1_2.bam,./bam_files/Sday1_3.bam,./bam_files/Sday2_1.bam,./bam_files/Sday2_2.bam,./bam_files/Sday2_3.bam,./bam_files/Sday3_1.bam,./bam_files/Sday3_2.bam,./bam_files/Sday3_3.bam,./bam_files/Sday7_1.bam,./bam_files/Sday7_2.bam,./bam_files/Sday7_3.bam,./bam_files/Sday8_1.bam,./bam_files/Sday8_2.bam,./bam_files/Sday8_3.bam,./bam_files/Sday9_1.bam,./bam_files/Sday9_2.bam,./bam_files/Sday9_3.bam,./bam_files/Sday10_1.bam,./bam_files/Sday10_2.bam,./bam_files/Sday10_3.bam  --softmasking  --etpmode 

etp mode predicted too many fragmented genes. we choose tsebra instead.
etp mode did not work for the polca full genome and the feywei full genome . but it works for the 9 scaffolds.

run braker 2 with the closed related 5 species protein file:
  
       braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.masked  --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep --softmasking --species=9 --epmode 
  
       braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta.masked   --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep --softmasking --species=10 --epmode 


      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL.fasta.masked   --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep --softmasking --species=12 --epmode 
  
      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL.fasta.masked   --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep --softmasking --species=13 --epmode --skip_fixing_broken_genes --useexisting

  
Polca_9scaffolds for 5 close related protein ep-mode: 5 species protein,  gene number : 17747
  
run braker2 for the both viridiplantae and the closely realted bryophytes species as protein files: 
  
      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL.fasta.masked   --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep,./proteins.fasta --softmasking --species=14 --epmode 
  
      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL.fasta.masked   --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep,./proteins.fasta --softmasking --species=15 --epmode 
  
      braker.pl --genome=./fasta_files/assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.masked  --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep,./proteins.fasta --softmasking --species=16 --epmode 
  
      braker.pl --genome=./fasta_files/ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta.masked   --prot_seq=./close_species_protein/AagrBONN_genome_PROT.fasta,./close_species_protein/Apunct_genome_PROT.fasta,./close_species_protein/Marpal_v1r1.25_02_2020.named_product_GO_FIXEDNCBI_27Feb2021_v4_sorted.PEP_OK_FINAL.fasta,./close_species_protein/Mpolymorpha_320_v3.1.protein_primaryTranscriptOnly.fa,./close_species_protein/Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa,./close_species_protein/blasia_pusilla_compreh_init_build.fasta.transdecoder_longest_ORF.pep,./proteins.fasta --softmasking --species=17 --epmode 

  
## 3. gene prediction using tsebra ( combing the braker 1 and braker 2 results)


set the configure file default.cfg " intron support " to 0.2

only RNA bam file, the genes are 20 k or so, the protein file is around 15k or so, combine them tother using tesebra is 11k .
do not know why it is so low. but rerun braker with local joining bam file and protein files. Because the "intron support" was higher before. 

config file:
  
      # src weights
      P 0.1
      E 10
      C 5
      M 1
      # Low evidence support
      intron_support 0.2
      stasto_support 1
      # Feature differences
      e_1 0
      e_2 0.5
      e_3 25
      e_4 10
    
       ./TSEBRA/bin/tsebra.py -g braker1/braker.gtf,braker2/braker.gtf -c TSEBRA/config/default.cfg -e braker1/hintsfile.gff,braker2/hintsfile.gff -o braker1+2_combined.gtf




# re_run tsebra again using the new version:

git clone https://github.com/Gaius-Augustus/TSEBRA

configure file:

# Weight for each hint source
# Values have to be >= 0
P 1
E 1
C 1
M 1
# Required fraction of supported introns 
# or supported start/stop-codons for a transcript
# Values have to be in [0,1]
intron_support 0.2
stasto_support 1
# Allowed difference for each feature 
# Values have to be in [0,2]
e_1 0.0
e_2 0.5
e_3 0.096
e_4 0.02
e_5 0.18
e_6 0.18

# non filter single exon:

~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/TSEBRA/./bin/tsebra.py -g braker1/augustus.hints.gtf,braker2/augustus.hints.gtf -c ~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/TSEBRA/config/default.cfg \
    -e braker1/hintsfile.gff,braker2/hintsfile.gff \
    -o braker1+2_combined.gtf 

    # run tsebra for the --filter_single_exon_genes

~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/TSEBRA/./bin/tsebra.py -g braker1/augustus.hints.gtf,braker2/augustus.hints.gtf -c ~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/TSEBRA/config/default.cfg -e braker1/hintsfile.gff,braker2/hintsfile.gff -o braker1+2_combined.gtf --filter_single_exon_genes


#filtering single exon only change the numbers of the unplaced scaffolds.
the first 9 scaffolds do not contain single exons.

i will use both filtered and unfiltered polca_braker results to run liftoff/bedtools with feywei_braker and later running gemoma.

# the number of mRNA did not change

rename the result file:

~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/TSEBRA/./bin/rename_gtf.py --gtf braker1+2_combined.gtf --prefix Bpus_H --translation_tab translation.tab --out tsebra_result_renamed.gtf

    # run the same for the feywei_9scaffolds genome:

    ~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/TSEBRA/./bin/rename_gtf.py --gtf braker1+2_combined.gtf --prefix Bpus_A --translation_tab translation.tab --out tsebra_result_renamed.gtf

#convert the gtf file to gff file using new gffread to keep the gene features:

    # download a new version gffread from link: https://launchpad.net/ubuntu/+source/gffread/0.12.7-2build1

then do the readme file: 

 cd /some/build/dir
  git clone https://github.com/gpertea/gffread
  cd gffread
  make release

~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/gffread-0.12.7/gffread/./gffread tsebra_result_renamed.gtf -o tsebra_result_renamed.gff --keep-genes

    
feywei_9scaffolds: combine braker1 with RNA bam files and braker2 with viridiplantae + closely related species protein files 

       --------------------------------------------------
       |Results from dataset viridiplantae_odb10         |
       --------------------------------------------------
       |C:92.7%[S:87.8%,D:4.9%],F:1.9%,M:5.4%,n:425      |
       |394    Complete BUSCOs (C)                       |
      |373    Complete and single-copy BUSCOs (S)       |
      |21     Complete and duplicated BUSCOs (D)        |
      |8      Fragmented BUSCOs (F)                     |
      |23     Missing BUSCOs (M)                        |
      |425    Total BUSCO groups searched               |
      --------------------------------------------------

       --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
       --------------------------------------------------
      |C:81.0%[S:76.0%,D:5.0%],F:3.3%,M:15.7%,n:1614    |
      |1306   Complete BUSCOs (C)                       |
      |1226   Complete and single-copy BUSCOs (S)       |
      |80     Complete and duplicated BUSCOs (D)        |
      |53     Fragmented BUSCOs (F)                     |
      |255    Missing BUSCOs (M)                        |
      |1614   Total BUSCO groups searched               |
      --------------------------------------------------
      
feywei assmbley_tsebra

       --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:92.9%[S:86.1%,D:6.8%],F:3.1%,M:4.0%,n:425      |
    
      |395	Complete BUSCOs (C)                       |
      |366	Complete and single-copy BUSCOs (S)       |
      |29	Complete and duplicated BUSCOs (D)        |
      |13	Fragmented BUSCOs (F)                     |
      |17	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
      --------------------------------------------------
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:80.9%[S:74.2%,D:6.7%],F:4.9%,M:14.2%,n:1614    |
      |1305	Complete BUSCOs (C)                       |
      |1197	Complete and single-copy BUSCOs (S)       |
      |108	Complete and duplicated BUSCOs (D)        |
      |79	Fragmented BUSCOs (F)                     |
      |230	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |

polca_9scaffolds_tsebra
  
      --------------------------------------------------
      |Results from dataset viridiplantae_odb10         |
      --------------------------------------------------
      |C:90.4%[S:83.3%,D:7.1%],F:3.8%,M:5.8%,n:425      |
      |384	Complete BUSCOs (C)                       |
      |354	Complete and single-copy BUSCOs (S)       |
      |30	Complete and duplicated BUSCOs (D)        |
      |16	Fragmented BUSCOs (F)                     |
      |25	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
      --------------------------------------------------
    
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:81.1%[S:74.2%,D:6.9%],F:4.2%,M:14.7%,n:1614    |
      |1309	Complete BUSCOs (C)                       |
      |1198	Complete and single-copy BUSCOs (S)       |
      |111	Complete and duplicated BUSCOs (D)        |
      |67	Fragmented BUSCOs (F)                     |
      |238	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |
      --------------------------------------------------


polca_braker_tsebra:
  
         --------------------------------------------------
       |Results from dataset viridiplantae_odb10         |
       --------------------------------------------------
       |C:90.8%[S:83.3%,D:7.5%],F:3.5%,M:5.7%,n:425      |
       |386	Complete BUSCOs (C)                       |
       |354	Complete and single-copy BUSCOs (S)       |
      |32	Complete and duplicated BUSCOs (D)        |
      |15	Fragmented BUSCOs (F)                     |
      |24	Missing BUSCOs (M)                        |
      |425	Total BUSCO groups searched               |
      --------------------------------------------------
    
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:81.0%[S:73.9%,D:7.1%],F:4.1%,M:14.9%,n:1614    |
      |1307	Complete BUSCOs (C)                       |
      |1192	Complete and single-copy BUSCOs (S)       |
      |115	Complete and duplicated BUSCOs (D)        |
      |66	Fragmented BUSCOs (F)                     |
      |241	Missing BUSCOs (M)                        |
      |1614	Total BUSCO groups searched               |
      --------------------------------------------------


feywei_braker_tsebra
  genes numbers: 16.1k
  
polca_9scaffolds_tsebra:
  
  gene numbers: 16.1k
  
feywei_9scaffolds_tsebra:
  gene number : 15.5k
  
polca _braker_tsebra:
  
  gene number : 16.3k



# befre running gushr, take the missing genes that is existing in feywei genome not in helsinki, and add them to helsinki genome.

 # first, take the missing genes from feywei genome out , then run liftoff agin against helsinki genme. and get the polca_lifted_annotation,gff append the gff files to polca genome.

        cut -f 20 Polca_overalpping.csv| cut -f1 -d ";" > Polca_overlapping_only_feywei_Ids.csv 

### 15468 genes that overlapping by bedtools

       grep "gene" polca_lifted_annotation.gff | cut -f9 | cut -f1 -d ";" > polca_lifted_annotation_onlyfeywei_IDs.csv

## 15977 genes are lifted by liftoff,


       grep -Fxv -f Polca_overlapping_only_feywei_Ids.csv polca_lifted_annotation_onlyfeywei_IDs.csv > feywei_nonoverlapping.csv

 ## 1189 genes are missing 


# extract all the lines together with the gene lines , based on the file2 missing genes :   and get the all other features out from the file 1, the feywei_annotation file.

       grep -Fwf <(grep -o 'ID=[^;\t]*' feywei_nonoverlapping.csv| grep -o '[^=]*$') feywei_singleexonremoved_renamed.gff > output.gff

### output.gff is a feywei files, lift feywei ;s file onto polca genome 

# bring back the missing genes , re-run liftoff 


       liftoff -g output.gff -f feature_type.txt -o lifted_annotation.gff -m ~/Blasia_Genome/Blasia_genome_annotation/minimap2/./minimap2 -u polca_unmapped_features2.txt -dir polca_intermediate2 polca_genome.fasta feywei_genome.fasta


# now i have to combine the missing genes to the polca file.

       cat Polca_singleexonremoved_renamed.gff lifted_annotation.gff  > Polca_singleexonremoved_renamed_addliftedmodels.gff

   
# after discussing with peter. we decided to usse gemoma only


2. ## Extract introns (and coverage) running:


       git clone https://github.com/soedinglab/MMseqs2.git

       cd MMseqs2
       mkdir build
       cd build
       cmake ..
       make -j4

       sudo make install

       mmseqs

       change search=$1 into search=mmseqs, tests.sh works.

# produce the intron files and covergae files:

       java -jar GeMoMa-1.9.jar CLI ERE m=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_full_bamfiles/merged.sorted.bam 

## run test for the annotation analyzer, works

       java -jar GeMoMa-1.9.jar CLI AnnotationFinalizer u=YES g=tests/gemoma/ref-fragment.fasta a=tests/gemoma/ref-annotation.gff c=UNSTRANDED rename=NO outdir=x 


# run the gemoma for the newly generated tsebra file with gene features produced by new gffread:

## still need to change the transcript into mRNA: 

       sed 's/\btranscript\b/mRNA/' Polca_singleexonremoved_renamed.gff > Polca_singleexonremoved_renamed2.gff

       sed 's/\btranscript\b/mRNA/' Polca_singleexonremoved_renamed_addliftedmodels.gff > Polca_singleexonremoved_renamed_addliftedmodels2.gff

       java -jar GeMoMa-1.9.jar CLI AnnotationFinalizer u=YES g=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/assembly.fasta.PolcaCorrected.FINAL.fasta a=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/Polca_singleexonremoved_renamed2.gff i=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/introns.gff c=UNSTRANDED coverage_unstranded=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/coverage.bedgraph rename=NO outdir=final_results

         java -jar GeMoMa-1.9.jar CLI AnnotationFinalizer u=YES g=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/assembly.fasta.PolcaCorrected.FINAL.fasta a=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/Polca_singleexonremoved_renamed_addliftedmodels2.gff i=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/introns.gff c=UNSTRANDED coverage_unstranded=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/coverage.bedgraph rename=NO outdir=final_results2


       java -jar GeMoMa-1.9.jar CLI AnnotationFinalizer u=YES g=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/assembly.fasta.PolcaCorrected.FINAL.fasta a=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/Polca_singleexonremoved_renamed2.gff i=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/introns.gff c=UNSTRANDED coverage_unstranded=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/coverage.bedgraph rename=NO outdir=repeat1

       java -jar GeMoMa-1.9.jar CLI AnnotationFinalizer u=YES g=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/assembly.fasta.PolcaCorrected.FINAL.fasta a=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/Polca_singleexonremoved_renamed_addliftedmodels2.gff i=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/introns.gff c=UNSTRANDED coverage_unstranded=~/Blasia_Genome/Blasia_genome_annotation/liftoff_bedtools_new/GeMoMa/analysis_ForAllAnnotationFiles/coverage.bedgraph rename=NO outdir=repeat2

# the final annotation for the lifted annotation and with UTR on is in the dirctory final-results2

# succeed
# using genometools to tidy up the gff file:

       gt gff3 -sort yes -tidy yes -retainids yes -checkids yes -fixregionboundaries yes -force yes -o Polca_singleexonremoved_renamed2_genometools.gff Polca_singleexonremoved_renamed2.gff

       gt loccheck Polca_singleexonremoved_renamed2_genometools.gff

## successful!

       gt gff3 -sort yes -tidy yes -retainids yes -checkids yes -fixregionboundaries yes -force yes -o Polca_singleexonremoved_renamed_addliftedmodels_genometools.gff Polca_singleexonremoved_renamed_addliftedmodels2.gff 

       gt loccheck Polca_singleexonremoved_renamed_addliftedmodels_genometools.gff 

## successful!

# run the annotation evidence for the gff file produced with utr on:

       java -jar GeMoMa-1.9.jar CLI AnnotationEvidence a=annotation_evidence/gff_file_liftedmodelsadded_singleexonremoved_withUTR.gff g=annotation_evidence/assembly.fasta.PolcaCorrected.FINAL.fasta i=annotation_evidence/introns.gff c=UNSTRANDED coverage_unstranded=annotation_evidence/coverage.bedgraph outdir=annotation_evidence

       java -jar GeMoMa-1.9.jar CLI AnnotationEvidence a=annotation_evidence/gff_file_nonliftedmodelsadded_singleexonremoved_withUTR.gff g=annotation_evidence/assembly.fasta.PolcaCorrected.FINAL.fasta i=annotation_evidence/introns.gff c=UNSTRANDED coverage_unstranded=annotation_evidence/coverage.bedgraph outdir=annotation_evidence

       java -jar GeMoMa-1.9.jar CLI GFFAttributes a=annotation_evidence/annotation_with_attributes.gff outdir=annotation_evidence




       ~/storage2/Yuling/executables/tools/necklace/tools/stringtie-2.0.3/stringtie





## first, use gffread to get the transcripts out:

## extract the transcripts file. UTR are included

       gffread -w transcripts.fa -g assembly.fasta.PolcaCorrected.FINAL.fasta gff_file_liftedmodelsadded_singleexonremoved_withUTR.gff

# extract the stringtie specific gene:

        gffread -w Stringtie_specific_transcripts.fa -g assembly.fasta.PolcaCorrected.FINAL.fasta Stringtie_specific_genemodels.gff 


# take the transcripts file to run trapid with taxonomy mode on:

## the results hshow there is 38% eukayota and 30% uncalssified genes and 30% bacterias.

## i will take out the viridiplantae hits from the transcripts and get the gff file out and upload onto coge again to compare with the symbiosis wig file, starvation wig file and braker annotation.

### to do so:

       grep "Viridiplantae" transcripts_tax_exp6250.csv > Stringtie_specific_onlyViridiplantae.csv
       grep "NA" transcripts_tax_exp6250.csv > unclasfied_transcripts.csv
       cat Stringtie_specific_onlyViridiplantae.csv unclasfied_transcripts.csv viridiplantaePlusunclassfied.csv

       cut -f2 -d "," viridiplantaePlusUnclassfied.csv > viridiplantaePlusUnclassfied_IDs.csv


       grep -Ff  viridiplantaePlusUnclassfied_IDs.csv -w Stringtie_specific_genemodels.gff > extracted_features_ViridiplantaePlusUnclassfied.gff


# run transdecoder to see how much is Complete:


       gffread -w Stringtie_specific_viridiplantaePlusUnclassied_transcripts.fa -g assembly.fasta.PolcaCorrected.FINAL.fasta extracted_features_ViridiplantaePlusUnclassfied.gff


       ~/Yuling_13June2022/raw_reads_blasia_symbiosis_2023Jan23Novegene_1/TransDecoder-TransDecoder-v5.7.0/TransDecoder.LongOrfs -t  Stringtie_specific_viridiplantaePlusUnclassied_transcripts.fa

       ~/Yuling_13June2022/raw_reads_blasia_symbiosis_2023Jan23Novegene_1/TransDecoder-TransDecoder-v5.7.0/TransDecoder.Predict -t  Stringtie_specific_viridiplantaePlusUnclassied_transcripts.fa

       grep -Ff  viridiplantaePlusUnclassfied_geneIDs.csv -w Stringtie_specific_genemodels.gff > extracted_features_ViridiplantaePlusUnclassfied.gff

### 845 genes are from the stringtie.
### 496 genes are from the viridiplantae and unclassfied.

       /home/ubuntu/miniconda3/envs/funannotate/opt/pasa-2.4.1/misc_utilities/cufflinks_gtf_genome_to_cdna_fasta.pl extracted_features_ViridiplantaePlusUnclassfied.gtf assembly.fasta.PolcaCorrected.FINAL.fasta > Stringtie_virdiplantae_unclassfied_transcripts.fasta

       /home/ubuntu/miniconda3/envs/funannotate/opt/pasa-2.4.1/misc_utilities/cufflinks_gtf_to_alignment_gff3.pl extracted_features_ViridiplantaePlusUnclassfied.gtf > extracted_features_ViridiplantaePlusUnclassfied.gff3


### Important, when running transdecoder include the --use_complete_orfs in TransDecoder.LongOrfs to avoid partial ORFs. Plus, you need to make sure you predict only one best ORF per transcript and not multiple. THis is achieved by using the --single_best_orf when running TransDecoder.Predict.


       ~/Yuling_13June2022/raw_reads_blasia_symbiosis_2023Jan23Novegene_1/TransDecoder-TransDecoder-v5.7.0/TransDecoder.LongOrfs -t tringtie_virdiplantae_unclassfied_transcripts.fasta  --complete_orfs_only

       ~/Yuling_13June2022/raw_reads_blasia_symbiosis_2023Jan23Novegene_1/TransDecoder-TransDecoder-v5.7.0/TransDecoder.Predict -t Stringtie_virdiplantae_unclassfied_transcripts.fasta  --single_best_only 

## 159 genes 

       ~/Yuling_13June2022/raw_reads_blasia_symbiosis_2023Jan23Novegene_1/TransDecoder-TransDecoder-v5.7.0/util/cdna_alignment_orf_to_genome_orf.pl Stringtie_virdiplantae_unclassfied_transcripts.fasta.transdecoder.gff3 extracted_features_ViridiplantaePlusUnclassfied.gff3 Stringtie_virdiplantae_unclassfied_transcripts.fasta > stringtie_viridiplantae_unclassfied.gff3

## 137 genes are parsed. 

# add the exon features according to the cDS 

       awk '{print} $3=="CDS" {exon_line=$0; gsub(/CDS/, "exon", exon_line); print exon_line}' gff_file_liftedmodelsadded_singleexonremoved_withUTR.gff > new_file.gff

# remove ;Name="ORF type:complete (-),score=..."

       sed 's/;Name="ORF type:[^"]*"//g' stringtie_viridiplantae_unclassfied.gff3 > stringtie_viridiplantae_unclassfied_renamed3.gff3

       cat new_file.gff stringtie_viridiplantae_unclassfied_renamed3.gff3 > output.gff

### now this should contain everything i need.
### then i need to rename this whole thing and also extract the transcripts.

       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries -addintrons output.gff > output1.gff 

       gt loccheck output1.gff

### successfully

## extract the UTR and CDS with old version gffread

       gffread -w transcripts.fasta -x cds.fasta -y protein.fasta -g assembly.fasta.PolcaCorrected.FINAL.fasta output1.gff 

### extract all the files from the gff file: with new version gffread

       ~/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/gffread-0.12.7/gffread/./gffread -w transcripts.fasta -x cds.fasta -y protein.fasta -g assembly.fasta.PolcaCorrected.FINAL.fasta output1.gff 

### final annotation is output1.gff

# without adding the exons :

       cat gff_file_liftedmodelsadded_singleexonremoved_withUTR.gff stringtie_viridiplantae_unclassfied_renamed3.gff3 > output2.gff 

       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries output2.gff > output3.gff 

       gt loccheck output3.gff

       gffread -w transcripts.fasta -x cds.fasta -y protein.fasta -g assembly.fasta.PolcaCorrected.FINAL.fasta output3.gff 

       mv output3.gff  gff_file_liftedmodelsadded_singleexonremoved_withUTR_withStringtie_Final.gff
       mv transcripts.fasta Blasia_Final_transcripts.fasta
       mv cds.fasta Blsia_Final_cds.fasta
       mv protein.fasta Blasia_Final_protein.fasta

### delete the liftoff version genes with mrna and cds  that comes with valid_ORFs=0/valid_ORF=False

### reextract the cds, protein and transciptome data and submit them to the server.

### delete all the 329 mRNA entries in the gff file, 317 genes that invalid ORF. actually when valid_ORFs=3, there can be one mRNA valid_ORF=false, cuza maybe it has 4 mRNA in total. 

       awk 'BEGIN{FS=","; OFS="\t"} {print $1, $2, $3, $4, $5, $6, $7, $8, $9}' BlasiaPusilla_Final_annotations_2.csv > BlasiaPusilla_Final_annotations_3.gff

       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries BlasiaPusilla_Final_annotations_3.gff > BlasiaPusilla_Final_annotations_4.gff

       gt loccheck BlasiaPusilla_Final_annotations_4.gff 

### successfully

       gffread -w transcripts.fasta -x cds.fasta -y protein.fasta -g assembly.fasta.PolcaCorrected.FINAL.fasta BlasiaPusilla_Final_annotations_4.gff 

       mv Blasia_Final_annotation_4.gff Blasia_Final_annotation_validORF.gff
       mv transcripts.fasta Blasia_Final_transcripts_validORF.fasta
       mv cds.fasta Blsia_Final_cds_validORF.fasta
       mv protein.fasta Blasia_Final_protein_validORF.fasta



# to prepare some additional genes for the sex genes from augustus cgp mode for helsinki genome. 

## in total is 12 genes .

## modify the gff file first before running:

       gt loccheck test.gff

## there are some tabs are wrong in the file, cuz i did it manually.

## search for the tabs in vi:

        /\t
       :set list 
       :set listchars=tab:>-

       cat BlasiaPusilla_Final_annotations_validORF.gff test.gff > BlasiaPusilla_Final_annotations_validORF_sexGenesAppended.gff

       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries BlasiaPusilla_Final_annotations_validORF_sexGenesAppended.gff > BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed.gff gt loccheck BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed.gff

        gt loccheck BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed.gff

# now there is 17394 genes, the oldder version has 17382 genes. 

       gffread -w sex_genes_appended_transcripts.fasta -x sex_gene_appended_cds.fasta -y sex_genes_appended_protein.fasta -g assembly.fasta.PolcaCorrected.FINAL.fasta BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed.gff

# to prepare some additional genes for the sex genes from augustus cgp mode for feywei genome. 

## do the same for feywei genome.

       gffread -w feywei_sex_genes_appended_transcripts.fasta -x feywei_sex_gene_appended_cds.fasta -y feywei_sex_genes_appended_protein.fasta -g ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta feywei_9scaffolds_tsebra_result_renamed_genometools.gff

# for feywei annotation:

       sed -i "s/  / /g" xxx.gff
       sed -i "s/  / /g" xxx.gff
       sed -i "s/  / /g" xxx.gff
       sed -i "s/  / /g" xxx.gff
       sed -i "s/  / /g" xxx.gff

## convert many spaces into 1 space. 

### convert >--- (arbitrary number of - ) or > into a tab
### special case: >---->---  



       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries alex_feywei_sex_genes_appendix.gff > feywei_sex_genes_appendix_2.gff

       gt loccheck feywei_sex_genes_appendix_2.gff  

       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries feywei_9scaffolds_tsebra_result_sex_genes_appended.gff  > feywei_9scaffolds_tsebra_result_sex_genes_appended_renamed.gff 

       gt loccheck feywei_9scaffolds_tsebra_result_sex_genes_appended_renamed.gff 




# do that for helsinki 9 scaffolds:


       gt gff3 -sort -tidy -checkids -addids -fixregionboundaries BlasiaPusilla_Final_annotations_validORD_sexGeneSppended_9scaffolds.gff > BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff 

       gt loccheck BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff

       samtools faidx assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta

        /home/ubuntu/USERS/Yuling/Blasia_Genome/Blasia_genome_annotation/Polca_9scaffolds_braker/gffread-0.12.7/gffread/./gffread -w helsinki_9scaffolds_sex_genes_appended_transcripts.fasta -x helsinki_9scaffolds_sex_gene_appended_cds.fasta -y helsinki_9scaffolds_sex_genes_appended_protein.fasta -g assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff --table @geneid
        



# Summarize the RepeatMasker results for the repeat elements statistics:
## helsinki genome:

/home/ubuntu/miniconda3/pkgs/repeatmasker-4.1.1-pl526_1/share/RepeatMasker/util/buildSummary.pl assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.out  > repeatmasker_summary.txt



~/USERS/Yuling/D/comparative_genomics/Liuyang/EDTA_ragtag_genome/EDTA/bin/buildSummary.pl MpTak_v6.1r2.genome_9chroms.fasta.out  > repeatmasker_summary.txt




Repeat Classes
==============
Total Sequences: 9
Total Length: 377871562 bp
Class                  Count        bpMasked    %masked
=====                  =====        ========     =======
Unspecified            461726       230009323    60.87%
                      ---------------------------------
    total interspersed 461726       230009323    60.87%

Simple_repeat          46481        2129708      0.56%
---------------------------------------------------------
Total                  508207       232139031    61.43%


sudo apt-get update
sudo apt-get install libtext-soundex-perl

## cornell genome:

/home/ubuntu/miniconda3/pkgs/repeatmasker-4.1.1-pl526_1/share/RepeatMasker/util/buildSummary.pl ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta.out   > repeatmasker_summary.txt


Repeat Classes
==============
Total Sequences: 9
Total Length: 343046717 bp
Class                  Count        bpMasked    %masked
=====                  =====        ========     =======
Unspecified            429704       199190370    58.07%
                      ---------------------------------
    total interspersed 429704       199190370    58.07%

Simple_repeat          44452        1945005      0.57%
---------------------------------------------------------
Total                  474156       201135375    58.63%



## caculate the TE elements statistics:

B. pusilla:

Repeat Classes 

============== 

Total Sequences: 9 

Total Length: 376971965 bp 

Class                  Count        bpMasked    %masked 

=====                  =====        ========     ======= 

LTR                    --           --           -- 

    Copia              36453        22952008     6.09% 

    Gypsy              47440        50938394     13.51% 

    unknown            2637         1156626      0.31% 

TIR                    --           --           -- 

    CACTA              19327        7926869      2.10% 

    Mutator            33925        12135251     3.22% 

    PIF_Harbinger      2951         1299691      0.34% 

    Tc1_Mariner        2163         619903       0.16% 

    hAT                12758        4501275      1.19% 

nonTIR                 --           --           -- 

    helitron           150622       91034982     24.15% 

                      --------------------------------- 

    total interspersed 308276       192564999    51.08% 

 

--------------------------------------------------------- 

Total                  308276       192564999    51.08% 

m.polymorpha :


Repeat Classes 

============== 

Total Sequences: 10 

Total Length: 219839509 bp 

Class                  Count        bpMasked    %masked 

=====                  =====        ========     ======= 

LINE                   --           --           -- 

    unknown            103          107114       0.05% 

LTR                    --           --           -- 

    Copia              6619         4500868      2.05% 

    Gypsy              19420        13831225     6.29% 

    unknown            6827         3635229      1.65% 

TIR                    --           --           -- 

    CACTA              3580         1983611      0.90% 

    Mutator            13826        7168347      3.26% 

    PIF_Harbinger      88           70567        0.03% 

    Tc1_Mariner        574          253196       0.12% 

    hAT                2113         740370       0.34% 

nonTIR                 --           --           -- 

    helitron           6552         3245282      1.48% 

repeat_region          13982        4849913      2.21% 

                      --------------------------------- 

    total interspersed 73684        40385722     18.37% 

 

--------------------------------------------------------- 

Total                  73684        40385722     18.37% 

 

 # generate some plots for these tables :

 import pandas as pd
import matplotlib.pyplot as plt

# Define the data
data = {
    "Transposable Element Class": [
        "LTR - Copia", "LTR - Gypsy", "LTR - Unknown", 
        "TIR - CACAT", "TIR - Mutator", "TIR - PIF_Harbinger", 
        "TIR - Tc1_Mariner", "TIR - hAT", "nonTIR - Helitron"
    ],
    "Masked (%)": [6.09, 13.51, 0.31, 2.10, 3.22, 0.34, 0.16, 1.19, 24.15]
}

# Create a DataFrame
df = pd.DataFrame(data)

# Plotting Masked (%) as a bar chart
plt.figure(figsize=(10, 6))
plt.bar(df["Transposable Element Class"], df["Masked (%)"], color="skyblue")
plt.xticks(rotation=45, ha="right")
plt.ylabel("Masked (%)")
plt.title("Masked Percentage of Transposable Element Classes")
plt.tight_layout()

# Save the plot as an image file
plt.savefig("Transposable_Element_Masked_Percentage.png")

# Show the plot
plt.show()

# COMPARE TWO SPECIES 
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import matplotlib.cm as cm

# Define the data for Blasia and Marchantia
data = {
    "Transposable Element Class": [
        "LINE", "LTR - Copia", "LTR - Gypsy", "LTR - Unknown", 
        "TIR - CACAT", "TIR - Mutator", "TIR - PIF_Harbinger", 
        "TIR - Tc1_Mariner", "TIR - hAT", "nonTIR - Helitron", "Repeat Region"
    ],
    "Blasia Masked (%)": [0.00, 6.09, 13.51, 0.31, 2.10, 3.22, 0.34, 0.16, 1.19, 24.15, 0.00],  # Added placeholder for missing data
    "Marchantia Masked (%)": [0.05, 2.05, 6.29, 1.65, 0.90, 3.26, 0.03, 0.12, 0.34, 1.48, 2.21]
}

# Create a DataFrame
df = pd.DataFrame(data)

# Set bar width and positions
bar_width = 0.35
index = np.arange(len(df))

# Generate colors from the viridis colormap
viridis = cm.get_cmap('viridis', len(df))
colors = [viridis(i) for i in range(len(df))]

# Plotting the grouped bar chart
plt.figure(figsize=(14, 8))
plt.bar(index, df["Blasia Masked (%)"], bar_width, label="Blasia", color=colors, alpha=0.8)
plt.bar(index + bar_width, df["Marchantia Masked (%)"], bar_width, label="Marchantia", color='darkblue', alpha=0.7)
plt.xticks(index + bar_width / 2, df["Transposable Element Class"], rotation=45, ha="right")
plt.ylabel("Masked (%)")
plt.title("Comparison of Masked Percentage of Transposable Element Classes in Blasia and Marchantia")
plt.legend()
plt.tight_layout()

# Save the plot as an image file
plt.savefig("Blasia_vs_Marchantia_TE_Comparison.png")

# Show the plot
plt.show()







 
