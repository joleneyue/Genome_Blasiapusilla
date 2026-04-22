## 1. genome scaffolding using 3-DNA for the nextpolish polished genome assembly:

nextpolish version running 3d-dna 

index the genome file

      bwa index genome.nextpolish.fasta

      cd ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/misc 

      python generate_site_positions.py none ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/genome.nextpolish.fasta  ~/storage2/Yuling/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/
  
      awk 'BEGIN{OFS="\t"}{print $1, $NF}' genome.nextpolish.fasta_none.txt > genome.nextpolish.fasta.chrom.sizes

errors:<restriction enzyme> must be one of ['HindIII', 'DpnII', 'MboI', 'Sau3AI', 'Arima'] Usage: generate_site_positions.py <restriction enzyme> <genome> [location]

go to the directory:  juicer/CPU, mkdir juicer_output, cd juicer_output, mkdir fastq, mkdir splits, and copy the library into the fastqt and splits folder.

      bash juicer.sh -D ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/CPU  -t 16 -s none -z ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/genome.nextpolish.fasta -y none -p ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/misc/genome.nextpolish.fasta.chrom.sizes -d Nextpolish_version_pool_library_output -S early --assembly

run 3d-dna

      bash run-asm-pipeline.sh ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/blasia_2020_hybrid_polishing.fasta ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/CPU/flye_uncleaned_assembly_pool_library_output/aligned/merged_nodups.txt

## 2. running 3D-DNA for the polca polished version genome assembly:

 polca polished version running 3d-dna 
  
index the genome file, sorted the genome scaffolds by order. 
  
       bwa index assembly.fasta.PolcaCorrected_sorted.fa 

       cd ~/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/misc 

       python3 generate_site_positions.py none /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/assembly.fasta.PolcaCorrected_sorted.fa   /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference
  
      awk 'BEGIN{OFS="\t"}{print $1, $NF}' assembly.fasta.PolcaCorrected.fa_none.txt > assembly.fasta.PolcaCorrected.fa_sorted.chrom.sizes

errors:<restriction enzyme> must be one of ['HindIII', 'DpnII', 'MboI', 'Sau3AI', 'Arima'] Usage: generate_site_positions.py <restriction enzyme> <genome> [location]

go to the directory:  juicer/CPU, mkdir juicer_output, cd juicer_output, mkdir fastq, mkdir splits, and copy the library into the fastq and splits folder.
# first run juicer :

       bash juicer.sh -D /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/CPU  -t 16 -s none -z /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/assembly.fasta.PolcaCorrected_sorted.fa -y none -p /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/misc/assembly.fasta.PolcaCorrected.fa_sorted.chrom.sizes -d Polca_sorted_version_pool_library_output -S early --assembly

# then run 3d-dna

      bash run-asm-pipeline.sh ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/assembly.fasta.PolcaCorrected.fa ~/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/CPU/Polca_version_pool_library_output/aligned/merged_nodups.txt

       bash run-asm-pipeline.sh /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/reference/assembly.fasta.PolcaCorrected_sorted.fa /home/ubuntu/USERS/Yuling/Yuling_20June2022/Blasia_OmniClibraries_seq_Novogene30May2022/juicer/CPU/Polca_sorted_version_pool_library_output/aligned/merged_nodups.txt



      # here i should have the final assembly and the hic map. 




run the busco and quast:

       ~/storage2/Yuling/executables/quast/./quast.py  assembly.fasta.PolcaCorrected.FINAL.fasta  -o quast_output

      busco -i assembly.fasta.PolcaCorrected.FINAL.fasta -o viridiplantae_output -m genome -l viridiplantae

      busco -i assembly.fasta.PolcaCorrected.FINAL.fasta -o embyophyta_output -m genome -l embryophyta

 
      --------------------------------------------------
      |Results from dataset embryophyta_odb10           |
      --------------------------------------------------
      |C:79.0%[S:77.0%,D:2.0%],F:5.3%,M:15.7%,n:1614    |
      |1275   Complete BUSCOs (C)                       |
      |1242   Complete and single-copy BUSCOs (S)       |
      |33     Complete and duplicated BUSCOs (D)        |
      |85     Fragmented BUSCOs (F)                     |
      |254    Missing BUSCOs (M)                        |
      |1614   Total BUSCO groups searched               |
      --------------------------------------------------
  
  
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



  
