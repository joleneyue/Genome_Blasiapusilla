
# convert gff file into gbff :

     gffread -E BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff  -g assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta  -o output.gbff


1. Identification of syntenic regions using MCScanX

## MCScanx publication: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3326336/
## MCScanx git: https://github.com/wyp1125/MCScanX

## input data:

## inter- and intra-species blastp output, concatenated in one file

    blastp command recommended on git: blastall  -i  query_file  -d database -p blastp -e 1e-10 -b 5 -v 5 -m 8 -o xyz.blast

### following combinations necessary: Pp vs Fh; Fh vs Pp; Pp vs Pp; Fh vs Fh

## simplified .gff file containing only gene features

### 4 tab-separated fields: ChrID; start; end; gene name
## again, Fh and Pp concatenated
## chromosome ID: two letters for species name followed by chromosome number (e.g. Fh3)

## notes:

## including only chromosome-scale scaffolds (9 ) and genes annotated on those scaffolds to make the plots better readable

## non-chromosome scaffolds are supposed to contain very few genes anyway (check numbers after filtering non-chromosomal genes out; 

### marchanthia data retrived from here:https://marchantia.info/download/MpTak_v6.1r2/

## blasia  annotation used for analysis: BRAKER all evidence annotation + gene models added from illumina feywei liftoff and stringtie symbiosis gens and comparative genomics sex linked genes. 


# choose the primary transcript for marchanthia

    awk '/\.1$/{p=1; header=$0; next} p{print header; print; p=0}' MpTak_v6.1r2.protein.fasta > MpTak_v6.1r2.protein_primaryTranscript.fasta

    grep ">" assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta| wc -l

     9
     grep ">" MpTak_v6.1r2.genome.fasta| wc -l

    39
     grep ">" MpTak_v6.1r2.genome.fasta| head -39

     >chr1
    >chr2
    >chr3
    >chr4
    >chr5
    >chr6
    >chr7
    >chr8
    >chrU
    >chrV
    >unplaced-scaffold_010
    >unplaced-scaffold_056
    >unplaced-scaffold_078
    >unplaced-scaffold_086
    >unplaced-scaffold_098
    >unplaced-scaffold_131
    >unplaced-scaffold_145
    >unplaced-scaffold_162
    >unplaced-scaffold_190
    >unplaced-scaffold_202
    >unplaced-scaffold_221
    >unplaced-scaffold_250
    >unplaced-scaffold_257
    >unplaced-scaffold_267
    >unplaced-scaffold_279
    >unplaced-scaffold_281
    >unplaced-scaffold_315
    >unplaced-scaffold_349
    >unplaced-scaffold_362
    >unplaced-scaffold_406
    >unplaced-scaffold_431
    >unplaced-scaffold_432
    >unplaced-scaffold_433
    >unplaced-scaffold_435
    >unplaced-scaffold_436
    >unplaced-scaffold_439
    >unplaced-scaffold_440
    >unplaced-scaffold_441
    >unplaced-scaffold_445

    grep ">"  MpTak_v6.1r2.protein.fasta| wc -l
    22674

    grep ">"  MpTak_v6.1r2.protein.fasta| tail
    >Mpzg01330.1
    >Mpzg01340.1
    >Mpzg01360.1
    >Mpzg01370.1
    >Mpzg01380.1
    >Mpzg01390.1
    >Mpzg01400.1
    >Mpzg01440.1
    >Mpzg01450.1
    >Mpzg01410.1


    grep ">" Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts.fasta| wc -l
    17046

    grep ">" Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts.fasta| head
    >mRNA1 gene=gene1
    >mRNA2 gene=gene2
    >mRNA3 gene=gene3
    >mRNA5 gene=gene4
    >mRNA6 gene=gene5
    >mRNA7 gene=gene6
    >mRNA8 gene=gene7
    >mRNA9 gene=gene8
    >mRNA10 gene=gene9
    >mRNA12 gene=gene10

    grep -v "mRNA" BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff > BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_simple.gff 

    grep "feature\tgene" MpTak_v6.1r2.gff > MpTak_v6.1r2_simple.gff

    grep ">" assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta| head 
    >HiC_scaffold_1
    >HiC_scaffold_2
    >HiC_scaffold_3
    >HiC_scaffold_4
    >HiC_scaffold_5
    >HiC_scaffold_6
    >HiC_scaffold_7
    >HiC_scaffold_8
     >HiC_scaffold_9


    touch Blasia_chromosomal_only.gff

    for i in {1..9};do 
     grep -w "HiC_scaffold_${i}" BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_simple.gff  >> Blasia_chromosomal_only.gff
     done

     for i in {1..25};do 
     grep -w "scaffold_${i}" BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_reordered_simple.gff  >> Blasia_chromosomal_only.gff
     done



    touch marchanthia_chromosomal_only.gff

    grep "chr" MpTak_v6.1r2_simple.gff > marchanthia_chromosomal_only.gff 
    grep "chr" marchanthia_chromosomal_only.gff | cut -f1 | uniq| head
    chr1
    chr2
    chr3
    chr4
    chr5
    chr6
    chr7
    chr8
    chrU
    chrV

    grep "chr" marchanthia_chromosomal_only.gff | cut -f1 | wc -l
    19027

    grep "HiC" Blasia_chromosomal_only.gff | cut -f1| wc -l
    17055

    grep "gene" marchanthia_chromosomal_only.gff | cut -f9| cut -f1 -d ";"| sed 's/ID=//g'| head
    Mp1g00005a
    Mp1g00015a
    Mp1g00015b
    Mp1g00015c
    Mp1g00015d
    Mp1g00025a
    Mp1g00025b
    Mp1g00025c
    Mp1g00025d
    Mp1g00025e

    grep "gene" marchanthia_chromosomal_only.gff | cut -f9| cut -f1 -d ";"| sed 's/ID=//g' > marchanthia_chrmosomal_only_genes.list

    grep "Mp" marchanthia_chrmosomal_only_genes.list | wc -l
    19026

    grep ">"  MpTak_v6.1r2.protein.fasta| wc -l
    22674

    grep ">" MpTak_v6.1r2.protein_primaryTranscript.fasta| wc -l
    18038

    cut -f1 -d " " MpTak_v6.1r2.protein_primaryTranscript.fasta| head
    >Mp1g00070.1


    sed 's/\.1//g' MpTak_v6.1r2.protein_primaryTranscript.fasta| head
    >Mp1g00070


    sed 's/\.1//g' MpTak_v6.1r2.protein_primaryTranscript.fasta > MpTak_v6.1r2.protein_primaryTranscript_simpleheader.fa

    seqtk subseq MpTak_v6.1r2.protein_primaryTranscript_simpleheader.fa marchanthia_chrmosomal_only_genes.list > marchanthia_chrmosomal_only.prptide.fa

    grep ">" marchanthia_chrmosomal_only.prptide.fa | wc -l
    17998

    grep "gene" Blasia_chromosomal_only.gff| cut -f9 | sed 's/ID=//g'| head
    gene1
    gene2
    gene3
    gene4
    gene5
    gene6
    gene7
    gene8
    gene9
    gene10

    grep "gene" Blasia_chromosomal_only.gff| cut -f9 | sed 's/ID=//g' > blasia_chrmosomal_only_genes.list
    grep "gene" blasia_chrmosomal_only_genes.list | wc -l
    17046

    sed "s/>.*=/>/"  Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts.fasta | head
    >gene1

    sed "s/>.*=/>/"  Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts.fasta > Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts_simpleheader.fasta 

    seqtk subseq Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts_simpleheader.fasta blasia_chrmosomal_only_genes.list > blasia_chromosomal_only_peptide.fa 


    grep "gene" Blasia_chromosomal_only.gff |awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| head
    HiC_scaffold_1	ID=gene1	2124	10618
    HiC_scaffold_1	ID=gene2	2124	10618
    HiC_scaffold_1	ID=gene3	2124	15625
    HiC_scaffold_1	ID=gene4	117840	122723
    HiC_scaffold_1	ID=gene5	123061	129679
    HiC_scaffold_1	ID=gene6	152387	158671
    HiC_scaffold_1	ID=gene7	191043	195716
    HiC_scaffold_1	ID=gene8	212888	214071
    HiC_scaffold_1	ID=gene9	222337	226568
    HiC_scaffold_1	ID=gene10	230963	239715

    grep "gene" Blasia_chromosomal_only.gff |awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| sed 's/HiC_scaffold_/bp/g'| head 
    bp1	ID=gene1	2124	10618
    bp1	ID=gene2	2124	10618
    bp1	ID=gene3	2124	15625
    bp1	ID=gene4	117840	122723
    bp1	ID=gene5	123061	129679
    bp1	ID=gene6	152387	158671
    bp1	ID=gene7	191043	195716
    bp1	ID=gene8	212888	214071
    bp1	ID=gene9	222337	226568
    bp1	ID=gene10	230963	239715

    grep "gene" Blasia_chromosomal_only.gff |awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| sed 's/HiC_scaffold_/bp/g'| sed 's/ID=//g'| head
    bp1	gene1	2124	10618
    bp1	gene2	2124	10618
    bp1	gene3	2124	15625
    bp1	gene4	117840	122723
    bp1	gene5	123061	129679
    bp1	gene6	152387	158671
    bp1	gene7	191043	195716
    bp1	gene8	212888	214071
    bp1	gene9	222337	226568
    bp1	gene10	230963	239715

    grep "gene" Blasia_chromosomal_only.gff |awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| sed 's/HiC_scaffold_/bp/g'| sed 's/ID=//g' > blasia_chrmosomal_only.bed

    grep "gene" marchanthia_chromosomal_only.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| sed 's/chr/mp/g'| head
    mp1	ID=Mp1g00005a;Name=Mp1g00005a;locus_type=rRNA;note=partial	1	1967
    mp1	ID=Mp1g00015a;Name=Mp1g00015a;locus_type=rRNA	3767	3924
    mp1	ID=Mp1g00015b;Name=Mp1g00015b;locus_type=rRNA	5072	6891
    mp1	ID=Mp1g00015c;Name=Mp1g00015c;locus_type=rRNA	13605	13723
    mp1	ID=Mp1g00015d;Name=Mp1g00015d;locus_type=rRNA	15718	17728
    mp1	ID=Mp1g00025a;Name=Mp1g00025a;locus_type=rRNA	19528	19685
    mp1	ID=Mp1g00025b;Name=Mp1g00025b;locus_type=rRNA	20833	22652
    mp1	ID=Mp1g00025c;Name=Mp1g00025c;locus_type=rRNA	24398	24516
    mp1	ID=Mp1g00025d;Name=Mp1g00025d;locus_type=rRNA	31237	33056
    mp1	ID=Mp1g00025e;Name=Mp1g00025e;locus_type=rRNA	34204	34361

    cat blasia_chrmosomal_only.bed marchanthia_chromosomal_only.bed > mp_bp.gff

    makeblastdb -in blasia_chromosomal_only_peptide.fa -out blasia_pep -dbtype prot
    makeblastdb -in marchanthia_chrmosomal_only.prptide.fa -out marchanthia_pep -dbtype prot


makeblastdb -in  blasia_liuyang_chromosomela_only.peptide.fa -out blasia_liuyang_pep -dbtype prot

makeblastdb -in  blasia_chromosomal_only_only_peptide.fa -out blasia_pepe -dbtype prot

# marchanthia vs marchanthia

    blastp -query marchanthia_chrmosomal_only.prptide.fa -db marchanthia_pep -evalue 1e-10 -outfmt "6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore" -out mp_vs_mp.blast 

# blasia vs blasia

    blastp -query blasia_chromosomal_only_peptide.fa -db blasia_pep -evalue 1e-10 -outfmt "6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore" -out bp_vs_bp.blast 

# marchanthia vs blasia

    blastp -query marchanthia_chrmosomal_only.prptide.fa -db blasia_pep -evalue 1e-10 -outfmt "6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore" -out mp_vs_bp.blast 

# blasia vs marchanthia

    blastp -query blasia_chromosomal_only_peptide.fa -db marchanthia_pep -evalue 1e-10 -outfmt "6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore" -out bp_vs_mp.blast 


# concatenate results

    cat mp_vs_mp.blast bp_vs_bp.blast mp_vs_bp.blast bp_vs_mp.blast > mp_bp.blast

## 1.3. Prepare input bed files from gff annotations

# combine into single file

    cat Blasia_chromosomal_only.bed Marchanthi_chromosomal_only.bed > mm_bb.gff

    cp mp_bp.blast MCScanX/mp_bp/
    cp mp_bp.gff MCScanX/mp_bp/
    cd MCScanX/mp_bp/

    ./MCScanX mp_bp/mp_bp
    tail -100 mp_bp.gff 

    awk 'BEGIN{FS=";"} {print $1}' marchanthia_chromosomal_only.gff | head
    awk 'BEGIN{FS=";"} {print $1}' marchanthia_chromosomal_only.gff | tail
    awk 'BEGIN{FS=";"} {print $1}' marchanthia_chromosomal_only.gff | head -100

    awk 'BEGIN{FS=";"} {print $1}' marchanthia_chromosomal_only.gff > marchanthia_chromosomal_only_fixed.gff
    grep "gene" marchanthia_chromosomal_only_fixed.gff | head
    grep "gene" marchanthia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}' | head
    grep "gene" marchanthia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}' | sed 's/chr/pp/g'| head
    grep "gene" marchanthia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}' | head
    grep "gene" marchanthia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}' | sed 's/chr/mp/g'|head
    grep "gene" marchanthia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}' | sed 's/chr/mp/g'| sed 's/ID=//g' | head
    grep "gene" marchanthia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}' | sed 's/chr/mp/g'| sed 's/ID=//g' > marchanthia_chromosomal_only.bed 

    awk 'BEGIN{FS=";"} {print $1}' BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_simple.gff | head
    awk 'BEGIN{FS=";"} {print $1}' BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_simple.gff | head -100
    awk 'BEGIN{FS=";"} {print $1}' BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_simple.gff > BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_simple_fixed.gff
    awk 'BEGIN{FS=";"} {print $1}' Blasia_chromosomal_only.gff|head
    awk 'BEGIN{FS=";"} {print $1}' Blasia_chromosomal_only.gff >Blasia_chromosomal_only_fixed.gff 
    grep "gene" Blasia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| head
    grep "gene" Blasia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| sed 's/HiC_scaffold_/bp/g'| sed 's/ID=//g' | head
     grep "gene" Blasia_chromosomal_only_fixed.gff | awk '{ print $1 "\t" $9 "\t" $4 "\t" $5}'| sed 's/HiC_scaffold_/bp/g'| sed 's/ID=//g' > blasia_chrmosomal_only.bed 

    grep "gene" Blasia_chromosomal_only_fixed.gff | head
    grep "gene" Blasia_chromosomal_only_fixed.gff | cut -f 9 -d $'\t'| head
    grep "gene" Blasia_chromosomal_only_fixed.gff | cut -f 9 -d $'\t'| cut -f 1 -d ";" | sed 's/ID=//g'| head
    grep "gene" Blasia_chromosomal_only_fixed.gff | cut -f 9 -d $'\t'| cut -f 1 -d ";" | sed 's/ID=//g' > blasia_chrmosomal_only_genes.list 
    grep "gene" blasia_chrmosomal_only_genes.list | wc -l
    grep ">" Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts_simpleheader.fasta| head -100

    seqtk subseq Helsinki_9scaffolds_sexgenes_Appended_protein_primaryTranscripts_simpleheader.fasta blasia_chrmosomal_only_genes. list > blasia_chromosomal_only_peptide.fa

    grep "gene" marchanthia_chromosomal_only_fixed.gff | cut -f 9 -d $'\t'| cut -f 1 -d ";" | sed 's/ID=//g'| head
    Mp1g00005a
    Mp1g00015a
    Mp1g00015b
    Mp1g00015c
    Mp1g00015d
    Mp1g00025a
    Mp1g00025b
    Mp1g00025c
    Mp1g00025d
    Mp1g00025e

     grep "gene" marchanthia_chromosomal_only_fixed.gff | cut -f 9 -d $'\t'| cut -f 1 -d ";" | sed 's/ID=//g' > marchanthia_chrmosomal_only_genes.list

    seqtk subseq MpTak_v6.1r2.protein_primaryTranscript_simpleheader.fa marchanthia_chrmosomal_only_genes.list > marchanthia_chrmosomal_only.prptide.fa 

    grep ">" marchanthia_chrmosomal_only.prptide.fa | wc -l
    17998

    grep ">" MpTak_v6.1r2.protein_primaryTranscript_simpleheader.fa| wc -l
    18038

# re-run the blastp

## 1.4. Run MCScanX 

## install MCScanX

    git clone https://github.com/wyp1125/MCScanX.git

    https://github.com/wyp1125/MCScanX.git

    cd MCScanX/
    make

## run MCScanX

    ./MCScanX mm_bb/mm_bb

    # re-run the analysis with relaxed parameters:

    ./MCScanX -s 6 -m 80 -a mp_bp_new_parameter/mp_bp

     parameter distance = 80, gene number = 6.  and –full mode

    ./MCScanX -s 5 -m 60 -a mp_bp_intermediate_parameter/mp_bp

sed -i '' \
  -e 's/\bbp1\b/Bp_6/g' \
  -e 's/\bbp2\b/Bp_3/g' \
  -e 's/\bbp3\b/Bp_1/g' \
  -e 's/\bbp4\b/Bp_5/g' \
  -e 's/\bbp5\b/Bp_4/g' \
  -e 's/\bbp6\b/Bp_2/g' \
  -e 's/\bbp7\b/Bp_8/g' \
  -e 's/\bbp8\b/Bp_7/g' \
  -e 's/\bbp9\b/Bp_9/g' \
mp_bp.collinearity

sed -i '' \
  -e 's/\bmp1\b/Mp_1/g' \
  -e 's/\bmp2\b/Mp_2/g' \
  -e 's/\bmp3\b/Mp_3/g' \
  -e 's/\bmp4\b/Mp_4/g' \
  -e 's/\bmp5\b/Mp_5/g' \
  -e 's/\bmp6\b/Mp_6/g' \
  -e 's/\bmp7\b/Mp_7/g' \
  -e 's/\bmp8\b/Mp_8/g' \
  -e 's/\bmpU\b/Mp_U/g' \
  -e 's/\bmpV\b/Mp_V/g' \
mp_bp.collinearity


# get the result file mp_bp.collinearity, upload to the graphfical interface to see the result: synviso

## generate plots
## corresponding control files (.ctl) have to be modified for each plot
## plot commands have to be run from inside the downstream analysis dir of MCScanX dir

# dot-plot

# go to the directory MCScan

    java dot_plotter -g ../mp_bp_new_parameter/mp_bp.gff -s ../mp_bp_new_parameter/mp_bp.collinearity -c ../dotplot/dot.ctl -o dot_plot_new/mp_bp_dotplot.png

# install circos:

    tar xvfz circos-0.69-9.tgz

    cd circos-0.69-9/

    cd example/

    ./run

    cd tutorials/2/2
# now try tutorial 2.2

      sudo cpan install Math::Round
      sudo cpan Math::Bezier
      sudo cpan Math::VecStat
      sudo cpan Regexp::Common
      sudo cpan Set::IntSpan
      sudo cpan Statistics::Basic
      sudo cpan GD::Polyline
      sudo apt-get install libgd-dev
      sudo cpan Config::General
      sudo cpan Font::TTF::Font
      sudo cpan Text::Format
      sudo cpan SVG
      sudo cpan Params::Validate
      sudo cpan List::MoreUtils
      sudo cpan Clone
      sudo cpan GD

    export PATH=~/USERS/Yuling/D/comparative_genomics/circos_new/bin:$PATH

    . ~/.bashrc

# convert gff file to karyotype file:

    cut -f1 BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff| sort | uniq > chromosomes.txt

# install mmucer:
    ./configure --prefix=/home/ubuntu/Blasia_Genome/Blasia_genome_annotation/Blasia_genome_TE_annotation/EDTA/genome_9scaffolds/mmuer/
    make
    make install
    ./nucmer --maxmatch -l 100 -c 500 assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta --prefix helsinkiref_feyweiquery 
    gzip helsinkiref_feyweiquery.delta
    ./mummerplot --png -l helsinkiref_feyweiquery.delta
    ./delta-filter -m helsinkiref_feyweiquery.delta > helsinkiref_feyweiquery.delta.m
    ./mummerplot --png helsinkiref_feyweiquery.delta.m -R assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta -Q ISAR.hipmer.final_assembly.FINAL_9scaffolds.fasta --prefix=helsinki_vs_feywei -large -layout

# make comparison for female and male blasia 
conda activate EDTA
seqtk seq -l 0 assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta > ref_clean.fa
seqtk seq -l 0 ragtag.scaffold.fasta > query_clean.fa

# too much repeats in the unplaced scaffolds, run only with first 9 plus the female scaffold:

seqkit grep -r -p Bp_1_RagTag -p Bp_2_RagTag -p Bp_3_RagTag -p Bp_4_RagTag -p Bp_5_RagTag \
-p Bp_6_RagTag -p Bp_7_RagTag -p Bp_8_RagTag -p Bp_9_RagTag -p scaffold_11 query_clean.fa > query_filtered.fa
 export LD_LIBRARY_PATH=$PWD/.libs:$LD_LIBRARY_PATH
nucmer --maxmatch -l 100 -c 500 ref_clean.fa query_filtered.fa --prefix helsinkiref_Chinesequery

# need to download the gnunplor over 5.2 version with conda :
mamba create -n gnuplot_env -c conda-forge gnuplot=5.4.3
conda activate gnuplot_env
 gnuplot --version
    ./delta-filter -m helsinkiref_Chinesequery.delta > helsinkiref_Chinesequery.delta.m
    gnuplot out.gp
    echo 'export LD_LIBRARY_PATH=/home/ubuntu/USERS/Yuling/D/comparative_genomics/mmuer/mummer-4.0.0rc1/.libs:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
    ./mummerplot --png helsinkiref_Chinesequery.delta.m -R assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta -Q ragtag.scaffold.fasta --prefix=helsinki_vs_Chinese -large -layout
    gnuplot helsinki_vs_Chinese.gp
# run quast 
    /home/ubuntu/USERS/Yuling/executables/quast/quast.py -r assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta -o quast_output ragtag.scaffold.fasta

# follow alexander's gitlab for circos:
# extract scaffold lengths, change scaffold names and extract chromosome scale scaffolds

    bioawk -c fastx '{ print $name, length($seq) }' < assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta| head -n 9 > chr.blasia.helsinki.txt

## add additional columns, describing coloring, ordering, etc. to get valid karyotype file
    karyotype <- read.table("chr.blasia.helsinki.txt")
    names(karyotype) <- c("chrID","stop")
    karyotype <- karyotype %>%
    mutate(type = "chr",
         parent = "-",
         start = 0,
         order = c(1,2,3,4,5,6,7,8,9)) %>%
  arrange(order) %>%
  mutate(label = paste("BP",order,sep = ""),
         color = paste("chr",order,sep = "")) %>%
  select(type, parent, chrID, label, start, stop, color)

write.table(karyotype, "karyotype.blasia.helsinki.txt", quote = F, row.names = F, col.names = F, sep = "\t")

## Collinearity and synteny between blasia and marchanthia genomes

## add information about self-synteny

## syntenic regions determined with MCScanX, using genes as anchors
## input file: mm_bb.collinearity output file from MCScanX pipeline

# extract collinearity information of Zurich accession
    grep -v "mm" mm_bb.collinearity | grep -v "Fh_uc" | grep -v "#" > zh.collinearity

# extract annotation of Zurich accession
    grep -v "uc" uc_zh.gff > zh.gff

##################

# Converting MCScanX collinearity output to circos compatible input

# input:

# - uc_zh.collinearity output file from MCScanX pipeline, as described in collinearity_analysis/README.md

# - uc_zh.gff input file from MCScanX pipeline to determine chromosomal positions

##################

    library(tidyverse)
    library(GenomicRanges)

# add function for conditional mutation
    mutate_cond <- function(.data, condition, ..., envir = parent.frame()) {
    condition <- eval(substitute(condition), .data, envir)
    .data[condition, ] <- .data[condition, ] %>% mutate(...)
    .data
    }

# read collinearity file
    col_mc <- read.table("zh.collinearity", sep = "\t")
    names(col_mc) <- c("ID","group1","group2","eval")

# remove whitespace from col1
    col_mc$ID <- gsub(" ","",col_mc$ID)

# separate collinear block IDs from gene numbering
    col_mc <- separate(col_mc, ID, c("blockID","gene_nr"), sep = "-")

# read annotation file
    anno <- read.table("zh.gff", sep = "\t")
    names(anno) <- c("chr","geneID","start","stop")

# add position information to collinearity file

    col <- left_join(col_mc,anno,by=c("group1" = "geneID")) %>%
    select(blockID,gene_nr,group1,chr,start,stop,group2,eval)

    names(col) <- c("blockID","gene_nr","group1","group1_chr","group1_start","group1_stop","group2","eval")

    col <- left_join(col,anno,by=c("group2" = "geneID")) %>%
    select(blockID,gene_nr,group1,group1_chr,group1_start,group1_stop,group2,chr,start,stop,eval)

    names(col) <- c("blockID","gene_nr","group1","group1_chr","group1_start","group1_stop","group2","group2_chr","group2_start","group2_stop","eval")

    col$gene_nr <- gsub(":","",col$gene_nr)

    col <- transform(col, gene_nr = as.integer(gene_nr))

## summarizing by blockID
# add function to get most frequent value per group
    calculate_mode <- function(x) {
    uniqx <- unique(na.omit(x))
    uniqx[which.max(tabulate(match(x, uniqx)))]
    }
# summarize by blockID
    col_blocks <- col %>%
    group_by(blockID) %>%
    summarise(size = max(gene_nr),
            chrA = calculate_mode(group1_chr),
            startA = min(group1_start),
            stopA = max(group1_stop),
            chrB = calculate_mode(group2_chr),
            startB = min(group2_start),
            stopB = max(group2_stop))

# circos compatible format:
    col_circos <- select(col_blocks, -c("blockID","size"))

    write.table(col_circos, "collinear_blocks_zurich.tbl", sep = "\t", quote = F, row.names = F, col.names = F)

# create 100kb/250kb  windows
     bedtools makewindows -g  blasia.chr.sizes -w 100000 > blasia.100kb.windows
     bedtools makewindows -g  blasia.chr.sizes -w 250000 > blasia.250kb.windows

# extract gene positions from gff
     awk '{ print $1 "\t" $4 "\t" $5 "\t" $3}' assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta.mod.EDTA.TEanno.gff3 > blasia.TEs.bed



# count overlapping genes
# do the same for the TEs:

    bedtools intersect  -a blasia.250kb.windows -b blasia.copia.bed -c > blasia.250kb.windows.copiacount
    bedtools intersect  -a blasia.250kb.windows -b blasia.gypsy.bed -c > blasia.250kb.windows.gypsycount

    bedtools intersect  -a blasia.250kb.windows -b blasia.helitron.bed -c > blasia.250kb.windows.helitroncount

    bedtools intersect  -a blasia.250kb.windows -b blasia.TIR.bed -c > blasia.250kb.windows.TIRcount


# befor running circos

conda deactivate

bin/circos -conf ./etc/marchanthia_new_circos.conf

bin/circos -conf ./etc/blasia_new_circos.conf

# using this file for future 


<<include colors_fonts_patterns.conf>>

<<include ideogram.conf>>
<<include ticks.conf>>

<image>
<<include etc/image.conf>>
</image>

karyotype   =/home/ubuntu/USERS/Yuling/D/comparative_genomics/circos_new/Blasia_new_circos/karyotype.blasia.helsinki.txt

chromosomes_units = 1000000
chromosomes = Bp_1;Bp_2;Bp_3;Bp_4;Bp_5;Bp_6;Bp_7;Bp_8;Bp_9
chromosomes_display_default = no

<plots>

type      = line
thickness = 2

<plot>

max_gap = 1u
file  =/home/ubuntu/USERS/Yuling/D/comparative_genomics/circos_new/Blasia_new_circos/blasia.250kb.windows_reordered.genecount
color   = vdgrey
min     = 0
max     = 50
r0      = 0.5r
r1      = 0.8r

fill_color = vdgrey_a3

<backgrounds>
<background>
color     = vvlgreen
y0        = 0.006
</background>
<background>
color     = vvlred
y1        = 0.002
</background>
</backgrounds>

<axes>
<axis>
color     = lgrey_a2
thickness = 1
spacing   = 0.025r
</axis>
</axes>


<rules>

<rule>
condition    = var(value) > 0.006
color        = dgreen
fill_color   = dgreen_a1
</rule>

<rule>
condition    = var(value) < 0.002
color        = dred
fill_color   = dred_a1
</rule>

</rules>

</plot>

# outside the circle, oriented out
<plot>

max_gap = 1u
file    =/home/ubuntu/USERS/Yuling/D/comparative_genomics/circos_new/Blasia_new_circos//blasia.250kb.windows.copiacount
color   = black
min     = 0
max     = 32
r0      = 1.075r
r1      = 1.15r
thickness = 1

fill_color = black_a4

<axes>
<axis>
color     = lgreen
thickness = 2
position  = 0.006
</axis>
<axis>
color     = lred
thickness = 2
position  = 0.002
</axis>
</axes>
                       
                       </plot>

<plot>
z       = 5
max_gap = 1u
file    =/home/ubuntu/USERS/Yuling/D/comparative_genomics/circos_new/Blasia_new_circos/blasia.250kb.windows.gypsyscount
color   = red
fill_color = red_a4
min     = 0
max     = 115
r0      = 1.075r
r1      = 1.15r
</plot>


# same plot, but inside the circle, oriented in
<plot>
max_gap = 1u
file    =/home/ubuntu/USERS/Yuling/D/comparative_genomics/circos_new/Blasia_new_circos/blasia.250kb.windows.TIRcount
color   = black
fill_color = black_a4
min     = 0
max     = 120
r0      = 0.85r
r1      = 0.95r
thickness   = 1
orientation = in

<axes>
<axis>
color     = lgreen
thickness = 2
position  = 0.01
</axis>
<axis>
color     = vlgreen
thickness = 2
position  = 0.008
</axis>
<axis>
color     = vlgreen
thickness = 2
position  = 0.006
</axis>
<axis>
color     = red
thickness = 2
position  = 0.002
</axis>
</axes>

</plot>

<plot>
z       = 5
max_gap = 1u
file    =/home/ubuntu/USERS/Yuling/D/comparative_genomics/circos_new/Blasia_new_circos/blasia.250kb.windows.helitroncount
color   = red
fill_color = red_a4
min     = 0
max     = 70
r0      = 0.85r
r1      = 0.95r
orientation = in
</plot>
</plots>

<<include etc/housekeeping.conf>>

# make the link file the karyotype for both marchanthia and blasia in one circos.
# prepare the links file : 

import pandas as pd
import re

# File paths (modify these paths as needed)
gff_path = "path/to/your_file.gff"
collinearity_path = "path/to/your_file.collinearity"
output_link_file = "circos_links_final.txt"

# Define colors for Blasia chromosomes
blasia_colors = {
    'Bp_1': 'orange_a3',
    'Bp_2': 'yellow_a3',
    'Bp_3': 'green_a3',
    'Bp_4': 'red_a3',
    'Bp_5': 'darkred_a3',
    'Bp_6': 'magenta_a3',
    'Bp_7': 'pink_a3',
    'Bp_8': 'darkorange_a3',
    'Bp_9': 'gold_a3'
}

# Load GFF file
gff_columns = ['seqid', 'source', 'start', 'end']
gff_data = pd.read_csv(gff_path, sep='\t', header=None, usecols=[0, 1, 3, 4], names=gff_columns)

# Create a lookup dictionary for gene coordinates
gene_coordinates = {row['source']: (row['seqid'], int(row['start']), int(row['end'])) for _, row in gff_data.iterrows()}

# Extract gene pairs from the collinearity file
gene_pairs = []
with open(collinearity_path, 'r') as file:
    for line in file:
        match = re.match(r'\t(\S+)\t(\S+)\t', line)
        if match:
            gene_pairs.append((match.group(1), match.group(2)))

# Generate link data
circos_links = []
for gene1, gene2 in gene_pairs:
    if gene1 in gene_coordinates and gene2 in gene_coordinates:
        chr1, start1, end1 = gene_coordinates[gene1]
        chr2, start2, end2 = gene_coordinates[gene2]
        color = blasia_colors.get(chr1, 'black')  # Default to black if no color is assigned
        circos_links.append([chr1, start1, end1, chr2, start2, end2, f"color={color}"])

# Convert to DataFrame for easy handling
link_columns = ['chr1', 'start1', 'end1', 'chr2', 'start2', 'end2', 'color']
df_links = pd.DataFrame(circos_links, columns=link_columns)

# Save the Circos link file
df_links.to_csv(output_link_file, sep='\t', index=False, header=False)

print(f"Circos link file generated: {output_link_file}")

# Reload necessary libraries and define input/output paths
import pandas as pd

# Input and output file paths
input_file = "/mnt/data/circos_links_final_colored.txt"
output_file = "/mnt/data/circos_links_final_fixed.txt"

# Load the input file into a DataFrame
columns = ['chr1', 'start1', 'end1', 'chr2', 'start2', 'end2', 'color']
df = pd.read_csv(input_file, sep='\t', header=None, names=columns)

# Ensure the color field is formatted as 'color=<color>'
df['color'] = 'color=' + df['color'].astype(str)

# Save the corrected DataFrame to a new file
df.to_csv(output_file, sep='\t', header=False, index=False)

output_file

conda install -c conda-forge perl-gd perl-clone perl-list-moreutils perl-params-validate

# Include necessary configuration files
<<include etc/housekeeping.conf>>
<<include etc/colors_fonts_patterns.conf>>

# Karyotype file
karyotype = karyotype_combined.txt

######################################
# Ideogram Configuration
######################################

<ideogram>
    <spacing>
        default = 0.01r
    </spacing>

    radius           = 0.9r
    thickness        = 20p
    fill             = yes

    # Label Configuration
    show_label       = yes
    label_font       = bold
    label_radius     = dims(ideogram,radius) + 0.05r
    label_size       = 20
    label_parallel   = yes
</ideogram>

######################################
# Links Configuration
######################################

<links>
    <link>
        file           = circos_links_final_fixed.txt
        radius         = 0.85r
        bezier_radius  = 0.2r
        thickness      = 1
        color          = eval(var(color))
        z              = 1
    </link>
</links>


######################################
# Image Configuration
######################################

<image>
    dir    = .
    file   = circos_plot.png
    png    = yes
 radius = 1000p
</image>


6 — Links and Relationships
1. Drawing Basic Links
Lesson Images Configuration
circos.conf

<<include colors_fonts_patterns.conf>>

<<include ideogram.conf>>
<<include ticks.conf>>

<image>
<<include etc/image.conf>>
</image>

karyotype   = karyotype_combined.txt

chromosomes_units = 1000000
chromosomes       = Bp_1; Bp_2;Bp_3;Bp_4;Bp_5;Bp_6;Bp_7;Bp_8;Bp_9;Mp_1;Mp_2;Mp3;Mp_4;Mp_5;Mp_6;Mp_7;Mp_8;Mp_U;Mp_V
chromosomes_display_default = no

# If you adjust the radius of the ideograms, links incident
# on these ideograms will inherit the new radius.


# Links (bezier curves or straight lines) are defined in <links> blocks.
# Each link data set is defined within a <link> block. 
#
# As with highlights, parameters defined
# in the root of <links> affect all data sets and are considered
# global settings. Individual parameters value can be refined by
# values defined within <link> blocks, or additionally on each
# data line within the input file.

<links>

<link>
file          = circos_links_final.txt
radius        = 0.95r
color         = black_a4

# Curves look best when this value is small (e.g. 0.1r or 0r)
bezier_radius = 0.1r
thickness     = 1

# These parameters have default values. To unset them
# use 'undef'
#crest                = undef
#bezier_radius_purity = undef

# Limit how many links to read from file and draw
record_limit  = 2000

</link>

</links>

<<include etc/housekeeping.conf>>
data_out_of_range* = trim

# If you want to turn off all track default values, 
# uncomment the line below. This overrides the
# value of the parameter imported from etc/housekeeping.conf

#track_defaults* = undef
# The defaults for links are
#
# ribbon           = no
# color            = black
# thickness        = 1
# radius           = 0.40r
# bezier_radius    = 0r
# crest                = 0.5
# bezier_radius_purity = 0.75
#
# See etc/tracks/link.conf in Circos distribution

bands.conf
show_bands            = yes
fill_bands            = yes
band_stroke_thickness = 2
band_stroke_color     = white
band_transparency     = 0

ideogram.conf

<ideogram>

<spacing>
default = 0.01r
break   = 0.5r
</spacing>

<<include ideogram.position.conf>>
<<include ideogram.label.conf>>
<<include bands.conf>>

</ideogram>

ideogram.label.conf
show_label       = yes
label_font       = default
label_radius     = dims(ideogram,radius) + 0.075r
label_with_tag   = yes
label_size       = 36
label_parallel   = yes
label_case       = lower
label_format     = eval(sprintf("chr%s",var(label)))

ideogram.position.conf
radius           = 0.90r
thickness        = 100p
fill             = yes
fill_color       = black
stroke_thickness = 2
stroke_color     = black

ticks.conf

show_ticks          = yes
show_tick_labels    = yes

<ticks>

radius           = dims(ideogram,radius_outer)
orientation      = out
label_multiplier = 1e-6
color            = black
size             = 20p
thickness        = 3p
label_offset     = 5p

<tick>
spacing        = 1u
show_label     = no
</tick>

<tick>
spacing        = 5u
show_label     = yes
label_size     = 20p
format         = %d
</tick>

<tick>
spacing        = 10u
show_label     = yes
label_size     = 24p
format         = %d
</tick>

</ticks>

# INSTALL MCSCANX PYTHON version:

    pip3 install jcvi

    python3 -m jcvi.formats.fasta ACTION
    python3 -m jcvi.formats.fasta extract

## prepare the bed files and cds files from gff files:

    python3 -m jcvi.formats.gff bed --type=mRNA --key=ID  MpTak_v6.1r2_9chromos.gff -o marchanthia.bed

    python3 -m jcvi.formats.gff bed --type=mRNA --key=ID BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff -o blasia.bed

    python3 -m jcvi.formats.fasta format MpTak_v6.1r2.genome_9chroms_cds.fasta marchanthia.cds

    python3 -m jcvi.formats.fasta format  BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds_cds.fasta blasia.cds

## reorder the scaffolds in blasia 

    sed -e 's/HiC_scaffold_1/chr6/g' -e 's/HiC_scaffold_2/chr3/g' -e 's/HiC_scaffold_3/chr1/g' -e 's/HiC_scaffold_4/chr5/g' -e 's/HiC_scaffold_5/chr4/g' -e 's/HiC_scaffold_6/chr2/g' -e 's/HiC_scaffold_7/chr8/g' -e 's/HiC_scaffold_8/chr7/g' -e 's/HiC_scaffold_9/chr9/g' blasia.bed >blasia_reordered.bed

###  install last 

    git clone https://gitlab.com/mcfrith/last.git
    make

    export PATH="/home/ubuntu/USERS/Yuling/Blasia_Genome/comparative_genomics/last/bin:$PATH"

## running analysis for the dot plots

    python3 -m jcvi.compara.catalog ortholog marchanthia blasia --no_strip_names --full --min_size=6

    python3 -m jcvi.graphics.dotplot marchanthia.blasia.anchors
# relax parameter :

    python3 -m jcvi.compara.catalog ortholog marchanthia blasia --no_strip_names --full --dbtype nucl --cscore 0.5 --dist 30 --min_size 2 --self_remove 95 --cpus 16 

# this has the Mp1g17210 gene myb , but the depth is 2:1. 

python3 -m jcvi.compara.catalog ortholog marchanthia blasia --no_strip_names --full --dbtype nucl --cscore 0.5 --dist 28 --min_size 2 --self_remove 95 --cpus 16 

rm marchanthia.blasia.*

grep "Mp1g17210" marchanthia.blasia.anchors | head 

    python3 -m jcvi.compara.synteny depth --histogram marchanthia.blasia.anchors 

dist 30/29 has myb gene , has no 1:1 pattern .

dist 28 has no myb gene, has 1:1 pattern. 


# this shows 1:1 pattern , but it does not have the myb gene  set the csore as 0.6 and try again. 
# 0.6 does not work. try 0.55, does not work,try 0.525, does not work, try 0.5125.  it did not change. 

## myb gene is in here now. 

# do the same within blasia species 

python3 -m jcvi.compara.catalog ortholog blasia blasia --no_strip_names --full --dbtype nucl --cscore 0.5 --dist 30 --min_size 2 --self_remove 95 --cpus 16

 python3 -m jcvi.compara.catalog ortholog blasia blasia --no_strip_ncames --full --min_size=6

 # it did not show any anchors, even i change the cscore from default to 0.5 

## We could also quick test if the synteny pattern is indeed 1:1, by running: 

    python3 -m jcvi.graphics.dotplot marchanthia.blasia.anchors

    python3 -m jcvi.compara.synteny depth --histogram marchanthia.blasia.anchors 

# Now let's move to a different kind of visualization using the same synteny output grape.peach.anchors. Aside from the BED files and synteny files we already have, we need to prepare two additional files.

    chr1,chr2,chr3,chr4,chr5,chr6,chr7,chr8,chrU,chrV
    chr1,chr2,chr3,chr4,chr5,chr6,chr7,chr8,chr_9

    Ap_1,Ap_2,Ap_3,Ap_4,Ap_5,Ap_6,Ap_7,Ap_8,Ap_13,Ap_12,Ap_11,Ap_10,Ap_9,Ap_19,Ap_20,Ap_21,Ap_22,Ap_23,Ap_24,Ap_25
    Bp_5,Bp_7,Bp_1,Bp_2,Bp_3,Bp_8,Bp_4,Bp_6,Bp_9

# Second is the layout file, which tells the plotter where to draw what. The whole canvas is 0-1 on x-axis and 0-1 on y-axis. First, three columns specify the position of the track. Then rotation, color, label, vertical alignment (va), and then the genome BED file. Track 0 is now grape, track 1 is now peach. The next stanza specifies what edges to draw between the tracks. e, 0, 1 asks to draw edges between track 0 and 1, using information from the .simple file.

# y, xstart, xend, rotation, color, label, va,  bed
 .6,     .1,    .8,       0,      , marchanthia, top, marchanthia.bed
 .4,     .1,    .8,       0,      , blasia, top, blasia.bed
# edges
e, 0, 1, marchanthia.blasia.anchors.simple

# y, xstart, xend, rotation, color, label, va,  bed
 .6,     .1,    .8,       0,      , blasia_Ap, top, blasia_Ap.bed
 .4,     .1,    .8,       0,      , blasia,   top, blasia.bed
# edges
e, 0, 1, blasia_Ap.blasia.anchors.simple

    python3 -m jcvi.compara.synteny screen --simple blasia_Ap.blasia.anchors  blasia_Ap.blasia.anchors.new
     
    python3 -m jcvi.graphics.karyotype seqids layout

 python3 -m jcvi.compara.synteny screen --simple marchanthia.blasia.anchors marchanthia.blasia.anchors.new

# Further customization is possible. For example, change the order of chromosomes in seqids could lead to a visually more appealing figure. Also, play with the positions, color, labels etc in the layout file.

# What if we want to highlight a specific block? We should go into the .simple file, locate the relevant block. Please note that each line in the .simple file is a synteny blocks, with start and stop grape gene, then start and stop peach gene, final two columns are score and orientation.

    vi grape.peach.anchors.simple 

    GSVIVT01012228001	GSVIVT01012173001	Prupe.1G281700.1	Prupe.1G288300.1	72	-
    g*GSVIVT01012028001	GSVIVT01000604001	Prupe.1G299800.1	Prupe.1G340600.1	518	+
    GSVIVT01000603001	GSVIVT01000557001	Prupe.1G239100.1	Prupe.1G242900.2	51	+    
    ...
    
    (save the file)

# look at the sytenety locally:

    python3 -m jcvi.compara.synteny mcscan marchanthia.bed marchanthia.blasia.anchors --iter=1 -o  marchanthia.blasia.blocks

python3 -m jcvi.compara.synteny mcscan blasia_Ap.bed blasia_Ap.blasia.anchors --iter=1 -o  blasia_Ap.blasia.blocks

python3 -m jcvi.formats.bed merge blasia_Ap.bed blasia.bed -o blasia_Ap_blasia.bed

python3 -m jcvi.graphics.synteny  blasia_Ap.blasia.blocks blasia_Ap_blasia.bed layout --genelabelsize=10 

# OK. The file grape.peach.i1.blocks contains lots of local regions! We can choose any arbitrary region to visualize.

    tail -133 marchanthia.blasia.blocks > blocks

# get the genes from chr1 into blocks

head -3618 marchanthia.blasia.blocks > chr1.blocks

    # change in blocks to make the line g*

## chr1.blocks.layout

# x,   y, rotation,   ha,     va,   color, ratio,            label 
0.5, 0.6,        0, left, center,       m,     1,       blasia_Ap Ap_1,Ap_13,Ap_12,Ap_11,Ap_10,Ap_7,Ap_8,Ap_9
0.5, 0.4,        0, left, center, #fc8d62,     1,       blasia Bp_9
# edges
e, 0, 1

 python3 -m jcvi.formats.bed merge marchanthia.bed blasia.bed -o marchanthia_blasia.bed
 python3 -m jcvi.graphics.synteny chr9.blocks blasia_Ap_blasia.bed  chr9.blocks.layout  --genelabelsize=10 

 python3 -m jcvi.graphics.synteny chr1.blocks marchanthia_blasia.bed  chr1.blocks.layout --glyphcolor=orthogroup --genelabelsize=10 --genelabels=Mp1g17210.2,mRNA4854

python3 -m jcvi.graphics.synteny chr1.region.blocks marchanthia_blasia.bed  chr1.blocks.layout --genelabelsize=10 --genelabels=Mp1g17210.2,mRNA4854

# change in blocks to make the line g*

# chromplot:

## prepare the bed file:

    Chrom Start End Name
    1 1 124535434 142535434 heterochromatin
    2 1 121535434 124535434 centromere
    3 1 3845268 3995268 contig
    4 1 13219912 13319912 contig
    5 1 17125658 17175658 clone
    6 1 29878082 30028082 contig

    grep "gene" marchanthia_chromosomal_only_fixed.gff| awk '{ print $1 "\t" $4 "\t" $5 "\t" $9}'| sed 's/ID=//g' > marchanthia_chrmosomal_only_fixed.bed

    grep "gene" Blasia_chromosomal_only_fixed.gff| awk '{ print $1 "\t" $4 "\t" $5 "\t" $9}'| sed 's/ID=//g' > blasia_chrmosomal_only_fixed.bed

# prepare the same for the TE annotations

     grep "EDTA" assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.mod.EDTA.TEanno.gff3| awk -F ';' '{print $1}' > assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.mod.EDTA.TEanno_fixed.gff3

    grep "EDTA" assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta.mod.EDTA.TEanno.gff3 | 




# sliding-windows visualization with GENESPACE

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install(c("Biostrings", "rtracklayer"))

library(GENESPACE)


library(GENESPACE)
library(data.table)
library(ggplot2)


# load genome file
dnaSS <- Biostrings::readDNAStringSet("/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/Mquadrata.genome.fasta")
seqInfo <- pull_seqInfo(dnaSS)
# load gene annotation
genes <- as.data.frame(rtracklayer::readGFF(
  "/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/mqua.longGenes.gff",
  columns = c("seqid", "start", "end", "type"), 
  tags = "Parent", 
  filter = list(type = c("mRNA", "exon"))))
# load repeat annotation
repeats <- fread(
  "/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/Mquadrata.genome.fasta.out.bed",
  col.names = c("chr", "start", "end", "type"))
# Remove rows where 'start' or 'end' are not numeric)
repeats <- repeats[!is.na(as.numeric(repeats$start)) & !is.na(as.numeric(repeats$end)), ]
repeats$start <- as.numeric(repeats$start)
repeats$end <- as.numeric(repeats$end)

# make a list with all annotation files
bedList <- list(
  exon = with(subset(genes, type == "exon"), data.frame(
    chr = seqid, start = start, end = end)),
  LTRGypsy = with(subset(repeats, grepl("LTR/Gypsy", type)), data.frame(
    chr = chr, start = start, end = end)),
  LTRCopia = with(subset(repeats, grepl("LTR/Copia", type)), data.frame(
    chr = chr, start = start, end = end)),
  LTRunknown = with(subset(repeats, grepl("LTR/unknown", type)), data.frame(
    chr = chr, start = start, end = end)),
  otherRepeat = with(subset(repeats, !grepl("LTR", type)), data.frame(
    chr = chr, start = start, end = end)),
  introns = with(subset(genes, type == "mRNA"), data.frame(
    chr = seqid, start = start, end = end)))

print(lapply(bedList, head))

# calculate fractions
suppressWarnings(genomeClasses <- classify_genome(
  dnaSS = dnaSS, listOfBeds = bedList, verbose = T))

print(lapply(genomeClasses, head))

# calculate fractions in sliding windows
sws <- slide_genome(
  seqInfo = seqInfo,
  listOfGrs = genomeClasses[c(1,6,7,2,3,4,5)],
  windowSize = 2e6,
  stepSize = 1e5)
head(sws)

# plot 

plotCols <- c("#CC6828", "#F4A460", "#FFFFFF", "#0F4F8B", "#4C86C6", "#AED8E6", "darkgrey")
plotTheme <- theme(
  panel.background = element_rect(fill = "black"),
  panel.border = element_blank(),
  panel.grid = element_blank(),
  axis.ticks = element_blank(),
  axis.text = element_blank(),
  panel.spacing = unit(0, "cm"),
  strip.background = element_blank())

mq_p1 <- ggplot(sws, aes(x = start/1e6, y = propWind, fill = id))+
  geom_area()+
  scale_fill_manual(values = plotCols) +
  facet_grid(.~ gsub("chr","",chr), scale = "free", space = "free") +
  scale_x_continuous(expand = c(0.05,0.05))+
  scale_y_continuous(expand = c(0.005,0.005))+
  plotTheme +
  labs(x = "Chromosomes (scaled by physical size; 5Mb windows, 1Mb steps)",
       y = "Cumulative % of Sequence")
ggsave(plot=mq_p1, "/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/Mquadrata.genespace_linear.svg", height = 10, width = 20)

# M. polymorpha 

# load genome file
dnaSS <- Biostrings::readDNAStringSet("/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/Mpolymorpha_ruderalis.genome.fasta")
seqInfo <- pull_seqInfo(dnaSS)
# load gene annotation
genes <- as.data.frame(rtracklayer::readGFF(
  "/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/mporu.longGenes.gff",
  columns = c("seqid", "start", "end", "type"), 
  tags = "Parent", 
  filter = list(type = c("mRNA", "exon"))))
# load repeat annotation
repeats <- fread(
  "/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/Mpolymorpha_ruderalisUV.genome.fasta.out.bed",
  col.names = c("chr", "start", "end", "type"))
# Remove rows where 'start' or 'end' are not numeric)
repeats <- repeats[!is.na(as.numeric(repeats$start)) & !is.na(as.numeric(repeats$end)), ]
repeats$start <- as.numeric(repeats$start)
repeats$end <- as.numeric(repeats$end)

# make a list with all annotation files
bedList <- list(
  exon = with(subset(genes, type == "exon"), data.frame(
    chr = seqid, start = start, end = end)),
  LTRGypsy = with(subset(repeats, grepl("LTR/Gypsy", type)), data.frame(
    chr = chr, start = start, end = end)),
  LTRCopia = with(subset(repeats, grepl("LTR/Copia", type)), data.frame(
    chr = chr, start = start, end = end)),
  LTRunknown = with(subset(repeats, grepl("LTR/unknown", type)), data.frame(
    chr = chr, start = start, end = end)),
  otherRepeat = with(subset(repeats, !grepl("LTR", type)), data.frame(
    chr = chr, start = start, end = end)),
  introns = with(subset(genes, type == "mRNA"), data.frame(
    chr = seqid, start = start, end = end)))

print(lapply(bedList, head))

# calculate fractions
suppressWarnings(genomeClasses <- classify_genome(
  dnaSS = dnaSS, listOfBeds = bedList, verbose = T))

print(lapply(genomeClasses, head))

# calculate fractions in sliding windows
sws <- slide_genome(
  seqInfo = seqInfo,
  listOfGrs = genomeClasses[c(1,6,7,2,3,4,5)],
  windowSize = 2e6,
  stepSize = 5e4)
head(sws)

# plot M. polymorpha ----
mp_p1 <- ggplot(sws[grepl("^chr", sws$chr),], aes(x = start/1e6, y = propWind, fill = id))+
  geom_area()+
  scale_fill_manual(values = plotCols) +
  facet_grid(.~ gsub("chr","",chr), scale = "free", space = "free") +
  scale_x_continuous(expand = c(0.05,0.05))+
  scale_y_continuous(expand = c(0.005,0.005))+
  plotTheme +
  labs(x = "Chromosomes (scaled by physical size; 5Mb windows, 1Mb steps)",
       y = "Cumulative % of Sequence")
ggsave(plot=mp_p1, "/home/giacomo/work/liverworts/marchantia_quadrata/genespace_vis/Mpolymorpha_ruderalis.genespace_linear.svg", height = 10, width = 20)


# prepare the input files for blasia male:

awk 'BEGIN{OFS="\t"} !/^#/ {print $1, $4-1, $5, $3}' assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta.mod.EDTA.TEanno_Bp9.gff3 > Bp_9_TE.bed


awk 'BEGIN{OFS="\t"} 
     {if(!/^#/ && $3 != "region") print $1, $4-1, $5, $3}' assembly.fasta.PolcaCorrected.FINAL_9scaffolds_reordered.fasta.mod.EDTA.TEanno.gff3 > Bp_all_TE.bed




# female blasia :

# make files for the genome:

>Bp_1_RagTag
>Bp_2_RagTag
>Bp_3_RagTag
>Bp_4_RagTag
>Bp_5_RagTag
>Bp_6_RagTag
>Bp_7_RagTag
>Bp_8_RagTag
>Bp_9_RagTag
>scaffold_11

samtools faidx ragtag.scaffold.fasta Bp_1_RagTag Bp_2_RagTag Bp_3_RagTag Bp_4_RagTag Bp_5_RagTag Bp_6_RagTag Bp_7_RagTag Bp_8_RagTag Bp_9_RagTag scaffold_11 > Bp_all.fasta

# make files for the gff files:


awk 'BEGIN{OFS="\t"} !/^#/ {print $1, $4-1, $5, $3}' ragtag.new.fasta.mod.EDTA.TEanno_9+female_scaffolds.gff3 > Bp_9_TE.bed

awk 'BEGIN{OFS="\t"} !/^#/ {print $1, $4-1, $5, $3}' ragtag.new.fasta.mod.EDTA.TEanno.gff3 > Bp_all_TE.bed 


awk '$1 ~ /^#/ || $1 ~ /^Bp_[1-9]_RagTag$/ || $1 == "scaffold_11"' 890_ragtag_new_sorted_geneadded.gff > Bp_all.gff3



# do the same for marchanthia

awk 'BEGIN{OFS="\t"} 
     {if(!/^#/ && $3 != "region") print $1, $4-1, $5, $3}' MpTak_v6.1r2.genome_9chroms.fasta.mod.EDTA.TEanno.gff3 > Mp_all_TE.bed

awk '$1 ~ /^chr([1-9]|U|V)$/' Mp_all.gff3 > filtered.gff3

# make a stacked plot for the TEs  :


# Load libraries
library(ggplot2)
library(scales)
library(dplyr)
library(tidyr)

# Your color scheme
circosCols <- c("#fa8878", "#FFFFFF", "#ffbe7a", "#8dcec8", "#82afda", "#c2bdde")

# Example dataset
# Let's assume you have a dataframe 'repeat_data' with columns:
#  - class: repeat class (e.g., "Gene", "unknown", "LTRGypsy", etc.)
#  - type: chromosome type ("autosomes" or "sex_chromosomes")
#  - proportion: numeric proportion of this class in the genome

# Example:
repeat_data <- data.frame(
  class = rep(c("Gene", "unknown", "LTRGypsy", "LTRCopia", "Helitron", "TIR"), each=2),
  type = rep(c("autosomes", "sex_chromosomes"), times=6),
  proportion = c(0.15, 0.10, 0.25, 0.30, 0.10, 0.15, 0.05, 0.08, 0.20, 0.25, 0.15, 0.12)
)

# Order of classes for plotting
repeat_data$class <- factor(repeat_data$class, levels=c("Gene", "unknown", "LTRGypsy", "LTRCopia", "Helitron", "TIR"))

# Stacked bar plot
p <- ggplot(repeat_data, aes(x=type, y=proportion, fill=class)) +
  geom_bar(stat="identity", width=0.6) +
  scale_fill_manual(values=circosCols, name="Repeat Class") +
  scale_y_continuous(labels=scales::percent_format(accuracy=1)) +
  theme_minimal(base_size=14) +
  theme(
    axis.text.x=element_text(angle=45, hjust=1),
    panel.grid.major.x=element_blank()
  ) +
  labs(
    title="Repeat Comparison with M. polymorpha",
    subtitle="Absolute proportions and major class composition\nfor autosomes and sex chromosomes",
    x="Chromosome Type",
    y="Proportion of Genome"
  )

# Save
ggsave("repeat_comparison_stacked_barplot.svg", plot=p, width=10, height=6)

# Display
print(p)


cat Mp_all.fasta.fai
chr1	30584173	6	30584173	30584174
chr2	29643427	30584186	29643427	29643428
chr3	27142341	60227620	27142341	27142342
chr4	26988051	87369968	26988051	26988052
chr5	26794015	114358026	26794015	26794016
chr6	23861560	141152048	23861560	23861561
chr7	21963529	165013615	21963529	21963530
chr8	21314552	186977151	21314552	21314553
chrU	4523046	208291710	4523046	4523047
chrV	7543715	212814763	7543715	7543716

(base) jolene@jolenedeMacBook-Air Blasia_marchanthia_stacked_barplot %

 awk '{print $1 "\t0\t" $2}' Mp_all.fasta.fai >  Mp_all.fasta.bed

cat Bp_all.fasta.fai
Bp_1	68046419	6	80	81
Bp_2	61757168	68897012	80	81
Bp_3	46960569	131426151	80	81
Bp_4	44059094	178973734	80	81
Bp_5	33940421	223583573	80	81
Bp_6	32808566	257948256	80	81
Bp_7	31024484	291166936	80	81
Bp_8	29732083	322579233	80	81
Bp_9	29542758	352682974	80	81

# DATA USED

Files received from Yuling 18.03.2024

The final annotation is from one drive https://uzh-my.sharepoint.com/:f:/r/personal/peter_szoevenyi_systbot_uzh_ch/Documents/Projects/Students/Yuling_Y/Manuscripts/Blasia_Genome/Gametolog_trees?csf=1&web=1&e=3eecd1

Included the Australia, Postdam transcriptome assemblies and the Helsinki and Cornel genome annotations.

Analysis results and data are all in the onedrive folder https://uzh-my.sharepoint.com/:f:/r/personal/peter_szoevenyi_systbot_uzh_ch/Documents/Projects/Students/Yuling_Y/Manuscripts/Blasia_Genome/Gametolog_trees?csf=1&web=1&e=Q5URIh

Basic analysis:

--First took the trees from the Bowman Lunularia paper, converted nexus alignments to fasta https://academic.oup.com/gbe/article/15/3/evad014/7023155. Alignments are in the folder Bowman alignments
--Then added Blasia seqs from Yulings orthofinder analysis.
--Genertaed trees using online mafft and checked whether all homolos were properly identified
--Recied gene models/orthoglogs if needed (see below). The new alignments including our seq data are in Alignments_Bowman_plusourseqs



TO DO:

--Take fasta files from Alignments_Bowman_plusourseqs directory. Whenver there are two files per gametolog take the _NEW.fasta version.
--Run muscle with default option to align the sequences
--Finally, run iqtree to get the trees (get the software here).  RUna trial using the webservere to get the default command line using the webservice http://iqtree.cibiv.univie.ac.at/

PLUS: You need to correct/revise/modify the folllwing gene models in the Helsinki annotation gff and protein/cds etc files:



########################## NEW SEQUENCES TO BE ADDED TO THE GENOMIC GFF FILE ##################################################################

###################### The Helsinki homolog of MpU00090. NEED TO BE ADDED TO THE GFF FILE

## HELSINKI_GENOME_homologofMpU0009
``>HELSINKI_GENOME_homologofMpU00090
AGATCATCTCCTAAAAGAATCCAAGGTCCCGATTCTAGGAACCTGCGCCTACAGTTTCGTAACAAATTGG
CCCTTCCTTTGTTTACTGGAAGCAAGGTAGAAGGTGAGCAAGGGTCAGCCATACATGTAGTGCTACAAGA
TGCCACTGCTGGCCAAGTGGTAACAGCTGGAGCTGAGGCCTCTGCCAAGCTTGAAGTTGTGGTCTTAGAA
GGGGACTTTTCTGCAGATGATGAGGAGGACTGGCTACAGGAGGAGTTTGAGAATCATGAAGTAAGAGAAC
GAGATGGAAAGAGACCCCTTTTGACTGGGGATCTTTCAGTAACTCTTAAAGAAGGGGTTGGCACCCTTGG
GGAGTTGACATTTACAGATAACTCAAGTTGGATCCGGAGCAGAAAGTTTCGTCTGGGTGTGAAAATTTGC
TCCGGATACTGTGAAGGATTACGCATTCGCGAGGCCAAAACAGAAGCTTTCACAGTGAAGGACCATCGTG
GAAAAGTGTACAAGAAGCACTATCCTCCTGCCTTACACGATGAAGTTTGGCGGTTGGACAAAATCGGTAA
AGATGGTGCTTTTCACAAGAGATTGAACCAGTCCGGAATCCTCACAGTAGAAGACTTCCTCCGGCTGGTG
GTAATGGATCCTCAGAAGCTGCGCAATATTCTCGGGAATGGCATGTCGAACAAGATGTGGGAAGGGACTG
TGGAGCATGCCAAGACTTGTGTCCTTAGTGGCAAGCTTCATGTGTACTACGCAGATGAGAAGCAGAATAT
AGGTGTTATTTTCAACAACATTTTCCAGCTCATGGGACTGATTGCAGATGGACAGTACATGTCAGTTGAT
TCCCTGTCAGATAGTGAGAAGGTTTATGTGGATAAGCTGGTAAAGGTTGCGTACGAGAATTGGGAGAATG
TTGTGGAATATGATGGTGAAGCACTTATTGGTGTGAAGCCCTACAAACTTCGGGGAATCGATGCTCACAC
TGAGGACCCTATCAATGGACATCCATTTGTAGTGCAGAATTTGACAAGTGGTGTAGCTTCAAGTCAAATT
CCTGATGCGGTCACAAAATCCATATCACTGTCAGCAATGGCATCCAGTGGGAGACATGGATATGTCCCTT
TGGAAGAAAAACTGTATGTCAGTCAGGGTCATCACTCAATAATGGATCAGCCATACAGCCAGAGTATGTC
AGGTTTGAGTACAAGCATCTCACCAAGACATGTTTCTGTAGTCAACATATCCCATCAGAACTTTCTGCAT
CCAAGTACTGCAGCCATGACGGGACTGGCCCTTGGCCCTCCTCAGTCAACCCTCATTCCTACCTCAAATG
TGTCGTTTCCTATCACTGGCCATGCTACGGCTGTGGGATCCAGTACAGAGTGGCCCCGCTACAAGGATGT
TCGTGGTCAAGAAATGTCTCGAGCTGTTCAGGTTGATGAACTTTTTACTGAGGAGGAACTACGAGCCAAA
AGTTTAGAAATTTTGGAGAATGAAGATATGCACTTACAGATTCAGCAGTTGCTGAGAATGTTCAACACAG
CTTCAGGAGACACATCAGGCTCATTTCCCAATGTTCAGGGTGAGGACACTTACACCTACGGGTCTTTCTC
TCCTGCACCAGACTTGAACGTTGGAACAGACAGAGCAAAAGTGAACGGGCGAGCAAATGTGGGCTGGTTG
AAGCTTAAAGCAGCCTTACGATGGGGTATTTTCATTCGAAAGAGGGCAGCTGCAAGGAGAGCCCAGCTGG
AGGAATTGGAAGATGAATGA

``##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-03-19
##type DNA

HiC_scaffold_9  exonerate:coding2genome gene    27101874        27117522        2203    -       .       gene_id 1 ; sequence Postdam_female_TRINITY_DN3906_c0_g2_i7.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     27117033        27117522        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27117033        27117522        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27117031        27117032        .       -       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27116782        27117032        .       -       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 27116782        27116783        .       -       .       intron_id 0 ; splice_site "GG"
HiC_scaffold_9  exonerate:coding2genome cds     27116615        27116781        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27116615        27116781        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27116613        27116614        .       -       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27103282        27116614        .       -       .       intron_id 2
HiC_scaffold_9  exonerate:coding2genome splice3 27103282        27103283        .       -       .       intron_id 1 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     27103078        27103281        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27103078        27103281        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27103076        27103077        .       -       .       intron_id 3 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27102940        27103077        .       -       .       intron_id 3
HiC_scaffold_9  exonerate:coding2genome splice3 27102940        27102941        .       -       .       intron_id 2 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     27102702        27102939        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27102702        27102939        .       -       .       insertions 3 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27102700        27102701        .       -       .       intron_id 4 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27102545        27102701        .       -       .       intron_id 4
HiC_scaffold_9  exonerate:coding2genome splice3 27102545        27102546        .       -       .       intron_id 3 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     27101874        27102544        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27101874        27102544        .       -       .       insertions 0 ; deletions 27
HiC_scaffold_9  exonerate:coding2genome similarity      27101874        27117522        2203    -       .       alignment_id 1 ; Query Postdam_female_TRINITY_DN3906_c0_g2_i7.p1 ; Align 27117523 223 489 ; Align 27116780 715 165 ; Align 27103282 880 204 ; Align 27102940 1084 144 ; Align 27102793 1228 90 ; Align 27102543 1321 42 ; Align 27102501 1369 183 ; Align 27102318 1564 87 ; Align 27102231 1654 141 ; Align 27102090 1801 216
# --- END OF GFF DUMP ---
#


## MpU00430 THE GENE MODEL IN THE ORIGINAL GFF FILE WAS NOT OK WAS TOO SHORT MUST BE REPLACD WITH THE ONE HERE!!!

The seq mRNA19124 is ok but a too short translation from the genome. It must be replaced with the following gene model

``>Helsinki_amle_MpUg000430_properhomolog
ATGGGAACGGATGGCGTTATTCGAAGCTTATGGGACGAGGATAATGCGATGATAGAAGCATTCATGGGAT
CGGCTGTGTATAACTTATCCTGTAGCTCTCAAGTGGATTTGACGGGAACAGAAACACCAGCTTACAGCGA
GACGGCGTTACAACAAAAACTCCAGCTTCTGGTTGAGTCATCGAGTATGAACTGGACCTACGCAATCTTT
TGGCAGATTTCAAATTCATCGAGTGGAGAGATGATGCTAGGTTGGGGTGACGGTTACTTCAAGGGACCAA
AAGATAGTGAGGTGAATGATATGAAGCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTAAGGCG
AAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGATGCTGGAGGTTCTGGCGGGCTT
GATTATGTAACAGATACAGAGTGGTTCTACCTTGTTTCGATGTCATGTTTCTTTCCTCCTGGTGTTGGGA
TTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGGCTAATGGAAGCAAACAAGGCTCCTAGTAGTGT
TTGTACACGAGCGGATCTTGCTAAGTATGTGTTTGTTCAGACGCGTCTGTGTGTGCCATCAAGTAGTGGA
GTTGTTGAGCTGGGCTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTTT
TTGCTGAGATGAACGAGATTAGCGGAAACTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGGC
TTGCCCAGCAAGCCCCTCTCTGGTCAGCATTCATGGAACAAGAAGGCATGAGTCCTTGGACAGGGGACCT
GATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTAGACT
CTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTCGCTGATGTTTCTATTGACAATGAAAGGGAGCTTCA
GAGTGGAACTACAATTTCTGGATCATTCCGTGGAAGAAGCTCTGGTCAAGTAGACAAGGTTGACTCTATT
GGAGTGGTGAAGAAAGCTCAGCCTGGTGTTCAAGCTCAAAATCAAAGATGTGGAATAGAAGGTGAAATTT
TAAATCCACCACCAGATATGAGACAAGGCAGAAGGGAGGTGTTGCATCCGAATAGTGTGTCAACAACTCA
CATCAAAGAAGAAGGGCCAAGGACATCAAAGCACAAGGTCTCGCCATCTATGAAGATCCCACATCCAGAA
GGGGTGGGACCTCTTCCCAGTGCTTATGTCAGATCAAGTATTGAGTCTGAACAAGACTTGGATTCAGAGG
CAGAAGTTTCATACAAAGAAGTTGAGTGCAGTTTCACTGTTGGACAAAAGCCACCTAGAAAACGTGGAAG
AAAGCCTGCGAATGACAGGGAAGAACCTTTAAATCATGTACAGGCTGAACGACAAAGACGAGAGAAACTC
AACCAGCGGTTCTATGCCTTGCGCTCTGTCGTTCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAG
GTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTACAGGATATGGAGTTTCAAATCAAGGAACT
CGAGGGTCAAGCAAAATTGCTGAGAGAAAAATCATCTGAGACAGAGATTGCAAGAGGAGGCATGACTAAG
TTGTCTAGAGACATTTCACATCCAAAAACCTTTTCTGGTGGAAGCTCGATGACTTCTGGAGTCAAGATTT
CTACGGATCAAAGACCAATGGTGTCTGTGCATGTCTTACGAAATGAGGCAATGATTCGAGTACATAGCAC
CAAGGAGTCATCTGGAATCGCAAACTTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGCATGCC
AATCTATCAATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAACGCACCAT
CAGAGGCGCAGCTGATTAGTGCAATTGAAAAGTCATTTGACACAGGTGTTATAAACCCAATTAAAATAAA
TCATGATCCATACCTTGTAGAACTCAAACCCGACAATGGAAAATGTAGTATTCATTGA

##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-05-14
##type DNA
HiC_scaffold_9  exonerate:coding2genome gene    21343425        21395260        3202    -       .       gene_id 1 ; sequence Postadam_female_TRINITY_DN1489_c0_g2_i3.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     21395019        21395260        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21395019        21395260        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21395017        21395018        .       -       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21391359        21395018        .       -       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 21391359        21391360        .       -       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21391113        21391358        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21391113        21391358        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21391111        21391112        .       -       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21385514        21391112        .       -       .       intron_id 2
HiC_scaffold_9  exonerate:coding2genome splice3 21385514        21385515        .       -       .       intron_id 1 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21385417        21385513        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21385417        21385513        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21385415        21385416        .       -       .       intron_id 3 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21375544        21385416        .       -       .       intron_id 3
HiC_scaffold_9  exonerate:coding2genome splice3 21375544        21375545        .       -       .       intron_id 2 ; splice_site "AT"
HiC_scaffold_9  exonerate:coding2genome cds     21375472        21375543        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21375472        21375543        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21375470        21375471        .       -       .       intron_id 4 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21375038        21375471        .       -       .       intron_id 4
HiC_scaffold_9  exonerate:coding2genome splice3 21375038        21375039        .       -       .       intron_id 3 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21374177        21375037        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21374177        21375037        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21374175        21374176        .       -       .       intron_id 5 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21369691        21374176        .       -       .       intron_id 5
HiC_scaffold_9  exonerate:coding2genome splice3 21369691        21369692        .       -       .       intron_id 4 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21369271        21369690        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21369271        21369690        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21369269        21369270        .       -       .       intron_id 6 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21343575        21369270        .       -       .       intron_id 6
HiC_scaffold_9  exonerate:coding2genome splice3 21343575        21343576        .       -       .       intron_id 5 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21343425        21343574        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21343425        21343574        .       -       .       insertions 0 ; deletions 0



##  MpUg00350   THIS IS THE NEW HELSINKI GENE TO BE ADDED TO THE GFF FILE

##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-05-14
##type DNA
HiC_scaffold_9  exonerate:coding2genome gene    26880469        26882256        1057    +       .       gene_id 1 ; sequence TRINITY_DN8004_c0_g1_i1.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     26880469        26880686        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    26880469        26880686        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 26880687        26880688        .       +       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  26880687        26881820        .       +       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 26881819        26881820        .       +       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     26881821        26882256        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    26881821        26882256        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome similarity      26880469        26882256        1057    +       .       alignment_id 1 ; Query TRINITY_DN8004_c0_g1_i1.p1 ; Align 26880469 1 216 ; Align 26881822 220 435
# --- END OF GFF DUMP ---
#
``>Halsinki_Male_MpUg00350_TRINITY_DN8004_c0_g1_i1.p1
ATGGCGTACAGGTCAGATGACGATTATGATTATCTTTTCAAGGTGGTGTTGATCGGCGATTCTGGTGTTG
GAAAATCAAATCTTCTTTCCAGATTCACCCGCAACGAGTTCAGCCTAGAGTCAAAATCGACCATCGGGGT
GGAGTTTGCGACGAGGAGCATAAATGTTGACAGCAAGCTGATCAAGGCACAGATATGGGATACTGCTGGT
CAAGAAAGGTACCGGGCTATTACAAGTGCCTATTACAGAGGAGCAGTTGGAGCATTGTTAGTGTACGACA
TAACTCGACATGTGACTTTTGAAAACGTGGAAAGATGGTTGAAAGAGCTCAAGGATCACACCGATTCCAA
CATTGTCGTGATGTTGGTTGGGAACAAGTCTGACCTGCGCCATTTAAGAGCTGTTTCTGCTGATGATGGT
CAATCATTCTCTGAGAAGGAGGGCCTTTTCTTCATGGAGACCTCAGCTCTTGAATCAACAAATGTGGAGA
ATGCTTTCAAACAGATATTGACTCAGATATACCGGGTTGTGAGCAAGAAGGCACTGGATGTTGGAGATGA
TCCATCAACTGTGCCAGGGAAGGGGCAAACCATAAGTGTTGGAAACAAGGATGATGTTACTGCAACAAAA
AAGGTTGGGTGCTGTTCTGCATAG




## MY DETAILED ANALYSES 

# MgU00010:::

## Blasted >Blasia_pusilla_c334155 againts Blasia_Male_Helsinki_primary_cds.fasta using blastn

Query= Blasia_pusilla_c334155

Length=3165


***** No hits found *****



Lambda      K        H
    1.33    0.621     1.12

Gapped
Lambda      K        H
    1.28    0.460    0.850

Effective search space used: 64649324724


  Database: Blasia_Male_Helsinki_primary_cds.fasta
    Posted date:  Mar 18, 2024  12:26 PM
  Number of letters in database: 21,038,712
  Number of sequences in database:  17,046



Matrix: blastn matrix 1 -2
Gap Penalties: Existence: 0, Extension: 2.5

NO HIT


## against Cornell cds 

NO HIT



## against Blasia_Female_Potsdam_primary_cds.fasta

TWO HITS WERE FOUND

Query= Blasia_pusilla_c334155

Length=3165
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

TRINITY_DN4976_c0_g3_i1.p1 TRINITY_DN4976_c0_g3~~TRINITY_DN4976_c...  2763    0.0
TRINITY_DN572_c0_g2_i11.p1 TRINITY_DN572_c0_g2~~TRINITY_DN572_c0_...  2447    0.0


>TRINITY_DN4976_c0_g3_i1.p1 TRINITY_DN4976_c0_g3~~TRINITY_DN4976_c0_g3_i1.p1  ORF type:complete len:506 (-),score=65.28 TRINITY_DN4976_c0_g3_i1:1073-2590(-)
ATGGAGAGCTTGGGGCAGCTGGAGGTCTTGTGTGAGAGACTCTTTAATTCACAAGATCCATTGGAACGAGCAGTTGCAGAGAGCACTCTGCAATGTTTCTCTGTTAATGCAGATTACATTTCCCGGTGCCAATATATTCTAGATAGCTCTGTTTCACCTTATGCTCAGCTGCTTGCAAGTTCTAGTCTGCTGAAGCAAGTTACGGAGCAGATTCTGTCAGTGCAGCTTCGTCTGGACATTCGAAATTATGTTGTCGGTTACCTGGCAAGGAAAGGCCAAGAGCTGCAATCTTTTGTTGCTACGTCCTTGATTCAGCTTCTTTGTAGAATAACAAAACTTGGATGGTATGTTGATGAAAACTTCCGTGATATTGTGAAGGAGGCAATGAATTTCCTCAATCAGGTAACAGCTGAGCATTTCTATATTGGATTAAAGATTCTCAATCAGCTCATTGCGGAAATGAATCAGGCCAATCCAGGTCTCTCTCTCGCTCATCATCGCAAAACAGCTTGCTCCTTTCGTGATCTTGCACTACTCCAAATTTTCCAAATTTCATTGACATCTCTCCGACAACTGCAAAATGATACACCTGCAGAGAAACTACGAGAGCAAGCGGTTGCCTTGGCCTTGAAATGCTTGTCCTTCGACTTTGTTGGAACATCACTTGACGAGAGCTCAGAAGATCTTGGTACCATTCAGATACCTTCATCATGGAGACCTCTTCTCGAGGATATGGGGACAATGCAAGTATTTTTTGATTATTATGCCATCACACAGCCACCACTTTCAAACGAGTCACTGGAGTGCCTTGTTAGATTAGCATCTGTTCGGCGATCATTGTTTTCGGGGGATATGGAACGCACCAAATTTTTGTCTCGGTTAATGGGGGGCACAAAGGACATACTCCAATTCCAGCTAGGACTTGGTCAGCATGAAAACTATCATGAATATTGCCGCCTGCTGGGTCGTTTGAAAACAAATTACCAGCTCTCAGAGCTGGTGAGTGTGGACAGTTACAGTGATTGGATTCGACTGGTGGCTGATTTCACAGTAAAATCCTTGCAATCGTGGCAGTGGGCAAGTGGCAGTGTTTACTATCTCTTGGGGCTATGGTCAAGACTTGTGTCTTCTGTGGCTTACTTGAAAAGTGATTGCCCTAGTCTCCTGGACGGTTATGTGCCGAGAATCATGGAAGCGTACATCACTTCAAGGCTTGATTCAGTGGAGGCTATTGTGGATAAGAAAACAGTTGATGATCCATTAGAGAACGGGGAGCAGCTGCAAGATCAGCTTGATAGTCTGCCCTACCTTTGTCGATTCCAGTATGAAAGTACAAGCAATTATATTGTCACCCACCTTGAGCCAGTGCTGGAGGCATACACGGCTGCTAGCAGATTGTCTGCAATTCAAAATAGCAGTCAATTGACAGTATTGGAAGGCCAACTCACTTGGCTTGTTCATATTATCGGAGCTATTATCAGAGGAAGACAAAGTGCAAGCATGAGGTTTGTTGCCTGA
>TRINITY_DN572_c0_g2_i11.p1 TRINITY_DN572_c0_g2~~TRINITY_DN572_c0_g2_i11.p1  ORF type:complete len:456 (+),score=56.92 TRINITY_DN572_c0_g2_i11:3453-4820(+)
ATGGGGACAATGCAAGTATTTTTTGATTATTATGCCATCACACAGCCACCACTTTCAAACGAGTCACTGGAGTGCCTTGTTAGATTAGCATCTGTTCGGCGATCATTGTTTTCGGGGGATATGGAACGCACCAAATTTTTGTCTCGGTTAATGGGGGGCACAAAGGACATACTCCAATTCCAGCTAGGACTTGGTCAGCATGAAAACTATCATGAATATTGCCGCCTGCTGGGTCGTTTGAAAACAAATTACCAGCTCTCAGAGCTGGTGAGTGTGGACAGTTACAGTGATTGGATTCGACTGGTGGCTGATTTCACAGTAAAATCCTTGCAATCGTGGCAGTGGGCAAGTGGCAGTGTTTACTATCTCTTGGGGCTATGGTCAAGACTTGTGTCTTCTGTGGCTTACTTGAAAAGTGATTGCCCTAGTCTCCTGGACGGTTATGTGCCGAGAATCATGGAAGCGTACATCACTTCAAGGCTTGATTCAGTGGAGGCTATTGTGGATAAGAAAACAGTTGATGATCCATTAGAGAACGGGGAGCAGCTGCAAGATCAGCTTGATAGTCTGCCCTACCTTTGTCGATTCCAGTATGAAAGTACAAGCAATTATATTGTCACCCACCTTGAGCCAGTGCTGGAGGCATACACGGCTGCTAGCAGATTGTCTGCAATTCAAAATAGCAGTCAATTGACAGTATTGGAAGGCCAACTCACTTGGCTTGTTCATATTATCGGAGCTATTATCAGAGGAAGACAAAGTGCAAGCATGAGCACTGAATCTCATGAGCTCATTGACGGGGATCTTGCTGCGCGTGTTTTTCAACTAATTCAAGTCATGGATATTGGCGTCCACATGAAGCGTTATCATGAGCGAAGCAAGCAGCGATTAGACTTGGCAGTTCTGAATTTTTTCCAAAATTTTCGTCGTGTCTATGTTGGTGATCAAGCTATGCATTCATCAAAGCAAGTGTATCTGCGTTTGGCGGAGCTTCTTGGCCTTCAAGATCATGTAATGGTTTTGAATGTTACAATTGAGAAGATTGGCACCAACTTGAAATGCTACACTGAGAGTGAAGAAGTGATTGGGGAGACTCTGAGCCTCTTTCAGGAACTGGCCACTGGATACATGAGTGGGAAATTGCTACTGAAGCTGGATGCTGTGAATTTTATTATGGGACATCATACGAAGGAGTACTTTCCTTTCTTGGAGGAGTCTACCTGCTCCAGAAACAGGACTATATTTTACTTCACCTTGGGTCGTCTGCTCTTCATGGAGGATAGTTTGGCAAAATTCAAAGCTTTTGTGGCTCCCTTAGAGCAGGTTGTTGATTGCAGCAAAAGTAACGCTTTAATTGGGGTTCTATGA


## against Blasia_Female_Austrilia_primary_cds.fasta

two hist

Query= Blasia_pusilla_c334155

Length=3165
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.14087.1.p1 GENE.STRG.14087.1~~STRG.14087.1.p1 ORF type:compl...  2763    0.0
STRG.14498.1.p1 GENE.STRG.14498.1~~STRG.14498.1.p1 ORF type:compl...  2447    0.0


>STRG.14087.1.p1 GENE.STRG.14087.1~~STRG.14087.1.p1  ORF type:complete (-),score=78.55 len:1518 STRG.14087.1:941-2458(-)
ATGGAGAGCTTGGGGCAGCTGGAGGTCTTGTGTGAGAGACTCTTTAATTCACAAGATCCATTGGAACGAGCAGTTGCAGAGAGCACTCTGCAATGTTTCTCTGTTAATGCAGATTACATTTCCCGGTGCCAATATATTCTAGATAGCTCTGTTTCACCTTATGCTCAGCTGCTTGCAAGTTCTAGTCTGCTGAAGCAAGTTACGGAGCAGATTCTGTCAGTGCAGCTTCGTCTGGACATTCGAAATTATGTTGTCGGTTACCTGGCAAGGAAAGGCCAAGAGCTGCAATCTTTTGTTGCTACGTCCTTGATTCAGCTTCTTTGTAGAATAACAAAACTTGGATGGTATGTTGATGAAAACTTCCGTGATATTGTGAAGGAGGCAATGAATTTCCTCAATCAGGTAACAGCTGAGCATTTCTATATTGGATTAAAGATTCTCAATCAGCTCATTGCGGAAATGAATCAGGCCAATCCAGGTCTCTCTCTCGCTCATCATCGCAAAACAGCTTGCTCCTTTCGTGATCTTGCACTACTCCAAATTTTCCAAATTTCATTGACATCTCTCCGACAACTGCAAAATGATACACCTGCAGAGAAACTACGAGAGCAAGCGGTTGCCTTGGCCTTGAAATGCTTGTCCTTCGACTTTGTTGGAACATCACTTGACGAGAGCTCAGAAGATCTTGGTACCATTCAGATACCTTCATCATGGAGACCTCTTCTCGAGGATATGGGGACAATGCAAGTATTTTTTGATTATTATGCCATCACACAGCCACCACTTTCAAACGAGTCACTGGAGTGCCTTGTTAGATTAGCATCTGTTCGGCGATCATTGTTTTCGGGGGATATGGAACGCACCAAATTTTTGTCTCGGTTAATGGGGGGCACAAAGGACATACTCCAATTCCAGCTAGGACTTGGTCAGCATGAAAACTATCATGAATATTGCCGCCTGCTGGGTCGTTTGAAAACAAATTACCAGCTCTCAGAGCTGGTGAGTGTGGACAGTTACAGTGATTGGATTCGACTGGTGGCTGATTTCACAGTAAAATCCTTGCAATCGTGGCAGTGGGCAAGTGGCAGTGTTTACTATCTCTTGGGGCTATGGTCAAGACTTGTGTCTTCTGTGGCTTACTTGAAAAGTGATTGCCCTAGTCTCCTGGACGGTTATGTGCCGAGAATCATGGAAGCGTACATCACTTCAAGGCTTGATTCAGTGGAGGCTATTGTGGATAAGAAAACAGTTGATGATCCATTAGAGAACGGGGAGCAGCTGCAAGATCAGCTTGATAGTCTGCCCTACCTTTGTCGATTCCAGTATGAAAGTACAAGCAATTATATTGTCACCCACCTTGAGCCAGTGCTGGAGGCATACACGGCTGCTAGCAGATTGTCTGCAATTCAAAATAGCAGTCAATTGACAGTATTGGAAGGCCAACTCACTTGGCTTGTTCATATTATCGGAGCTATTATCAGAGGAAGACAAAGTGCAAGCATGAGGTTTGTTGCCTGA
>STRG.14498.1.p1 GENE.STRG.14498.1~~STRG.14498.1.p1  ORF type:complete (+),score=71.32 len:1368 STRG.14498.1:3380-4747(+)
ATGGGGACAATGCAAGTATTTTTTGATTATTATGCCATCACACAGCCACCACTTTCAAACGAGTCACTGGAGTGCCTTGTTAGATTAGCATCTGTTCGGCGATCATTGTTTTCGGGGGATATGGAACGCACCAAATTTTTGTCTCGGTTAATGGGGGGCACAAAGGACATACTCCAATTCCAGCTAGGACTTGGTCAGCATGAAAACTATCATGAATATTGCCGCCTGCTGGGTCGTTTGAAAACAAATTACCAGCTCTCAGAGCTGGTGAGTGTGGACAGTTACAGTGATTGGATTCGACTGGTGGCTGATTTCACAGTAAAATCCTTGCAATCGTGGCAGTGGGCAAGTGGCAGTGTTTACTATCTCTTGGGGCTATGGTCAAGACTTGTGTCTTCTGTGGCTTACTTGAAAAGTGATTGCCCTAGTCTCCTGGACGGTTATGTGCCGAGAATCATGGAAGCGTACATCACTTCAAGGCTTGATTCAGTGGAGGCTATTGTGGATAAGAAAACAGTTGATGATCCATTAGAGAACGGGGAGCAGCTGCAAGATCAGCTTGATAGTCTGCCCTACCTTTGTCGATTCCAGTATGAAAGTACAAGCAATTATATTGTCACCCACCTTGAGCCAGTGCTGGAGGCATACACGGCTGCTAGCAGATTGTCTGCAATTCAAAATAGCAGTCAATTGACAGTATTGGAAGGCCAACTCACTTGGCTTGTTCATATTATCGGAGCTATTATCAGAGGAAGACAAAGTGCAAGCATGAGCACTGAATCTCATGAGCTCATTGACGGGGATCTTGCTGCGCGTGTTTTTCAACTAATTCAAGTCATGGATATTGGCGTCCACATGAAGCGTTATCATGAGCGAAGCAAGCAGCGATTAGACTTGGCAGTTCTGAATTTTTTCCAAAATTTTCGTCGTGTCTATGTTGGTGATCAAGCTATGCATTCATCAAAGCAAGTGTATCTGCGTTTGGCGGAGCTTCTTGGCCTTCAAGATCATGTAATGGTTTTGAATGTTACAATTGAGAAGATTGGCACCAACTTGAAATGCTACACTGAGAGTGAAGAAGTGATTGGGGAGACTCTGAGCCTCTTTCAGGAACTGGCCACTGGATACATGAGTGGGAAATTGCTACTGAAGCTGGATGCTGTGAATTTTATTATGGGACATCATACGAAGGAGTACTTTCCTTTCTTGGAGGAGTCTACCTGCTCCAGAAACAGGACTATATTTTACTTCACCTTGGGTCGTCTGCTCTTCATGGAGGATAGTTTGGCAAAATTCAAAGCTTTTGTGGCTCCCTTAGAGCAGGTTGTTGATTGCAGCAAAAGTAACGCTTTAATTGGGGTTCTATGA


SEARCH FOR MALE HOMLOG IN GENOME SEQUENCE!!!!!!



DONE





# MpgU00090 (Blasiaseq_from_MpgU00090.fasta)


## against Helsinki

NO HIT FOUND

Query= Blasia_pusilla_c316451

Length=1380


***** No hits found *****



Lambda      K        H
    1.33    0.621     1.12

Gapped
Lambda      K        H
    1.28    0.460    0.850

Effective search space used: 27930021510


  Database: Blasia_Male_Helsinki_primary_cds.fasta
    Posted date:  Mar 18, 2024  12:26 PM
  Number of letters in database: 21,038,712
  Number of sequences in database:  17,046



Matrix: blastn matrix 1 -2
Gap Penalties: Existence: 0, Extension: 2.5


## against Cornell

NO HIT

Query= Blasia_pusilla_c316451

Length=1380


***** No hits found *****



Lambda      K        H
    1.33    0.621     1.12

Gapped
Lambda      K        H
    1.28    0.460    0.850

Effective search space used: 26534830855


  Database: Blasia_Male_Cornell_primary_cds.fasta
    Posted date:  Mar 18, 2024  12:38 PM
  Number of letters in database: 19,970,951
  Number of sequences in database:  15,522



Matrix: blastn matrix 1 -2
Gap Penalties: Existence: 0, Extension: 2.5


## against Potsdam


Query= Blasia_pusilla_c316451

Length=1380
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

TRINITY_DN3906_c0_g2_i7.p1 TRINITY_DN3906_c0_g2~~TRINITY_DN3906_c...  2043    0.0
TRINITY_DN3906_c0_g1_i19.p1 TRINITY_DN3906_c0_g1~~TRINITY_DN3906_...  2032    0.0

>TRINITY_DN3906_c0_g2_i7.p1 TRINITY_DN3906_c0_g2~~TRINITY_DN3906_c0_g2_i7.p1  ORF type:complete len:672 (+),score=131.23 TRINITY_DN3906_c0_g2_i7:585-2600(+)
ATGGCAAGAGAGAAGCGGCCGCTGGAGAAGGATGCATCCACGGAAGGCATGCCAGAGGAAAAGCGGCAACGCGTCCCCGCCCTAGCTAGCGTGATCGTGGAAGCGATGAAAATGGATAGCCTCCAGAAGCTATGCTCGACTCTCGAGCCTTTGCTTCGCAGGGTGGTGGGAGAAGAATTGGAGCGCGCACTTTCAAAGCTAACTCCCGCCAAAGTAGGTTTCAGATCGTCTCCTAAAAGAATCCAGGGCCTTGATGCAAGAAGCTTACGGCTTCAGTTCAGAAATAAACTGGCTCTTCCTTTATTCACAGGGAGCAAGGTTGAAGGTGAACAAGGATCAACTATTCATGTTGTCTTGCAAGATGCAACCTCGGGTCAGGTTGTGACAGGGGGTGCAGAAGCCTCAGCAAAGCTTGAAGTAGTGGTACTTGAAGGTGATTTTTCGGCAGATGATGAAGAGGACTGGCCACAAGAAGAATTTGAAAACCATGAAGTTCGTGAGCGTGATGGTAAGAGACCTCTTTTGACAGGAGATCTTTTTGTGACTTTGAAAGAAGGTGTGGGAACTCTTGGAGAACTGACATTCACAGACAATTCGAGCTGGATAAGGAGCAGAAAGTTTCGGCTTGGAGTAAAAATTTCCTCAGGGTACTGTGAGGGACTCCGAATCCGAGAGGCAAAGACTGAGGCTTTTACTGTTAAAGACCATCGTGGAGAATTGTATAAAAAGCATTACCCACCAGCATTGCACGATGAAGTGTGGCGTCTTGACAAGATTGGGAAGGATGGTGCTTTCCACAAGCGACTAAACCAGTCTGGAATACAGACTGTGGAGGACTTTTTAAGGCTTGTCGTAATGGATCCTCAAAAACTTCGCAATATTTTGGGCAATGGAATGTCCAACAAGATGTGGGAGGGAACTGTGGAGCACGCCAAAACATGTGTCCTTAGTGGCAAGCTACATGTGTACTATGCTGATGATAAGCAGAATATTGGAGTTATATTCAACAACATTTTCCAGCTCATGGGCCTTATTGCAGATGGACAATACATGTCTGTGGATTCTCTATCAGACAGTGAGAAGGTGTATGTGGATAAACTTGTAAAGGTGGCGTATGAGAACTGGGAGAACGTGGTGGAGTATGATGGTGAAGCTCTCATTGGTGTGAAACCATATAAGCTCAGGGGAATTGATGCCCATACTGAGGATCCAATTAATGGCCAAGGACCAAGTGGGCATCAATCCCAGACAAGTGCATCGAATCAGCAAAGTGTCTCTACTACTCAAATAATGGCAATGCAGCGGCCTTCAGCTGGTGGTGGTCAGGTTTACACACCACCAGAACAGAAAGTGCACATGAAGTATCTGAACCAAGCAAATTCTTTATCTGAGCATCCAAGTAGCCAGGCACTCCTGAGTACAGGGATAAGTTTCCCTCCCAGTCCATCTTCTATGATCAGCATTTCACATCAGAATTTCTTGCATGGTATCAATGCCCCTATGACTGGACTGGCACTTGGTCTTCCTCAAACCAACATGGGTTCATCTTCAGGACAGCCAATTTCAGTAGCCAATCCAGTTCATAGTATGCCCAGTGGTGTTACCTCAGTAAGTGACTGGCCGCGATATAAAGAGCTCAGAGGACAGGAAAGTTTGTCACGAGGCGTGCCGATGGATGAATTGTTTACAGAGGAAGAACTACGGGCAAAGAGTCTGGAGATTCTTGAAAATGAAGACATGCATTTACAGATTCAGCAACTTCTACGCATGTTTAATGCTGCTACAGGAGAAACACCACCACCAAACCCATATCCTAATGTCCAAGGAGACGAAAGTTTTACTTTTGGAGCCTTCTCTCCTGCACCAGACTTGAATGTTGGCATGGACAGAACTCGTGTGAATGGTAGGGCAAACGTTGGCTGGTTGAAACTCAAGGCTGCTCTGCGATGGGGAATTTTTATTAGGAAGAGGGCAGCTGAACGAAGGGCCCAGCTTGAGGAGCTGGAGGATGAGTGA
>TRINITY_DN3906_c0_g1_i19.p1 TRINITY_DN3906_c0_g1~~TRINITY_DN3906_c0_g1_i19.p1  ORF type:complete len:672 (-),score=129.51 TRINITY_DN3906_c0_g1_i19:687-2702(-)
ATGGCAAGAGAGAAGCGGCCGCTGGAGAAGGATGCATCCACGGAAGGCATGCCAGAGGAAAAGCGGCAACGCGTCCCCGCCCTAGCTAGCGTGATCGTGGAAGCGATGAAAATGGATAGCCTCCAGAAGCTATGCTCGACTCTCGAGCCTTTGCTTCGCAGGGTGGTGGGAGAAGAATTGGAGCGCGCACTTTCAAAGCTAACTCCCGCCAAAGTAGGTTTCAGATCGTCTCCTAAAAGAATCCAGGGCCTTGATGCAAGAAGCTTACGGCTTCAGTTCAGAAATAAACTGGCTCTTCCTTTATTCACAGGGAGCAAGGTTGAATGTGAACAAGGATCAACTATTAATGTTGTCTTGCAAGATGCAACCTCGGGTCAGGTTGTGACAGGGGGTGCAGAAGCCTCAGCAAAGCTTGAAGTAGTGGTACTTGAAGGTGATTTTTCGGCAGATGATGAAGAGGACTGGCCACAAGAAGAATTTGAAAACCATGAAGTTCGTGAGCGTGATGGTAAGAGACCTCTTTTGACAGGAGATCTTTTTGTGACTTTGAAAGAAGGTGTGGGAACTCTTGGAGAACTGACATTCACAGACAATTCGAGCTGGATAAGGAGCAGAAAGTTTCGGCTTGGAGTAAAAATTTCCTCAGGGTACTGTGAGGGACTCCGAATCCGAGAGGCAAAGACTGAGGCTTTTACTGTTAAAGACCATCGTGGAGAATTGTATAAAAAGCATTACCCACCAGCATTGCACGATGAAGTGTGGCGTCTTGACAAGATTGGGAAGGATGGTGCTTTCCACAAGCGACTAAACCAGTCTGGAATACAGACTGTGGAGGACTTTTTAAGGCTTGTCGTAATGGATCCTCAAAAACTTCGCAATATTTTGGGCAATGGAATGTCCAACAAGATGTGGGAGGGAACTGTGGAGCACGCCAAAACATGTGTCCTTAGTGGCAAGCTACATGTGTACTATGCTGATGATAAGCAGAATATTGGAGTTATATTCAACAACATTTTCCAGCTCATGGGCCTTATTGCAGATGGACAATACATGTCTGTGGATTCTCTATCAGACAGTGAGAAGGTGTATGTGGATAAACTTGTAAAGGTGGCGTATGAGAACTGGGAGAACGTGGTGGAGTATGATGGTGAAGCTCTCATTGGTGTGAAACCATATAAGCTCAGGGGAATTGATGCCCATACTGAGGATCCAATTAATGGCCAAGGACCAAGTGGGCATCAATCCCAGACAAGTGCATCGAATCAGCAAAGTGTCTCTACTACTCAAATAATGGCAATGCAGCGGCCTTCAGCTGGTGGTGGTCAGGTTTACACACCACCAGAACAGAAAGTGCACATGAAGTATCTGAACCAAGCAAATTCTTTATCTGAGCATCCAAGTAGCCAGGCACTCCTGAGTACAGGGATAAGTTTCCCTCCCAGTCCATCTTCTATGATCAGCATTTCACATCAGAATTTCTTGCATGGTATCAATGCCCCTATGACTGGACTGGCACTTGGTCTTCCTCAAACCAACATGGGTTCATCTTCAGGACAGCCAATTTCAGTAGCCAATCCAGTTCATAGTATGCCCAGTGGTGTTACCTCAGTAAGTGACTGGCCGCGATATAAAGAGCTCAGAGGACAGGAAAGTTTGTCACGAGGCGTGCCGATGGATGAATTGTTTACAGAGGAAGAACTACGGGCAAAGAGTCTGGAGATTCTTGAAAATGAAGACATGCATTTACAGATTCAGCAACTTCTACGCATGTTTAATGCTGCTACAGGAGAAACACCACCACCAAACCCATATCCTAATGTCCAAGGAGACGAAAGTTTTACTTTTGGAGCCTTCTCTCCTGCACCAGACTTGAATGTTGGCATGGACAGAACTCGTGTGAATGGTAGGGCAAACGTTGGCTGGTTGAAACTCAAGGCTGCTCTGCGATGGGGAATTTTTATTAGGAAGAGGGCAGCTGAACGAAGGGCCCAGCTTGAGGAGCTGGAGGATGAGTGA


## Australia

INCOMPLETE SEQUENCES SHORT

Query= Blasia_pusilla_c316451

Length=1380
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.13450.1.p1 GENE.STRG.13450.1~~STRG.13450.1.p1 ORF type:compl...  228     3e-59
STRG.13449.1.p1 GENE.STRG.13449.1~~STRG.13449.1.p1 ORF type:compl...  228     3e-59




# MpUg00110 
>Helsinki_male_mRNA17723 gene=gene15754
was an outlier replaced it with


## against Helsinki male

Query= Aistralia_female_STRG.3064.1.p1 GENE.STRG.3064.1~~STRG.3064.1.p1
ORF type:complete (-),score=72.09 len:1587 STRG.3064.1:450-2036(-)

Length=1587
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

mRNA7723 gene=gene6903                                                2652    0.0
mRNA7722 gene=gene6902                                                2593    0.0

>mRNA7723 gene=gene6903
ATGAGTCCTGTCCTACTCCTAGCAGAGAGCAGGAGTAAAGGTGGATATCTGAAAGGTCCCGGTCCTACTCTTTACCTAAGAAAGGTGACCGGTTCCCGGACGGGTCCCGTAGGGGATGAATGGCTGGGGGTCTTCGTTGAACGGGCAGAGAGAAGAGGAGGAGGAGGTGGAGGAAATTTCCGACGGAGAGGAAGAGCGAGGGTTCCGAACGGAGTCCACACGAGCGGGGGCGGATTGATTCACGGAGGCCGGGGAAGAGGGAGGGAGTCTTATGGAGTCGCGTACTTGTTGGGGATAGAGACGAAGAGAGGGAAATTGGTGCGCCCGCGTGGGCGTTCGTGTTGGGGAGGGAGCGGAATCGATCAGAGCGGCCGCGGAGTCGTCCAGGGAGGAGGTGGACGAGGAGTTCCGTTTGGCTGCAGAGATCTGCATGCTTTGACCAGGAGCTTCTGCAGCGTCGCCTATCAAGATTCAGCGGATCTTACGTACTGCTTAGGTTTCGGGGAGGCGCACTGTCACATTCGCGCATACGTGTGCGTTCTGTGCTCGAAAATAAAGTTCTGTATTGTAGATTTGGGTTTTAGGGCACAGAGAAATTGGCCCGAGGGCGACGGAGTGGTCTCGATCTGTGGCTTTGTTCTGATGTGGCCACCTCCCGGGCGCCAGTGGAGAAAGGACAGAGAAGAGCGACTTGAGGCCAGAGGGAGGAGTGTAAAGCCTGAAAGGCAGATTAGAAGGCTTGGCGATGGAGGGAAGTCAAAGGCGCTAGGTTTGTCTTTGGTGTCGAGTGTGATTCGTGAGCACTCCTCCCGCGGCCATTTCAATACCCTCCACACCCCTCCTCAGGTATTCCGCCAGATTGATTTTCCCCATAACGATCGATGGGGAATTGTTGCGCATGCGGGACGTGTGGAGCTCGGGCGGGGCGAGTTCGGCATCACCTACTTGTGTAAAGACAACAAAACTAAGGAGCAATTGGCATGCAAGTCCATTTCCAAGCGCAAGCTCCGGACCGCGGTGGATGTGGAAGATGTCCGGCGTGAAGTCGCGGTCATGCACCACCTCCCCCCTCATCCCAACATCGTCAGGCTGAGGGGTGTCTATCAAGACGAAGCCGCAGTGCACCTGGTCATGGAGCTCTGCGAGGGCGGGGAGCTCTTCGACCGCATCATCGCCCGTGGTCACTACAGCGAGCGCGAGGCTGCAGCCGTGACGCGCACCATCGTGGAGGTGGTTCAGGTCTGTCACAAGCACCGGGTGATGCACCGCGACCTCAAGCCGGAGAACTTTCTCTTCGCCAGTAAGGACAATAACTCACCTCTGAAGGCCATAGATTTCGGGCTTTCGTGTTTCTTCTCACCTGGGGAACGCTTCTCGGAGATCGTGGGCAGCCCTTACTACATGGCGCCCGAGGTCCTCAAGAGGAATTATGGTCCGGAGGTGGATGTGTGGAGTGCGGGAGTGATTCTTTACATTCTTCTATGCGGAGTGCCTCCTTTCTGGGCTGAGACCGAGCAAGGAGTGGCGCTTGCTATTCTCCGCGGGGTGCTGGACTTCTCCAGAGACCCGTGGCCCAAGGTGTCGGACGCTGCGAAATCTCTTGTTCGTCACATGCTGGAAGCCGATCCCAAGGCTCGCTACACAGCGCACCAGGTACTCGAGCATCCGTGGCTTCAAAACGCGAAGAAAGCTTCCAATACCGCGCTCGGGGATGCGGTGCGAGCAAGGCTGAAGCAATTCTCCGCCATGAACAAGCTCAAGAAGAAAGCTCTGCGGATCATCGCTGAACATTTGTCCGGAGAAGAGATCGAGGGCCTACGGGATATGTTTCAAATGATGGATACGGACAACAGCGGATCTATCACCTTTGAGGAGCTCAAGGCAGGTCTGCAGAAGATCGGGTCACAACTCGCAGAATCAGAGGTGCGGTTGCTGATGGATGCGGCCGATGTGAATAATGACGGTACTCTGGATTACGGGGAGTTCGTGGCTGCGAGTATACACCTACAGAGGATGGACAACGAGGAGCATTTGATGAAGGCATTTGAGCATTTCGACAGGAATGGGAGTGGGTACATCGACATGGATGAGCTCCGAGATGCTCTCGGAGACGACCTGGGGCCAAACGACACAGATGTCATTCAAGACATAATGCATGAGGTGGATACGGATAAAGACGGCCAAATAAGCTACGAGGAATTCGCCTCGATGATGCGCACCGGGACGGACTGGAGGAAAGCTTCTCGCCATTACTCGAGAGAGCTGTTCAACAGTTTGAGCACCAGGTTATTCAGGGATGGATCTCTACAGTCTGGCAACTATTCGGCCCGCGAGAAAAGATGA
>mRNA7722 gene=gene6902
ATGGGGAATTGTTGCGCATGCGGGACGTCGAAACCGCACTCCGGCAACAAGAAACCGAACGCCTACGCACAAGATGGATGCCCTCCCTCCAAGGTCTTGCAATTCAAAGGTGTTAAAGAGGGTGAGAATCTCCACGACAAATATACTCTAGGTGTGGAGCTCGGGCGGGGCGAGTTCGGCATCACCTACTTGTGTAAAGACAACAAAACTAAGGAGCAATTGGCATGCAAGTCCATTTCCAAGCGCAAGCTCCGGACCGCGGTGGATGTGGAAGATGTCCGGCGTGAAGTCGCGGTCATGCACCACCTCCCCCCTCATCCCAACATCGTCAGGCTGAGGGGTGTCTATCAAGACGAAGCCGCAGTGCACCTGGTCATGGAGCTCTGCGAGGGCGGGGAGCTCTTCGACCGCATCATCGCCCGTGGTCACTACAGCGAGCGCGAGGCTGCAGCCGTGACGCGCACCATCGTGGAGGTGGTTCAGGTCTGTCACAAGCACCGGGTGATGCACCGCGACCTCAAGCCGGAGAACTTTCTCTTCGCCAGTAAGGACAATAACTCACCTCTGAAGGCCATAGATTTCGGGCTTTCGTGTTTCTTCTCACCTGGGGAACGCTTCTCGGAGATCGTGGGCAGCCCTTACTACATGGCGCCCGAGGTCCTCAAGAGGAATTATGGTCCGGAGGTGGATGTGTGGAGTGCGGGAGTGATTCTTTACATTCTTCTATGCGGAGTGCCTCCTTTCTGGGCTGAGACCGAGCAAGGAGTGGCGCTTGCTATTCTCCGCGGGGTGCTGGACTTCTCCAGAGACCCGTGGCCCAAGGTGTCGGACGCTGCGAAATCTCTTGTTCGTCACATGCTGGAAGCCGATCCCAAGGCTCGCTACACAGCGCACCAGGTACTCGAGCATCCGTGGCTTCAAAACGCGAAGAAAGCTTCCAATACCGCGCTCGGGGATGCGGTGCGAGCAAGGCTGAAGCAATTCTCCGCCATGAACAAGCTCAAGAAGAAAGCTCTGCGGATCATCGCTGAACATTTGTCCGGAGAAGAGATCGAGGGCCTACGGGATATGTTTCAAATGATGGATACGGACAACAGCGGATCTATCACCTTTGAGGAGCTCAAGGCAGGTCTGCAGAAGATCGGGTCACAACTCGCAGAATCAGAGGTGCGGTTGCTGATGGATGCGGCCGATGTGAATAATGACGGTACTCTGGATTACGGGGAGTTCGTGGCTGCGAGTATACACCTACAGAGGATGGACAACGAGGAGCATTTGATGAAGGCATTTGAGCATTTCGACAGGAATGGGAGTGGGTACATCGACATGGATGAGCTCCGAGATGCTCTCGGAGACGACCTGGGGCCAAACGACACAGATGTCATTCAAGACATAATGCATGAGTCTGGCAACTATTCGGCCCGCGAGAAAAGATGA






# MpUg00160


## aiangst Postdam

Query= Blasia_pusilla_c330513

Length=1023
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

asmbl_84687.p1 GENE.asmbl_84687~~asmbl_84687.p1 ORF type:complete...  948     0.0
TRINITY_DN3550_c2_g1_i50.p1 TRINITY_DN3550_c2_g1~~TRINITY_DN3550_...  948     0.0

>asmbl_84687.p1 GENE.asmbl_84687~~asmbl_84687.p1  ORF type:complete len:654 (+),score=107.37 asmbl_84687:293-2254(+)
ATGATGTGGAATGGTGCAGTGTGGTCTTCTCCAGGGCGGCCCCTGTGGAGTGGCGGAGTGACGAAGAAGGTGCTTGAGGTGGCCAGTAGGATTTGTCAACAGCAGCAATTGTGGGTTGCGTCTGCTTTTGCGAATTGGAGACACTTGACGGGCTCTGTTTCTCCGGTGCGTGCTGCTGCCATGCCTCGGCCCCCGTCCATGGCGACGGCGTCTCGGAGCGATAGTAGCGACTTATCGGATTCGGAGGACGAAGGGAGCGAAGATTATCGAAGAGGCGGTTATCATGTCGTGCGGACTGGAGACACTTTCAAAGGAGGCCGCTATCTGGTGCACCGCAAGCTTGGCTGGGGCCATTTCTCCACCGTGTGGCTGGCGTGGGATAATGTCGACAAGAAATTTGTTGCTCTCAAGGTGCAAAAGAGCGCGCAGCATTACACAGATGCTGCCATCGATGAGATCACCATCTTGAAGCAGGTTGCTGCAGGGGATCCAGCTGACTGTAAAGGAGTTGTTAAGTTACTGGACCATTTCAAGCATACAGGTCCCAATGGCCACCATGTGTGCATGGTGTTTGAGCTCTTAGGGGACAACCTCCTTACTTTGATAAAGCACTATAATTACAGAGGTCTTTCCATTAATATGGTCAAACAACTATCATACCAAATTCTTGTGGGCTTAGATTACCTGCATAGAGAATGCTCCATCATTCACACTGATTTGAAGCCTGAGAATGTCCTACTTCTGTACCCCTTGGACCCCGCCAAAGATCCACGGAAGTCTGGCACCATTCAGTCAACTGGAAGCTTTGGAGGCAGAGAAGCTGCACCCGGTCAACTGGGTGTGGGAGAGGCAAGCCAAGAAGCTGTAACAACTGGTCTTTCAAAGAATAAGAAGAAAAAACTGAAAAGGAGGGCAAAAAAGGCAGGGAACAACACTGATCAAGCACTTGCACAACTCAAGGACTCTGCAAATGCTATAGGAGAGACTAATGTCAGTGTCATGACAGCACAGCCTTCCGTTAAGCCCTCACAAATTGTGGATAATTCAAATGTGAACACAAAAGATATTCAGGAAATCGGTGTCGGAAACACAACAAATGGTGATCGGGTGTCAAATGAAGGGACTGATAAAAGTCATGTAACTACTGCATCCAGCCACCCTGAAGAAAGGGCTGTGGAAAAGCAAGACCTGAACAGAGCTGATGGAGATAGACCACAGTCTAGTCAAGCAGGGCACTTGGATCTGAGATGCAAAATAGTAGACCTAGGAAATGCTTGCTGGACTCGGAAGCAGTTCACCAGTGATATTCAGACTCGCCAGTATCGATGCCCAGAGGTGGTCCTCGGTGCAAAGTATTCTCACTCCGCTGACATATGGTCATTTGCCTGCATTGTGTTTGAGCTTGCAACAGGGGATGTGTTGTTTGACCCAAGGAGCGGTGATGATTTTGGCAGAGATGAGGATCATCTTGCACTCATGATGGAGCTATTGGGCCGCATGCCCCGCAAGATTGCATTGGGTGGTCGGTACTCACGAGATTACTTCAACCGACATGGGGAGTTGAGACACATCCGAAAACTGCGGTATTGGCCTATGGACAGAGTTCTTATTGAGAAGTATGAATTTGCAGAGAGCGATGCCCAAAAGCTTGCAGATTTCCTATGTCCATTACTGGATTTCTACCCAGAGAAGCGGCCTACTGCAGGACAAAGCCTGCAGCATCCCTGGTTGAATCCTGGACCTATGGTCTTGAATCTATCTGTGGGTGCAAAGGATGAAACTTCTTTCGAGACCCATGCAATCATCAATGCCAGTGAAGCTGTACACGCACTCACTAACACAAGTGAGGCTGTGGAAAGGGCTGAGGTAGCTATGGGCAATATTGCCATCAGATGTAACATCAAAGATACAAAACTCCAAGACTCTGCAGACCACAAATCTTGCAGCCAAACTTCTTCATGA
>TRINITY_DN3550_c2_g1_i50.p1 TRINITY_DN3550_c2_g1~~TRINITY_DN3550_c2_g1_i50.p1  ORF type:complete len:594 (-),score=96.48 TRINITY_DN3550_c2_g1_i50:188-1969(-)
ATGCCTCGGCCCCCGTCCATGGCGACGGCGTCTCGGAGCGATAGTAGCGACTTATCGGATTCGGAGGACGAAGGGAGCGAAGATTATCGAAGAGGCGGTTATCATGTCGTGCGGACTGGAGACACTTTCAAAGGAGGCCGCTATCTGGTGCACCGCAAGCTTGGCTGGGGCCATTTCTCCACCGTGTGGCTGGCGTGGGATAATGTCGACAAGAAATTTGTTGCTCTCAAGGTGCAAAAGAGCGCGCAGCATTACACAGATGCTGCCATCGATGAGATCACCATCTTGAAGCAGGTTGCTGCAGGGGATCCAGCTGACTGTAAAGGAGTTGTTAAGTTACTGGACCATTTCAAGCATACAGGTCCCAATGGCCACCATGTGTGCATGGTGTTTGAGCTCTTAGGGGACAACCTCCTTACTTTGATAAAGCACTATAATTACAGAGGTCTTTCCATTAATATGGTCAAACAACTATCATACCAAATTCTTGTGGGCTTAGATTACCTGCATAGAGAATGCTCCATCATTCACACTGATTTGAAGCCTGAGAATGTCCTACTTCTGTACCCCTTGGACCCCGCCAAAGATCCACGGAAGTCTGGCACCATTCAGTCAACTGGAAGCTTTGGAGGCAGAGAAGCTGCACCCGGTCAACTGGGTGTGGGAGAGGCAAGCCAAGAAGCTGTAACAACTGGTCTTTCAAAGAATAAGAAGAAAAAACTGAAAAGGAGGGCAAAAAAGGCAGGGAACAACACTGATCAAGCACTTGCACAACTCAAGGACTCTGCAAATGCTATAGGAGAGACTAATGTCAGTGTCATGACAGCACAGCCTTCCGTTAAGCCCTCACAAATTGTGGATAATTCAAATGTGAACACAAAAGATATTCAGGAAATCGGTGTCGGAAAGACAACAAATGGTGATCGGGTGTCAAATGAAGGGACTGATAAAAGTCATGTAACTACTGCATCCAGCCACCCTGAAGAAAGGGCTGTGGAAAAGCAAGACCTGAACAGAGCTGATGGAGATAGACCACAGTCTAGTCAAGCAGGGCACTTGGATCTGAGATGCAAAATAGTAGACCTAGGAAATGCTTGCTGGACTCGGAAGCAGTTCACCAGTGATATTCAGACTCGCCAGTATCGATGCCCAGAGGTGGTCCTCGGTGCAAAGTATTCTCACTCCGCTGACATATGGTCATTTGCCTGCATTGTGTTTGAGCTTGCAACAGGGGATGTGTTGTTTGACCCAAGGAGCGGTGATGATTTTGGCAGAGATGAGGATCATCTTGCACTCATGATGGAGCTATTGGGCCGCATGCCCCGCAAGATTGCATTGGGTGGTCGGTACTCACGAGATTACTTCAACCGACATGGGGAGTTGAGACACATCCGAAAACTGCGGTATTGGCCTATGGACAGAGTTCTTATTGAGAAGTATGAATTTGCAGAGAGCGATGCCCAAAAGCTTGCAGATTTCCTATGTCCATTACTGGATTTCTACCCAGAGAAGCGGCCTACTGCAGGACAAAGCCTGCAGCATCCCTGGTTGAATCCTGGACCTATGGTCTTGAATCTATCTGTGGGTGCAAAGGATGAAACTTCTTTCGAGACCCATGCAATCATCAATGCCAGTGAAGCTGTACACGCACTCACTAACACAAGTGAGGCTGTGGAAAGGGCTGAGGTAGCTATGGGCAATATTGCCATCAGATGTAACATCAAAGATACAAAACTCCAAGACTCTGCAGACCACAAATCTTGCAGCCAAACTTCTTCATGA




## against Autralia

Query= Blasia_pusilla_c330513

Length=1023
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.13207.1.p1 GENE.STRG.13207.1~~STRG.13207.1.p1 ORF type:compl...  948     0.0
STRG.13205.1.p1 GENE.STRG.13205.1~~STRG.13205.1.p1 ORF type:compl...  420     4e-117


>STRG.13207.1.p1 GENE.STRG.13207.1~~STRG.13207.1.p1  ORF type:complete (-),score=101.27 len:1782 STRG.13207.1:25-1806(-)
ATGCCTCGGCCCCCGTCCATGGCGACGGCGTCTCGGAGCGATAGTAGCGACTTATCGGATTCGGAGGACGAAGGGAGCGAAGATTATCGAAGAGGCGGTTATCATGTCGTGCGGACTGGAGACACTTTCAAAGGAGGCCGCTATCTGGTGCACCGCAAGCTTGGCTGGGGCCATTTCTCCACCGTGTGGCTGGCGTGGGATAATGTCGACAAGAAATTTGTTGCTCTCAAGGTGCAAAAGAGCGCGCAGCATTACACAGATGCTGCCATCGATGAGATCACCATCTTGAAGCAGGTTGCTGCAGGGGATCCAGCTGACTGTAAAGGAGTTGTTAAGTTACTGGACCATTTCAAGCATACAGGTCCCAATGGCCACCATGTGTGCATGGTGTTTGAGCTCTTAGGGGACAACCTCCTTACTTTGATAAAGCACTATAATTACAGAGGTCTTTCCATTAATATGGTCAAACAACTATCATACCAAATTCTTGTGGGCTTAGATTACCTGCATAGAGAATGCTCCATCATTCACACTGATTTGAAGCCTGAGAATGTCCTACTTCTGTACCCCTTGGACCCCGCCAAAGATCCACGGAAGTCTGGCACCATTCAGTCAACTGGAAGCTTTGGAGGCAGAGAAGCTGCACCCGGTCAACTGGGTGTGGGAGAGGCAAGCCAAGAAGCTGTAACAACTGGTCTTTCAAAGAATAAGAAGAAAAAACTGAAAAGGAGGGCAAAAAAGGCAGGGAACAACACTGATCAAGCACTTGCACAACTCAAGGACTCTGCAAATGCTATAGGAGAGACTAATGTCAGTGTCATGACAGCACAGCCTTCCGTTAAGCCCTCACAAATTGTGGATAATTCAAATGTGAACACAAAAGATATTCAGGAAATCGGTGTCGGAAAGACAACAAATGGTGATCGGGTGTCAAATGAAGGGACTGATAAAAGTCATGTAACTACTGCATCCAGCCACCCTGAAGAAAGGGCTGTGGAAAAGCAAGACCTGAACAGAGCTGATGGAGATAGACCACAGTCTAGTCAAGCAGGGCACTTGGATCTGAGATGCAAAATAGTAGACCTAGGAAATGCTTGCTGGACTCGGAAGCAGTTCACCAGTGATATTCAGACTCGCCAGTATCGATGCCCAGAGGTGGTCCTCGGTGCAAAGTATTCTCACTCCGCTGACATATGGTCATTTGCCTGCATTGTGTTTGAGCTTGCAACAGGGGATGTGTTGTTTGACCCAAGGAGCGGTGATGATTTTGGCAGAGATGAGGATCATCTTGCACTCATGATGGAGCTATTGGGCCGCATGCCCCGCAAGATTGCATTGGGTGGTCGGTACTCACGAGATTACTTCAACCGACATGGGGAGTTGAGACACATCCGAAAACTGCGGTATTGGCCTATGGACAGAGTTCTTATTGAGAAGTATGAATTTGCAGAGAGCGATGCCCAAAAGCTTGCAGATTTCCTATGTCCATTACTGGATTTCTACCCAGAGAAGCGGCCTACTGCAGGACAAAGCCTGCAGCATCCCTGGTTGAATCCTGGACCTATGGTCTTGAATCTATCTGTGGGTGCAAAGGATGAAACTTCTTTCGAGACCCATGCAATCATCAATGCCAGTGAAGCTGTACACGCACTCACTAACACAAGTGAGGCTGTGGAAAGGGCTGAGGTAGCTATGGGCAATATTGCCATCAGATGTAACATCAAAGATACAAAACTCCAAGACTCTGCAGACCACAAATCTTGCAGCCAAACTTCTTCATGA
>STRG.13205.1.p1 GENE.STRG.13205.1~~STRG.13205.1.p1  ORF type:complete (+),score=28.99 len:501 STRG.13205.1:3471-3971(+)
ATGACAGCACAGCCTTCCGTTAAGCCCTCACAAATTGTGGATAATTCAAATGTGAACACAAAAGATATTCAGGAAATCGGTGTCGGAAAGACAACAAATGGTGATCGGGTGTCAAATGAAGGGACTGATAAAAGTCATGTAACTACTGCATCCAGCCACCCTGAAGAAAGGGCTGTGGAAAAGCAAGACCTGAACAGAGCTGATGGAGATAGACCACAGTCTAGTCAAGCAGGGCACTTGGATCTGAGATGCAAAATAGTAGACCTAGGAAATGCTTGCTGGACTCGGAAGCAGTTCACCAGTGATATTCAGACTCGCCAGTATCGATGCCCAGAGGTGGTCCTCGGTGCAAAGTATTCTCACTCCGCTGACATATGGTCATTTGCCTGCATTGTGTTTGAGCTTGCAACAGGGGATGTGTTGTTTGACCCAAGGAGCGGTGATGATTTTGGCAGAGATGAGGTATGTGCTTCTGCTTCAAACTTGAATGCTAGTCAATGA


## aiganist Helinsik


Query= Blasia_pusilla_c330513

Length=1023
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

mRNA1242 gene=gene1121                                                948     0.0

>mRNA1242 gene=gene1121
ATGGCGACGGCGTCTCGGAGCGATAGTAGCGACTTATCGGATTCGGAGGACGAAGGGAGCGAAGATTATCGAAGAGGCGGTTATCATGTCGTGCGGACTGGAGACACTTTCAAAGGAGGCCGCTATCTGGTGCACCGCAAGCTTGGCTGGGGCCATTTCTCCACCGTGTGGCTGGCGTGGGATAATGTCGACAAGAAATTTGTTGCTCTCAAGGTGCAAAAGAGCGCGCAGCATTACACAGATGCTGCCATCGATGAGATCACCATCTTGAAGCAGGTTGCTGCAGGGGATCCAGCTGACTGTAAAGGAGTTGTTAAGTTACTGGACCATTTCAAGCATACAGGTCCCAATGGCCACCATGTGTGCATGGTGTTTGAGCTCTTAGGGGACAACCTCCTTACTTTGATAAAGCACTATAATTACAGAGGTCTTTCCATTAATATGGTCAAACAACTATCATACCAAATTCTTGTGGGCTTAGATTACCTGCATAGAGAATGCTCCATCATTCACACTGATTTGAAGCCTGAGAATGTCCTACTTCTGTACCCCTTGGACCCCGCCAAAGATCCACGGAAGTCTGGCACCATTCAGTCAACTGGAAGCTTTGGAGGCAGAGAAGCTGCACCCGGTCAACTGGGTGTGGGAGAGGCAAGCCAAGAAGCTGTAACAACTGGTCTTTCAAAGAATAAGAAGAAAAAACTGAAAAGGAGGGCAAAAAAGGCAGGGAACAACACTGATCAAGCACTTGCACAACTCAAGGACTCTGCAAATGCTATAGGAGAGACTAATGTCAGTGTCATGACAGCACAGCCTTCCGTTAAGCCCTCACAAATTGTGGATAATTCAAATGTGAACACAAAAGATATTCAGGAAATCGGTGTCGGAAACACAACAAATGGTGATCGGGTGTCAAATGAAGGGACTGATAAAAGTCATGTAACTACTGCATCCAGCCACCCTGAAGAAAGGGCTGTGGAAAAGCAAGACCTGAACAGAGCTGATGGAGATAGACCACAGTCTAGTCAAGCAGGGCACTTGGATCTGAGATGCAAAATAGTAGACCTAGGAAATGCTTGCTGGACTCGGAAGCAGTTCACCAGTGATATTCAGACTCGCCAGTATCGATGCCCAGAGGTGGTCCTCGGTGCAAAGTATTCTCACTCCGCTGACATATGGTCATTTGCCTGCATTGTGTTTGAGCTTGCAACAGGGGATGTGTTGTTTGACCCAAGGAGCGGTGATGATTTTGGCAGAGATGAGGATCATCTTGCACTCATGATGGAGCTATTGGGCCGCATGCCCCGCAAGATTGCATTGGGTGGTCGGTACTCACGAGATTACTTCAACCGACATGGGGAGTTGAGACACATCCGAAAACTGCGGTATTGGCCTATGGACAGAGTTCTTATTGAGAAGTATGAATTTGCAGAGAGCGATGCCCAAAAGCTTGCAGATTTCCTATGTCCATTACTGGATTTCTACCCAGAGAAGCGGCCTACTGCAGGACAAAGCCTGCAGCATCCCTGGTTGAATCCTGGACCTATGGTCTTGAATCTATCTGTGGGTGCAAAGGATGAAACTTCTTTCGAGACCCATGCAATCATCAATGCCAGTGAAGCTGTACACGCACTCACTAACACAAGTGAGGCTGTGGAAAGGGCTGAGGTAGCTATGGGCAATATTGCCATCAGATGTAACATCAAAGATACAGAACTCCAAGACTCTGCAGACCACAAATCTTGCAGCCAAACTTCTTCATGA





# MpgU00300

## against Male helsinki

Query= Blasia_pusilla_c333016

Length=2379
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

mRNA19132 gene=gene17019                                              704     0.0


>mRNA19132 gene=gene17019
ATGGAAGTTGATGCAGGTATGGTTGGGGAGAATGATCAGCAGTCGCGGCCGCCTCCGCTGGAACAGCCAGAGCACCAACATCAAAGTGTTGCCCCGAGTGTACCAAGCCCAGCTCCCACCTACAAAATTGTGAATGCCATTCTGGATAAAAAGGAGGACGGCCCCGGGCCTAGATGCGGACATACTCTCACTGCCGTAGCAGCTGTTGGGGATGACGGATCTCCTGGTTACATTGGCCCTCGTCTTATTCTCTTCGGGGGTGCCACCGCCCTTGAGGGCAATTCAAATACCGTTGGCCCCCAAACCTCGTCTGGCGCTGGCATCAGATTAGCAGGGGCCACAGCAGACGTACATTGCTATGATGTCAATTCCAATAAGTGGACGAGGCTGACTCCTGTGGGAGATCCACCATCACCCCGTGCAGCGCACGCTGCAACTGCTGTGGGCACCATGGTTGTCATTCAGGGTGGGATTGGTCCTGCAGGTCTTTCAACGGACGACCTTCATGTACTAGACTTGACACAATCTAGACCCAGATGGCACCGGGTCGTTGTGCAAGGACCAGGTCCAGGTCCACGATATGGTCATGTCATGTCACTAGTTGCACAACGATTTCTTCTGTCTATCAGTGGCAACGATGGTAAGCGTCCCTTGGCTGATGTGTGGGCACTGGACACTGCCGCAAAACCATATGAATGGCGGAAGCTTGAGCCAGAAGGCGATGGTCCACCCCCATGCATGTATGCCACTGCTAGTGCCCGTTCAGATGGTCTGCTTCTCCTTTGTGGTGGGAGGGATGCTAGCAGCGTGCCTTTGGCAAGTGCATATGGACTTGCTAAGCACCGTGATGGAAGATGGGAGTGGGCGCTTGCACCAGGCGTTTCACCTTCTCCAAGATACCAACATGCAGCGGTTTTTGTCAATGCAAGGCTACATGTGTCTGGTGGAGCTTTGGGAGGTGGGCGAATGGTTGAGGATGGCTCGAGTGTTGCAGTCCTTGATACAGCAGCTGGAGTATGGTGTGATCGGAAAGCGGTCGTTACAAGTCCAAGGACGGGAAGATATAGTGCTGATGCTGCTGGTGGAGAGGCGTCAGTAGAGCTGACAAGACGCTGCCGTCATGCAGCTGCTGCTGTGGGTGATCTCATCTTTATGTATGGTGGACTCAGAGGAGGTAAGATTCATAATGCTTAA

## against Postdam


Query= Blasia_pusilla_c333016

Length=2379
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

TRINITY_DN2301_c0_g2_i6.p1 TRINITY_DN2301_c0_g2~~TRINITY_DN2301_c...  2316    0.0
TRINITY_DN2301_c0_g1_i67.p1 TRINITY_DN2301_c0_g1~~TRINITY_DN2301_...  813     0.0

>TRINITY_DN2301_c0_g2_i6.p1 TRINITY_DN2301_c0_g2~~TRINITY_DN2301_c0_g2_i6.p1  ORF type:complete len:986 (-),score=174.98 TRINITY_DN2301_c0_g2_i6:415-3372(-)
ATGGAAGTGGATGCAGGCATGGGCGGGGAGGCTGACCAGCAGCAGGGCCCGCTGCAGCACCAACAACATCATCAGCAACCTCCATCTTCGTTATCAGCACCTAAACCAGCCCCAAGTTACCGAGCCGTGAGTTCAGTAATTGAGAAGAAAGAGGACGGTCCGGGACCCCGGTGCGGTCATACCCTTACGGCGGTAGCAGCTGTCGGAGATGAAGGTTCTCCGGGTTTCATCGGGCCCCGTCTTATTCTCTTCGGTGGTGCCACTGCTCTCGAGGGGAATTCCCACGCTGCCGGTCCCCCTGCTTCTTCTGGCGCAGGCATCAGGTTGGCAGGAGCTACGGCAGATGTGCACTGCTACGATATACAAGCGAACAAGTGGATAAGACTTACACCTGTTGGGGATCCACCATCTCCGCGAGCTGCCCATGCTGCCACAGCTGTGGGAACTATGGTCGTTATTCAGGGTGGCATTGGTCCTGCAGGGCTTTCGACTGATGATCTGCATGTCCTTGACCTCACACAAAGCAGGCCCCGATGGCACAGAGTCGTCGTACAAGGACCTGGACCTGGACCTCGGTATGGTCATGTTATGTCTTTGGTTGCTCAGCGCTTTCTCCTGTCCATAAGTGGCAACGATGGAAAACGACCACTTGCAGATGTGTGGGCGCTAGATACTGCTGCGAAGCCTTATGAGTGGCGCAAATTGGAACCTGAAGGAGACGGTCCTCCACCTTGCATGTATGCAACGGCGAGTGCCCGATCTGATGGTCTGTTACTTCTTTGTGGTGGAAGAGATGCCAGCAGTGTGCCTCTTGCAAGTGCTTATGGGCTTGCCAAACATCGAGATGGGCGGTGGGAGTGGGCAATTGCCCCTGGCGTATCTCCCTCTCCACGTTACCAACATGCTGCTGTCTTTGTGAATGCAAGATTACATGTTTCTGGAGGAGCTCTTGGAGGTGGGCGGATGGTGGAGGATGCGTCTAGTGTTGCAGTCCTGGATACAGCTGCTGGTGTATGGTGTGATAGAAAATCAGTTGTCACCAGTCCTCGAACTGGAAGATACAGTGCTGATGCAGCAGGTGGGGAAGCATCTTGGGAACTGACGAGACGATGCCGACATGCTGCCGCAGCTGTTGGTGATCTTATCTTTATGTATGGTGGACTCAGAGGAGGTGTTTTGTTGGATGACTTGCTTGTAGCAGAGGACTTGGCAGCTGCTGAAACAACCACCGCTGCACTTCAGGCAGCGGCGGCTTCTGCTGCTCTTGCTAGCAATCCCTCTCCTGGATCAGCTTCAATTCCAAGTACAACCGCCATGTCAAACTTTGCTGAAGCCCAAACAAAATTGGGAGATGCATGTCCAGAGGGTGCCGTGGTGATGGGAAGTCCCGTTGCAGCACCCATCAATGGTGACACCTCAATGGATATAAGTACTGAGAATGCTTTTGCCCATGGGCATCGTAATGCTGTCAAAGGAGTGGAGCTACTTGTGAAGGCTTCTGCTGCTGAAGCTGAGGCAATAAGTGCTGCATTAGCAGCAGCAACTGCCCGTCACAACAATGGTGAATCCGAGAAAGAAATTGTTGAAAGAGATCGTGGTGCCGAAGCTTCACCAAGTGGAAAATCTCTCGTACCGTCACCATCAGCAGTAGTTACCAACAAGACTGCAATCATTACTACTACATCAATGCATCCTCCCTCTATGGGTGTCAGGCTGCATCATCGAGCAGTTGTTGTGGCTGCGGAATCAGGGGGTGCTTTAGGGGGATTGGTCAGGCAACTTTCTATTGACCAGTTTGAAAATGAAGGGCGGAGAGTGAGCCATGGAACTCCCGATAGTGCGAGCGCTGCCAGAAGACTTCTAGACAGGCAGATGTCTTTGAGTGGAGTTCAAAAGAAGGTTCTCTCACACCTACTCAAACCTCGAGGCTGGAAGCCTCCAGTCAAGAGACAATTCTTTATGGACTGCAATGAGATCGCTGAACTCTGTGATACAGCAGAGAGATGGTTTGCACGCGAGCCAAGTGTATTACAGATTAAAGCACCAGTCAAAATTTTTGGTGATCTTCACGGTCAATTTGGTGATCTCATGCGACTTTTTGATGAGTATGGATCTCCTTCAACTGCAGGTGACATAACGTATATTGATTATCTTTTCTTGGGTGACTACGTTGATCGTGGACAACACAGCTTGGAAACTATCACCCTGCTTCTTGCACTTAAGATAGAGTACCCCAACAATGTCCATCTTATACGAGGAAATCATGAGGCTGCTGACATCAATGCTTTGTTTGGCTTCCGCATTGAGTGCATTGAGCGAATGGGTGAGCGGGATGGAATTTGGGCCTGGCAACGCATCAATCAGCTATTCAATTGGCTCCCTCTAGCAGCTCTCATAGAAAAGAAAATCATATGTATGCATGGTGGAATTGGAAGATCCATAAATCATGTTGATCAGATTGAAGCTCTGCAGCGGCCAATAACCATGGAGGCAGGATCGGTTGTCCTTATGGATCTCCTCTGGTCTGACCCAACAGAAAATGACAGCGTAGAAGGGTTACGCCCGAATGCTCGAGGCCCTGGTCTTGTCACTTTTGGACCAGACCGAGTGATGGAATTCTGTAAAAACAACGATTTGCAGCTCATTGTCAGGGCACATGAATGTGTAATGGATGGGTTTGAACGCTTTGCTCAGGGTCATTTAATAACTCTTTTTTCTGCCACAAACTACTGTGGTACTGCAAATAATGCTGGAGCAATTTTAGTCTTGGGAAGGGACCTAGTGGTTGTGCCAAAGCTTATACACCCTATCCCACCGGCGATTACCTCTCCCGAATCATCTCCAGAACGACATCTGGAAGATACCTGGATGCAGGAGCTGAATGTGCAAAGGCCGCCCACTCCAACTAGGGGCAGGCCTCAAGCTTCCAGTGATCGTGGATCATTGGCGTGGATCTGA


## against australia


Query= Blasia_pusilla_c333016

Length=2379
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.12286.1.p1 GENE.STRG.12286.1~~STRG.12286.1.p1 ORF type:compl...  2316    0.0
STRG.12282.1.p1 GENE.STRG.12282.1~~STRG.12282.1.p1 ORF type:compl...  520     9e-147
STRG.12283.1.p1 GENE.STRG.12283.1~~STRG.12283.1.p1 ORF type:compl...  477     5e-134

>STRG.12286.1.p1 GENE.STRG.12286.1~~STRG.12286.1.p1  ORF type:complete (-),score=161.18 len:2511 STRG.12286.1:299-2809(-)
ATGGTCGTTATTCAGGGTGGCATTGGTCCTGCAGGGCTTTCGACTGATGATCTGCATGTCCTTGACCTCACACAAAGCAGGCCCCGATGGCACAGAGTCGTCGTACAAGGACCTGGACCTGGACCTCGGTATGGTCATGTTATGTCTTTGGTTGCTCAGCGCTTTCTCCTGTCCATAAGTGGCAACGATGGAAAACGACCACTTGCAGATGTGTGGGCGCTAGATACTGCTGCGAAGCCTTATGAGTGGCGCAAATTGGAACCTGAAGGAGACGGTCCTCCACCTTGCATGTATGCAACGGCGAGTGCCCGATCTGATGGTCTGTTACTTCTTTGTGGTGGAAGAGATGCCAGCAGTGTGCCTCTTGCAAGTGCTTATGGGCTTGCCAAACATCGAGATGGGCGGTGGGAGTGGGCAATTGCCCCTGGCGTATCTCCCTCTCCACGTTACCAACATGCTGCTGTCTTTGTGAATGCAAGATTACATGTTTCTGGAGGAGCTCTTGGAGGTGGGCGGATGGTGGAGGATGCGTCTAGTGTTGCAGTCCTGGATACAGCTGCTGGTGTATGGTGTGATAGAAAATCAGTTGTCACCAGTCCTCGAACTGGAAGATACAGTGCTGATGCAGCAGGTGGGGAAGCATCTTGGGAACTGACGAGACGATGCCGACATGCTGCCGCAGCTGTTGGTGATCTTATCTTTATGTATGGTGGACTCAGAGGAGGTGTTTTGTTGGATGACTTGCTTGTAGCAGAGGACTTGGCAGCTGCTGAAACAACCACCGCTGCACTTCAGGCAGCGGCGGCTTCTGCTGCTCTTGCTAGCAATCCCTCTCCTGGATCAGCTTCAATTCCAAGTACAACCGCCATGTCAAACTTTGCTGAAGCCCAAACAAAATTGGGAGATGCATGTCCAGAGGGTGCCGTGGTGATGGGAAGTCCCGTTGCAGCACCCATCAATGGTGACACCTCAATGGATATAAGTACTGAGAATGCTTTTGCCCATGGGCATCGTAATGCTGTCAAAGGAGTGGAGCTACTTGTGAAGGCTTCTGCTGCTGAAGCTGAGGCAATAAGTGCTGCATTAGCAGCAGCAACTGCCCGTCACAACAATGGTGAATCCGAGAAAGAAATTGTTGAAAGAGATCGTGGTGCCGAAGCTTCACCAAGTGGAAAATCTCTCGTACCGTCACCATCAGCAGTAGTTACCAACAAGACTGCAATCATTACTACTACATCAATGCATCCTCCCTCTATGGGTGTCAGGCTGCATCATCGAGCAGTTGTTGTGGCTGCGGAATCAGGGGGTGCTTTAGGGGGATTGGTCAGGCAACTTTCTATTGACCAGTTTGAAAATGAAGGGCGGAGAGTGAGCCATGGAACTCCCGATAGTGCGAGCGCTGCCAGAAGACTTCTAGACAGGCAGATGTCTTTGAGTGGAGTTCAAAAGAAGGTTCTCTCACACCTACTCAAACCTCGAGGCTGGAAGCCTCCAGTCAAGAGACAATTCTTTATGGACTGCAATGAGATCGCTGAACTCTGTGATACAGCAGAGAGATGGTTTGCACGCGAGCCAAGTGTATTACAGATTAAAGCACCAGTCAAAATTTTTGGTGATCTTCACGGTCAATTTGGTGATCTCATGCGACTTTTTGATGAGTATGGATCTCCTTCAACTGCAGGTGACATAACGTATATTGATTATCTTTTCTTGGGTGACTACGTTGATCGTGGACAACACAGCTTGGAAACTATCACCCTGCTTCTTGCACTTAAGATAGAGTACCCCAACAATGTCCATCTTATACGAGGAAATCATGAGGCTGCTGACATCAATGCTTTGTTTGGCTTCCGCATTGAGTGCATTGAGCGAATGGGTGAGCGGGATGGAATTTGGGCCTGGCAACGCATCAATCAGCTATTCAATTGGCTCCCTCTAGCAGCTCTCATAGAAAAGAAAATCATATGTATGCATGGTGGAATTGGAAGATCCATAAATCATGTTGATCAGATTGAAGCTCTGCAGCGGCCAATAACCATGGAGGCAGGATCGGTTGTCCTTATGGATCTCCTCTGGTCTGACCCAACAGAAAATGACAGCGTAGAAGGGTTACGCCCGAATGCTCGAGGCCCTGGTCTTGTCACTTTTGGACCAGACCGAGTGATGGAATTCTGTAAAAACAACGATTTGCAGCTCATTGTCAGGGCACATGAATGTGTAATGGATGGGTTTGAACGCTTTGCTCAGGGTCATTTAATAACTCTTTTTTCTGCCACAAACTACTGTGGTACTGCAAATAATGCTGGAGCAATTTTAGTCTTGGGAAGGGACCTAGTGGTTGTGCCAAAGCTTATACACCCTATCCCACCGGCGATTACCTCTCCCGAATCATCTCCAGAACGACATCTGGAAGATACCTGGATGCAGGAGCTGAATGTGCAAAGGCCGCCCACTCCAACTAGGGGCAGGCCTCAAGCTTCCAGTGATCGTGGATCATTGGCGTGGATCTGA






# MpgU00340

## againast Helinki

Query= Blasia_pusilla_c334273

Length=1026
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

mRNA19123 gene=gene17011                                              363     1e-99
mRNA19124 gene=gene17012                                              318     2e-86

>mRNA19123 gene=gene17011
ATGATTGCAAATTATGTGTTTGTTCAGACGCGTCTGTGTGTGCCATCAAGTAGTGGAGTTGTTGAGCTGGGCTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTTTTTGCTGAGATGAACGAGATTAGCGGAAACTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGGCTTGCCCAGCAAGCCCCTCTCTGGTCAGCATTCATGGAACAAGAAGGCATGAGTCCTTGGACAGGGGACCTGATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTAGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTCGCTGATGTTTCTATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATTTCTGGATCATTCCGTGGAAGAAGCTCTGGTCAAGTAGACAAGGTTGACTCTATTGGAGTGGTGAAGAAAGCTCAGCCTGGTGTTCAAGCTCAAAATCAAAGATGTGGAATAGAAGGTGAAATTTTAAATCCACCACCAGATATGAGACAAGGCAGAAGGGAGGTGTTGCATCCGAATAGTGTGTCAACAACTCACATCAAAGAAGAAGGGCCAAGGACATCAAAGCACAAGGTCTCGCCATCTATGAAGATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAGTGCTTATGTCAGATCAAGTATTGAGTCTGAACAAGACTTGGATTCAGAGGCAGAAGTTTCATACAAAGAAGTTGAGTGCAGTTTCACTGTTGGACAAAAGCCACCTAGAAAACGTGGAAGAAAGCCTGCGAATGACAGGGAAGAACCTTTAAATCATGTACAGGCTGAACGACAAAGACGAGAGAAACTCAACCAGCGGTTCTATGCCTTGCGCTCTGTCGTTCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTACAGGATATGGAGTTTCAAATCAAGGAACTCGAGGGTCAAGCAAAATTGCTGAGAGAAAAATCATCTGAGACAGAGATTGCAAGAGGAGGCATGACTAAGTTGTCTAGAGACATTTCACATCCAAAAACCTTTTCTGGTGGAAGCTCGATGACTTCTGGAGTCAAGATTTCTACGGATCAAAGACCAATGGTGTCTGTGCATGTCTTACGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCTGGAATCGCAAACTTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGCATGCCAATCTATCAATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAGTAAGTAGTCCCTATTCTTAG


## against Potsdam

Query= Blasia_pusilla_c334273

Length=1026
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

TRINITY_DN1489_c0_g2_i3.p1 TRINITY_DN1489_c0_g2~~TRINITY_DN1489_c...  579     1e-164
TRINITY_DN9772_c2_g1_i1.p1 TRINITY_DN9772_c2_g1~~TRINITY_DN9772_c...  106     3e-22

>TRINITY_DN1489_c0_g2_i3.p1 TRINITY_DN1489_c0_g2~~TRINITY_DN1489_c0_g2_i3.p1  ORF type:complete len:696 (+),score=102.21 TRINITY_DN1489_c0_g2_i3:202-2289(+)
ATGGGAACGGATGGAGGTATTCGAAGCTTATGGGACGAGGATAATGCGATGATAGAAGCATTCATGGGATCGGCTGTGTATAACCTATCTTGTAGCTCTCAAGTGGATTTCCCGGGAGCAGAAAGACCTGCTTACAGCGAGACCGCGTTACAACAAAAACTCCAGCTTTTGGTTGAGTCATCGACTGTGAATTGGACCTACGCAATCTTTTGGCAGATTTCGAATTCGTCGAGTGGAGAGATGATGCTAGGTTGGGGTGATGGTTACTTTAAGGGACCAAAAGATAGTGAGGTGAATGATCTGAATCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTAAGGCGAAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGAAGCTGGAGCTTCAGCCGGGCTTGATTATGTAACAGATACAGAGTGGTTCTACCTTGTTTCGATGTCATGTTTCTTTCCTCCTGGTGTTGGGATTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGGCTAATGGAAGCTAACAAGGCTCCTAGTAATATTTGCACACAAGCCGATCTTGCTAAGAAGGCAGGGATCCAGACGCGTTTGTGTGTACCATCAACTAGTGGAGTTGTTGAGCTGGGGTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTGTTTGCTGAGATGAACGAGATCAGCGGAAATTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGCCTTGCCCAGCAAGTCCCTCTCTGGTCAGCATTCATGGATCAAGAAGGCATGAATCCTTGGACAGGGGACCTGATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTGGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTTGCTGATGTTTCCATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATCTCTGGACCATATCGTGGAAGAAACTCTGGTCAAGTAGACAAGGTCGACTCTATGGTAGTGGTGAAGAAAGCTCAGCCTGGTGTCCAAGCTCAAAATCAAAGGTGTGGAATTGAGGGTGAAATTTTAAATCCACCACCCGATATGAGACAGGGCAGAGGGGAGGTGTTGCATCTGAACAGTGTTTCAACAACTCACATCAAAGAGGAAGGGCCTAGGTCATCAAAGCAAAAGGTCTCTCCATCTATGAAAATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAGTGCTTACGTCCGATCAAGTATTGAGTCTGAACAAGACCTGGATTCAGAGGCAGAAGTTTCATACAAAGAAGTAGAGTGCAGTTTCACTGTTGGACAAAAGCCACCGCGAAAACGTGGAAGAAAGCCTGCAAATGACAGGGAAGAACCTTTAAACCATGTACAGGCTGAACGACAAAGACGAGAGAAGCTCAACCAGCGATTCTACGCCTTGCGCTCTGTTGTGCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTGCAGGATATGGAGTTTCAAATCAAGGAACTCGAGGCTCAAGCTAAATTGCTGACAGAAAAATCATCTGAGACAGAGGTTGGAAGAGGAGGCATGAACAAGTTGTCTAATGAGATTTTACATCCCAAAACCTTTTCTGGCAGAAGCTCAATGACTTCTGGAGTCAAGATTTCTACGGATCAGAGACCAATGGTATCTGTGCATGTCTTGCGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCCGGAATGGCAAATTTGATGTGGACTTTGGAAGATTTAAAGCTGCAGGTTCAGCACGCCAACCTATCAATTCATGCGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAATGCACTATCAGAAGCTCAGTTGATTAGTGCTATCGAAAAGTCATTTGAGGTAGGTGATGTGACTTCAATGAGGACAAATCATGATCCATACCTTTTAGAACTCAAACCTGACAATGGCAAATGTAGCATTCATTGA


## against Asutralia

Query= Blasia_pusilla_c334273

Length=1026
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.11460.1.p1 GENE.STRG.11460.1~~STRG.11460.1.p1 ORF type:compl...  466     5e-131

>STRG.11460.1.p1 GENE.STRG.11460.1~~STRG.11460.1.p1  ORF type:complete (+),score=95.20 len:1515 STRG.11460.1:4773-6287(+)
ATGATTGCAAATTGTGTCTTTGTTCAGACGCGTTTGTGTGTACCATCAACTAGTGGAGTTGTTGAGCTGGGGTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTGTTTGCTGAGATGAACGAGATCAGCGGAAATTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGCCTTGCCCAGCAAGTCCCTCTCTGGTCAGCATTCATGGATCAAGAAGGCATGAATCCTTGGACAGGGGACCTGATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTGGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTTGCTGATGTTTCCATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATCTCTGGACCATATCGTGGAAGAAACTCTGGTCAAGTAGACAAGGTCGACTCTATGGTAGTGGTGAAGAAAGCTCAGCCTGGTGTCCAAGCTCAAAATCAAAGGTGTGGAATTGAGGGTGAAATTTTAAATCCACCACCCGATATGAGACAGGGCAGAGGGGAGGTGTTGCATCTGAACAGTGTTTCAACAACTCACATCAAAGAGGAAGGGCCTAGGTCATCAAAGCAAAAGGTCTCTCCATCTATGAAAATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAGTGCTTACGTCCGATCAAGTATTGAGTCTGAACAAGACCTGGATTCAGAGGCAGAAGTTTCATACAAAGAAGTAGAGTGCAGTTTCACTGTTGGACAAAAGCCACCGCGAAAACGTGGAAGAAAGCCTGCAAATGACAGGGAAGAACCTTTAAACCATGTACAGGCTGAACGACAAAGACGAGAGAAGCTCAACCAGCGATTCTACGCCTTGCGCTCTGTTGTGCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTGCAGGATATGGAGTTTCAAATCAAGGAACTCGAGGCTCAAGCTAAATTGCTGACAGAAAAATCATCTGAGACAGAGGTTGGAAGAGGAGGCATGAACAAGTTGTCTAATGAGATTTTACATCCCAAAACCTTTTCTGGCAGAAGCTCAATGACTTCTGGAGTCAAGATTTCTACGGATCAGAGACCAATGGTATCTGTGCATGTCTTGCGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCCGGAATGGCAAATTTGATGTGGACTTTGGAAGATTTAAAGCTGCAGGTTCAGCACGCCAACCTATCAATTCATGCGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAATGCACTATCAGAAGCTCAGTTGATTAGTGCTATCGAAAAGTCATTTGAGGTAGGTGATGTGACTTCAATGAGGACAAATCATGATCCATACCTTTTAGAACTCAAACCTGACAATGGCAAATGTAGCATTCATTGA



# MpUg00350


## aganst Helsinki malie

Query= Blasia_pusilla_c353810

Length=606


***** No hits found *****



Lambda      K        H
    1.33    0.621     1.12

Gapped
Lambda      K        H
    1.28    0.460    0.850

Effective search space used: 12006431856


  Database: Blasia_Male_Helsinki_primary_cds.fasta
    Posted date:  Mar 18, 2024  12:26 PM
  Number of letters in database: 21,038,712
  Number of sequences in database:  17,046



Matrix: blastn matrix 1 -2
Gap Penalties: Existence: 0, Extension: 2.5




## aiagnts Potsdam female

Query= Blasia_pusilla_c353810

Length=606
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

TRINITY_DN8004_c0_g1_i1.p1 TRINITY_DN8004_c0_g1~~TRINITY_DN8004_c...  1081    0.0
TRINITY_DN8004_c0_g2_i1.p1 TRINITY_DN8004_c0_g2~~TRINITY_DN8004_c...  704     0.0

>TRINITY_DN8004_c0_g1_i1.p1 TRINITY_DN8004_c0_g1~~TRINITY_DN8004_c0_g1_i1.p1  ORF type:complete len:218 (-),score=21.80 TRINITY_DN8004_c0_g1_i1:1007-1660(-)
ATGGCTTATAGATCGGACGATGATTATGACTATTTGTTCAAAGTGGTTCTTATTGGAGACTCTGGAGTAGGTAAATCAAATCTGTTGTCTAGATTTACCCGGAATGAGTTCAGTCTGGAATCGAAATCAACTATTGGCGTGGAGTTTGCTACTCGTAGCATCAATGTGGACAGCAAACTCATCAAGGCGCAGATATGGGACACTGCAGGTCAAGAAAGGTACAGGGCAATCACAAGTGCGTATTATCGTGGGGCTGTCGGAGCTCTACTGGTCTATGATATTACACGGCACGTAACCTTCGAGAATGTGGAGAGATGGCTAAAAGAACTCAAGGATCACACTGATTCTAATATTGTGGTTATGCTAGTTGGTAACAAGTCAGATCTGCGACATCTAAGAGCCGTTTCAACTGACGATGGTCAATCCTTTTCGGAGAAGGAAGGTCTTTTCTTCATGGAGACATCTGCGCTTGAATCTACGAATGTTGAAAATGCCTTCAAGCAGATACTGACGCAGATCTACCGGGTGGTAAGTAAAAAGGCTCTGGACGTTGGTGACGATCCATCTGCTGGCCCTGGGAAGGGTCAAACTATTACAGTCGGCAACAAAGATGAAGTTACAGCCACCAAAAAAGTAGGATGCTGCTCAGCCTGA



## aigainst Australia

Query= Blasia_pusilla_c353810

Length=606
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.15548.1.p1 GENE.STRG.15548.1~~STRG.15548.1.p1 ORF type:compl...  1081    0.0
STRG.15549.1.p1 GENE.STRG.15549.1~~STRG.15549.1.p1 ORF type:compl...  704     0.0

>STRG.15548.1.p1 GENE.STRG.15548.1~~STRG.15548.1.p1  ORF type:complete (-),score=25.72 len:654 STRG.15548.1:950-1603(-)
ATGGCTTATAGATCGGACGATGATTATGACTATTTGTTCAAAGTGGTTCTTATTGGAGACTCTGGAGTAGGTAAATCAAATCTGTTGTCTAGATTTACCCGGAATGAGTTCAGTCTGGAATCGAAATCAACTATTGGCGTGGAGTTTGCTACTCGTAGCATCAATGTGGACAGCAAACTCATCAAGGCGCAGATATGGGACACTGCAGGTCAAGAAAGGTACAGGGCAATCACAAGTGCGTATTATCGTGGGGCTGTCGGAGCTCTACTGGTCTATGATATTACACGGCACGTAACCTTCGAGAATGTGGAGAGATGGCTAAAAGAACTCAAGGATCACACTGATTCTAATATTGTGGTTATGCTAGTTGGTAACAAGTCAGATCTGCGACATCTAAGAGCCGTTTCAACTGACGATGGTCAATCCTTTTCGGAGAAGGAAGGTCTTTTCTTCATGGAGACATCTGCGCTTGAATCTACGAATGTTGAAAATGCCTTCAAGCAGATACTGACGCAGATCTACCGGGTGGTAAGTAAAAAGGCTCTGGACGTTGGTGACGATCCATCTGCTGGCCCTGGGAAGGGTCAAACTATTACAGTCGGCAACAAAGATGAAGTTACAGCCACCAAAAAAGTAGGATGCTGCTCAGCCTGA




TRIED AGAINST GENOME

/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model est2genome --showtargetgff --percent 50 Blasiaseq_from_MpgU00350_Australia.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta

Hostname: [bio226-oct23-ubuntu22]

C4 Alignment:
------------
         Query: TRINITY_DN8004_c0_g1_i1.p1 TRINITY_DN8004_c0_g1~~TRINITY_DN8004_c0_g1_i1.p1  ORF type:complete len:218 (-),score=21.80 TRINITY_DN8004_c0_g1_i1:1007-1660(-)
        Target: HiC_scaffold_9
         Model: est2genome
     Raw score: 1979
   Query range: 0 -> 652
  Target range: 26880468 -> 26882254

        1 : ATGGCTTATAGATCGGACGATGATTATGACTATTTGTTCAAAGTGGTTCTTATT :       54
            ||||| || || || || || |||||||| ||| | ||||| |||||  | ||
 26880469 : ATGGCGTACAGGTCAGATGACGATTATGATTATCTTTTCAAGGTGGTGTTGATC : 26880522

       55 : GGAGACTCTGGAGTAGGTAAATCAAATCTGTTGTCTAGATTTACCCGGAATGAG :      108
            || || ||||| || || |||||||||||  | || ||||| ||||| || |||
 26880523 : GGCGATTCTGGTGTTGGAAAATCAAATCTTCTTTCCAGATTCACCCGCAACGAG : 26880576

      109 : TTCAGTCTGGAATCGAAATCAACTATTGGCGTGGAGTTTGCTACTCGTAGCATC :      162
            ||||| || || || ||||| || || || ||||||||||| ||  | |||||
 26880577 : TTCAGCCTAGAGTCAAAATCGACCATCGGGGTGGAGTTTGCGACGAGGAGCATA : 26880630

      163 : AATGTGGACAGCAAACTCATCAAGGCGCAGATATGGGACACTGCAGGTCAAGAA :      216
            ||||| |||||||| || |||||||| ||||||||||| ||||| |||||||||
 26880631 : AATGTTGACAGCAAGCTGATCAAGGCACAGATATGGGATACTGCTGGTCAAGAA : 26880684

      217 : AG  >>>> Target Intron 1 >>>>  GTACAGGGCAATCACAAGTGCGT :      241
            ||++         1134 bp         ++|||| |||| || |||||||| |
 26880685 : AGgt.........................agGTACCGGGCTATTACAAGTGCCT : 26881843

      242 : ATTATCGTGGGGCTGTCGGAGCTCTACTGGTCTATGATATTACACGGCACGTAA :      295
            ||||  | || || || |||||  |  | || || || || || || || || |
 26881844 : ATTACAGAGGAGCAGTTGGAGCATTGTTAGTGTACGACATAACTCGACATGTGA : 26881897

      296 : CCTTCGAGAATGTGGAGAGATGGCTAAAAGAACTCAAGGATCACACTGATTCTA :      349
            | || || || ||||| |||||| | ||||| |||||||||||||| ||||| |
 26881898 : CTTTTGAAAACGTGGAAAGATGGTTGAAAGAGCTCAAGGATCACACCGATTCCA : 26881951

      350 : ATATTGTGGTTATGCTAGTTGGTAACAAGTCAGATCTGCGACATCTAAGAGCCG :      403
            | ||||| || ||| | ||||| |||||||| || ||||| ||| ||||||| |
 26881952 : ACATTGTCGTGATGTTGGTTGGGAACAAGTCTGACCTGCGCCATTTAAGAGCTG : 26882005

      404 : TTTCAACTGACGATGGTCAATCCTTTTCGGAGAAGGAAGGTCTTTTCTTCATGG :      457
            ||||  |||| ||||||||||| || || |||||||| || |||||||||||||
 26882006 : TTTCTGCTGATGATGGTCAATCATTCTCTGAGAAGGAGGGCCTTTTCTTCATGG : 26882059

      458 : AGACATCTGCGCTTGAATCTACGAATGTTGAAAATGCCTTCAAGCAGATACTGA :      511
            |||| || || |||||||| || ||||| || ||||| ||||| |||||| |||
 26882060 : AGACCTCAGCTCTTGAATCAACAAATGTGGAGAATGCTTTCAAACAGATATTGA : 26882113

      512 : CGCAGATCTACCGGGTGGTAAGTAAAAAGGCTCTGGACGTTGGTGACGATCCAT :      565
            | ||||| |||||||| || || || ||||| ||||| ||||| || |||||||
 26882114 : CTCAGATATACCGGGTTGTGAGCAAGAAGGCACTGGATGTTGGAGATGATCCAT : 26882167

      566 : CTGCTGGCCCTGGGAAGGGTCAAACTATTACAGTCGGCAACAAAGATGAAGTTA :      619
            |  |||  || |||||||| ||||| || |  || || ||||| ||||| ||||
 26882168 : CAACTGTGCCAGGGAAGGGGCAAACCATAAGTGTTGGAAACAAGGATGATGTTA : 26882221

      620 : CAGCCACCAAAAAAGTAGGATGCTGCTCAGCCT :      652
            | || || ||||| || || ||||| || || |
 26882222 : CTGCAACAAAAAAGGTTGGGTGCTGTTCTGCAT : 26882254

vulgar: TRINITY_DN8004_c0_g1_i1.p1 0 652 + HiC_scaffold_9 26880468 26882254 + 1979 M 218 218 5 0 2 I 0 1130 3 0 2 M 434 434
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:est2genome 2.2.0
##date 2024-05-14
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:est2genome    gene    26880469        26882254        1979    +       .       gene_id 1 ; sequence TRINITY_DN8004_c0_g1_i1.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:est2genome    utr5    26880469        26880686        .       +       .
HiC_scaffold_9  exonerate:est2genome    exon    26880469        26880686        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 26880687        26880688        .       +       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  26880687        26881820        .       +       .       intron_id 1
HiC_scaffold_9  exonerate:est2genome    splice3 26881819        26881820        .       +       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    exon    26881821        26882254        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    similarity      26880469        26882254        1979    +       .       alignment_id 1 ; Query TRINITY_DN8004_c0_g1_i1.p1 ; Align 26880469 1 218 ; Align 26881821 219 434
# --- END OF GFF DUMP ---
#
-- completed exonerate analysis




/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 50 --ryo ">%qi\n%tcs\n" Blasiaseq_from_MpgU00350_Australia.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta

Hostname: [bio226-oct23-ubuntu22]

C4 Alignment:
------------
         Query: TRINITY_DN8004_c0_g1_i1.p1 TRINITY_DN8004_c0_g1~~TRINITY_DN8004_c0_g1_i1.p1  ORF type:complete len:218 (-),score=21.80 TRINITY_DN8004_c0_g1_i1:1007-1660(-)
        Target: HiC_scaffold_6 [revcomp]
         Model: coding2genome
     Raw score: 653
   Query range: 15 -> 654
  Target range: 10102692 -> 10100960

       16 : GACGATGATTATGACTATTTGTTCAAAGTGGTTCTTATTGGAGACTCTGGAGTA :       67
            AspAspAspTyrAspTyrLeuPheLysValValLeuIleGlyAspSerGlyVal
            ||+ !  !    |||||+:!:||+||+||||||||+||||||||||||!.!|||
            AspHisLysIleAspTyrValPheLysValValLeuIleGlyAspSerAlaVal
 10102692 : GATCACAAAATAGACTACGTTTTTAAGGTGGTTCTGATTGGAGACTCTGCAGTA : 10102641

       68 : GGTAAATCAAATCTGTTGTCTAGATTTACCCGGAATGAGTTCAGTCTGGAATCG :      121
            GlyLysSerAsnLeuLeuSerArgPheThrArgAsnGluPheSerLeuGluSer
            ||+||+|||.!.|||+|+:!:+|||||:!:||||||||+||||||||+||+|||
            GlyLysSerGlnLeuLeuAlaArgPheSerArgAsnGluPheSerLeuGluSer
 10102640 : GGGAAGTCACAGCTGCTTGCGCGATTTTCTCGGAATGAATTCAGTCTAGAGTCG : 10102587

      122 : AAATCAACTATTGGCGTGGAGTTTGCTACTCGTAGCATCAATGTGGACAGCAAA :      175
            LysSerThrIleGlyValGluPheAlaThrArgSerIleAsnValAspSerLys
            ||+:!:||+|||||+||||||||+   |||||+!::||+   ||+.!....||+
            LysAlaThrIleGlyValGluPheGlnThrArgThrIleValValGlnGlnLys
 10102586 : AAGGCCACCATTGGTGTGGAGTTCCAAACTCGAACTATTGTCGTTCAGCAGAAG : 10102533

      176 : CTCATCAAGGCGCAGATATGGGACACTGCAGGTCAAGAA{AG}  >>>> Targ :      219
            LeuIleLysAlaGlnIleTrpAspThrAlaGlyGlnGlu{Ar}
               |||||+||+||+||+|||||+||+||+|||||+||+{||}
            ThrIleLysAlaGlnIleTrpAspThrAlaGlyGlnGlu{Ar}++
 10102532 : ACAATCAAAGCTCAAATTTGGGATACAGCTGGTCAGGAG{AG}gt......... : 10102487

      220 : et Intron 1 >>>>  {G}TACAGGGCAATCACAAGTGCGTATTATCGTGGG :      250
                              {g}TyrArgAlaIleThrSerAlaTyrTyrArgGly
            1084 bp           {|}||+|||||+:!:||+|||||+||+||+||+||+
                            ++{g}TyrArgAlaValThrSerAlaTyrTyrArgGly
 10102486 : ................ag{G}TATAGGGCTGTAACTAGTGCCTACTACCGAGGA : 10101374

      251 : GCTGTCGGAGCTCTACTGGTCTATGATATTACACGGCACGTAACCTTCGAGAAT :      304
            AlaValGlyAlaLeuLeuValTyrAspIleThrArgHisValThrPheGluAsn
            |||||+||+||+:!:|||||+|||||+||+||+:::!..  !:!:||+!!::!!
            AlaValGlyAlaMetLeuValTyrAspIleThrLysArgGlnSerPheAspHis
 10101373 : GCTGTAGGGGCCATGCTGGTTTATGACATCACGAAACGGCAATCATTTGATCAT : 10101320

      305 : GTGGAGAGATGGCTAAAAGAACTCAAGGATCACACTGATTCTAATATTGTGGTT :      358
            ValGluArgTrpLeuLysGluLeuLysAspHisThrAspSerAsnIleValVal
            |||.!.+|+|||||+:!:|||+|+|||!  ||+.!.|||:!:||||||||+:!:
            ValHisArgTrpLeuGluGluLeuLysAlaHisAlaAspAlaAsnIleValIle
 10101319 : GTGCACCGGTGGCTCGAGGAATTGAAGGCACATGCAGATGCCAATATTGTTATA : 10101266

      359 : ATGCTAGTTGGTAACAAGTCAGATCTGCGACATCTAAGAGCCGTTTCAACTGAC :      412
            MetLeuValGlyAsnLysSerAspLeuArgHisLeuArgAlaValSerThrAsp
            |||+|+:!:||+||||||||||||||| !!   ||+|||   ||| !!|||!!:
            MetLeuIleGlyAsnLysSerAspLeuGlyAlaLeuArgGlnValProThrGlu
 10101265 : ATGTTGATAGGGAACAAGTCAGATCTGGGAGCGCTTAGACAAGTTCCAACTGAA : 10101212

      413 : GATGGTCAATCCTTTTCGGAGAAGGAAGGTCTTTTCTTCATGGAGACATCTGCG :      466
            AspGlyGlnSerPheSerGluLysGluGlyLeuPhePheMetGluThrSerAla
            ||+!..:!:...!:!:!:||+::!||+||+||+||+|||:!:||+||+|||||+
            AspAlaLysGluTyrAlaGluArgGluGlyLeuPhePheLeuGluThrSerAla
 10101211 : GACGCCAAGGAGTATGCCGAACGGGAGGGACTCTTTTTCCTTGAAACTTCTGCC : 10101158

      467 : CTTGAATCTACGAATGTTGAAAATGCCTTCAAGCAGATACTGACGCAGATCTAC :      520
            LeuGluSerThrAsnValGluAsnAlaPheLysGlnIleLeuThrGlnIleTyr
            :!:!!:|||||+||||||||+   ||+|||      :!:||+:!::!:||+|||
            MetAspSerThrAsnValGluAlaAlaPhePheThrValLeuSerGluIleTyr
 10101157 : ATGGACTCTACAAATGTTGAGGCAGCTTTCTTCACTGTGCTCTCCGAAATTTAC : 10101104

      521 : CGGGTGGTAAGTAAAAAGGCTCTG<-><-><->GACGTTGGTGACGATCCATCT :      565
            ArgValValSerLysLysAlaLeu<-><-><->AspValGlyAspAspProSer
            +||:!:||+||+||+|||||+|||         !!:   ....!.  !   ...
            ArgIleValSerLysLysAlaLeuIleAlaAspGluLysSerGlnThrAsnGly
 10101103 : AGGATCGTGAGCAAGAAGGCCCTGATTGCAGATGAGAAGTCGCAAACTAATGGG : 10101050

      566 : GCTGGCCCTGGGAAGGGTCAAACTATTACAGTCGGCAACAAAGATGAAGTTACA :      619
            AlaGlyProGlyLysGlyGlnThrIleThrValGlyAsnLysAspGluValThr
            ::!!..||+  !! !|||  !!  :!:...||+   ...:!!!!::!!!  !::
            SerAlaProLeuMetGlyThrLysLeuValValProGlyGlnGluGlnAspSer
 10101049 : AGTGCTCCACTGATGGGTACAAAGCTGGTTGTGCCAGGACAAGAGCAAGACAGT : 10100996

      620 : GCCACCAAAAAAGTAGGATGCTGCTCAGCCTGA :      654
            AlaThrLysLysValGlyCysCysSerAla***
            !..   :!!|||  !  !||||||||+.!.|++
            GlyLeuGlnLysLysThrCysCysSerThr***
 10100995 : GGTCTACAAAAAAAAACATGCTGCTCTACTTAG : 10100961

vulgar: TRINITY_DN8004_c0_g1_i1.p1 15 654 + HiC_scaffold_6 10102692 10100960 - 653 C 201 201 S 2 2 5 0 2 I 0 1080 3 0 2 S 1 1 C 327 327 G 0 9 C 108 108
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-05-14
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_6  exonerate:coding2genome gene    10100961        10102692        653     -       .       gene_id 1 ; sequence TRINITY_DN8004_c0_g1_i1.p1 ; gene_orientation +
HiC_scaffold_6  exonerate:coding2genome cds     10102490        10102692        .       -       .
HiC_scaffold_6  exonerate:coding2genome exon    10102490        10102692        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_6  exonerate:coding2genome splice5 10102488        10102489        .       -       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_6  exonerate:coding2genome intron  10101406        10102489        .       -       .       intron_id 1
HiC_scaffold_6  exonerate:coding2genome splice3 10101406        10101407        .       -       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_6  exonerate:coding2genome cds     10100961        10101405        .       -       .
HiC_scaffold_6  exonerate:coding2genome exon    10100961        10101405        .       -       .       insertions 9 ; deletions 0
HiC_scaffold_6  exonerate:coding2genome similarity      10100961        10102692        653     -       .       alignment_id 1 ; Query TRINITY_DN8004_c0_g1_i1.p1 ; Align 10102693 16 201 ; Align 10101405 220 327 ; Align 10101069 547 108
# --- END OF GFF DUMP ---
#
>TRINITY_DN8004_c0_g1_i1.p1
GATCACAAAATAGACTACGTTTTTAAGGTGGTTCTGATTGGAGACTCTGCAGTAGGGAAGTCACAGCTGC
TTGCGCGATTTTCTCGGAATGAATTCAGTCTAGAGTCGAAGGCCACCATTGGTGTGGAGTTCCAAACTCG
AACTATTGTCGTTCAGCAGAAGACAATCAAAGCTCAAATTTGGGATACAGCTGGTCAGGAGAGGTATAGG
GCTGTAACTAGTGCCTACTACCGAGGAGCTGTAGGGGCCATGCTGGTTTATGACATCACGAAACGGCAAT
CATTTGATCATGTGCACCGGTGGCTCGAGGAATTGAAGGCACATGCAGATGCCAATATTGTTATAATGTT
GATAGGGAACAAGTCAGATCTGGGAGCGCTTAGACAAGTTCCAACTGAAGACGCCAAGGAGTATGCCGAA
CGGGAGGGACTCTTTTTCCTTGAAACTTCTGCCATGGACTCTACAAATGTTGAGGCAGCTTTCTTCACTG
TGCTCTCCGAAATTTACAGGATCGTGAGCAAGAAGGCCCTGATTGCAGATGAGAAGTCGCAAACTAATGG
GAGTGCTCCACTGATGGGTACAAAGCTGGTTGTGCCAGGACAAGAGCAAGACAGTGGTCTACAAAAAAAA
ACATGCTGCTCTACTTAG


C4 Alignment:
------------
         Query: TRINITY_DN8004_c0_g1_i1.p1 TRINITY_DN8004_c0_g1~~TRINITY_DN8004_c0_g1_i1.p1  ORF type:complete len:218 (-),score=21.80 TRINITY_DN8004_c0_g1_i1:1007-1660(-)
        Target: HiC_scaffold_9
         Model: coding2genome
     Raw score: 1057
   Query range: 0 -> 654
  Target range: 26880468 -> 26882256

        1 : ATGGCTTATAGATCGGACGATGATTATGACTATTTGTTCAAAGTGGTTCTTATT :       52
            MetAlaTyrArgSerAspAspAspTyrAspTyrLeuPheLysValValLeuIle
            |||||+||+||+||+||+||+||||||||+|||+|+|||||+|||||++|+||+
            MetAlaTyrArgSerAspAspAspTyrAspTyrLeuPheLysValValLeuIle
 26880469 : ATGGCGTACAGGTCAGATGACGATTATGATTATCTTTTCAAGGTGGTGTTGATC : 26880520

       53 : GGAGACTCTGGAGTAGGTAAATCAAATCTGTTGTCTAGATTTACCCGGAATGAG :      106
            GlyAspSerGlyValGlyLysSerAsnLeuLeuSerArgPheThrArgAsnGlu
            ||+||+|||||+||+||+|||||||||||++|+||+|||||+|||||+||+|||
            GlyAspSerGlyValGlyLysSerAsnLeuLeuSerArgPheThrArgAsnGlu
 26880521 : GGCGATTCTGGTGTTGGAAAATCAAATCTTCTTTCCAGATTCACCCGCAACGAG : 26880574

      107 : TTCAGTCTGGAATCGAAATCAACTATTGGCGTGGAGTTTGCTACTCGTAGCATC :      160
            PheSerLeuGluSerLysSerThrIleGlyValGluPheAlaThrArgSerIle
            |||||+||+||+||+|||||+||+||+||+|||||||||||+||++|+|||||+
            PheSerLeuGluSerLysSerThrIleGlyValGluPheAlaThrArgSerIle
 26880575 : TTCAGCCTAGAGTCAAAATCGACCATCGGGGTGGAGTTTGCGACGAGGAGCATA : 26880628

      161 : AATGTGGACAGCAAACTCATCAAGGCGCAGATATGGGACACTGCAGGTCAAGAA :      214
            AsnValAspSerLysLeuIleLysAlaGlnIleTrpAspThrAlaGlyGlnGlu
            |||||+||||||||+||+||||||||+|||||||||||+|||||+|||||||||
            AsnValAspSerLysLeuIleLysAlaGlnIleTrpAspThrAlaGlyGlnGlu
 26880629 : AATGTTGACAGCAAGCTGATCAAGGCACAGATATGGGATACTGCTGGTCAAGAA : 26880682

      215 : {AG}  >>>> Target Intron 1 >>>>  {G}TACAGGGCAATCACAAGT :      235
            {Ar}                             {g}TyrArgAlaIleThrSer
            {||}           1134 bp           {|}|||+||||+||+||||||
            {Ar}++                         ++{g}TyrArgAlaIleThrSer
 26880683 : {AG}gt.........................ag{G}TACCGGGCTATTACAAGT : 26881837

      236 : GCGTATTATCGTGGGGCTGTCGGAGCTCTACTGGTCTATGATATTACACGGCAC :      289
            AlaTyrTyrArgGlyAlaValGlyAlaLeuLeuValTyrAspIleThrArgHis
            ||+|||||++|+||+||+||+|||||++|++|+||+||+||+||+||+||+||+
            AlaTyrTyrArgGlyAlaValGlyAlaLeuLeuValTyrAspIleThrArgHis
 26881838 : GCCTATTACAGAGGAGCAGTTGGAGCATTGTTAGTGTACGACATAACTCGACAT : 26881891

      290 : GTAACCTTCGAGAATGTGGAGAGATGGCTAAAAGAACTCAAGGATCACACTGAT :      343
            ValThrPheGluAsnValGluArgTrpLeuLysGluLeuLysAspHisThrAsp
            ||+||+||+||+||+|||||+||||||+|+|||||+||||||||||||||+|||
            ValThrPheGluAsnValGluArgTrpLeuLysGluLeuLysAspHisThrAsp
 26881892 : GTGACTTTTGAAAACGTGGAAAGATGGTTGAAAGAGCTCAAGGATCACACCGAT : 26881945

      344 : TCTAATATTGTGGTTATGCTAGTTGGTAACAAGTCAGATCTGCGACATCTAAGA :      397
            SerAsnIleValValMetLeuValGlyAsnLysSerAspLeuArgHisLeuArg
            ||+||+|||||+||+|||+|+|||||+||||||||+||+|||||+|||+|||||
            SerAsnIleValValMetLeuValGlyAsnLysSerAspLeuArgHisLeuArg
 26881946 : TCCAACATTGTCGTGATGTTGGTTGGGAACAAGTCTGACCTGCGCCATTTAAGA : 26881999

      398 : GCCGTTTCAACTGACGATGGTCAATCCTTTTCGGAGAAGGAAGGTCTTTTCTTC :      451
            AlaValSerThrAspAspGlyGlnSerPheSerGluLysGluGlyLeuPhePhe
            ||+|||||+.!!||+|||||||||||+||+||+||||||||+||+|||||||||
            AlaValSerAlaAspAspGlyGlnSerPheSerGluLysGluGlyLeuPhePhe
 26882000 : GCTGTTTCTGCTGATGATGGTCAATCATTCTCTGAGAAGGAGGGCCTTTTCTTC : 26882053

      452 : ATGGAGACATCTGCGCTTGAATCTACGAATGTTGAAAATGCCTTCAAGCAGATA :      505
            MetGluThrSerAlaLeuGluSerThrAsnValGluAsnAlaPheLysGlnIle
            ||||||||+||+||+||||||||+||+|||||+||+|||||+|||||+||||||
            MetGluThrSerAlaLeuGluSerThrAsnValGluAsnAlaPheLysGlnIle
 26882054 : ATGGAGACCTCAGCTCTTGAATCAACAAATGTGGAGAATGCTTTCAAACAGATA : 26882107

      506 : CTGACGCAGATCTACCGGGTGGTAAGTAAAAAGGCTCTGGACGTTGGTGACGAT :      559
            LeuThrGlnIleTyrArgValValSerLysLysAlaLeuAspValGlyAspAsp
            +||||+|||||+||||||||+||+||+||+|||||+|||||+|||||+||+|||
            LeuThrGlnIleTyrArgValValSerLysLysAlaLeuAspValGlyAspAsp
 26882108 : TTGACTCAGATATACCGGGTTGTGAGCAAGAAGGCACTGGATGTTGGAGATGAT : 26882161

      560 : CCATCTGCTGGCCCTGGGAAGGGTCAAACTATTACAGTCGGCAACAAAGATGAA :      613
            ProSerAlaGlyProGlyLysGlyGlnThrIleThrValGlyAsnLysAspGlu
            |||||+.!!!  ||+||||||||+|||||+||+!::||+||+|||||+|||!!:
            ProSerThrValProGlyLysGlyGlnThrIleSerValGlyAsnLysAspAsp
 26882162 : CCATCAACTGTGCCAGGGAAGGGGCAAACCATAAGTGTTGGAAACAAGGATGAT : 26882215

      614 : GTTACAGCCACCAAAAAAGTAGGATGCTGCTCAGCCTGA :      654
            ValThrAlaThrLysLysValGlyCysCysSerAla***
            |||||+||+||+|||||+||+||+|||||+||+||+|++
            ValThrAlaThrLysLysValGlyCysCysSerAla***
 26882216 : GTTACTGCAACAAAAAAGGTTGGGTGCTGTTCTGCATAG : 26882256

vulgar: TRINITY_DN8004_c0_g1_i1.p1 0 654 + HiC_scaffold_9 26880468 26882256 + 1057 C 216 216 S 2 2 5 0 2 I 0 1130 3 0 2 S 1 1 C 435 435
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-05-14
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:coding2genome gene    26880469        26882256        1057    +       .       gene_id 1 ; sequence TRINITY_DN8004_c0_g1_i1.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     26880469        26880686        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    26880469        26880686        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 26880687        26880688        .       +       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  26880687        26881820        .       +       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 26881819        26881820        .       +       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     26881821        26882256        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    26881821        26882256        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome similarity      26880469        26882256        1057    +       .       alignment_id 1 ; Query TRINITY_DN8004_c0_g1_i1.p1 ; Align 26880469 1 216 ; Align 26881822 220 435
# --- END OF GFF DUMP ---
#
>TRINITY_DN8004_c0_g1_i1.p1
ATGGCGTACAGGTCAGATGACGATTATGATTATCTTTTCAAGGTGGTGTTGATCGGCGATTCTGGTGTTG
GAAAATCAAATCTTCTTTCCAGATTCACCCGCAACGAGTTCAGCCTAGAGTCAAAATCGACCATCGGGGT
GGAGTTTGCGACGAGGAGCATAAATGTTGACAGCAAGCTGATCAAGGCACAGATATGGGATACTGCTGGT
CAAGAAAGGTACCGGGCTATTACAAGTGCCTATTACAGAGGAGCAGTTGGAGCATTGTTAGTGTACGACA
TAACTCGACATGTGACTTTTGAAAACGTGGAAAGATGGTTGAAAGAGCTCAAGGATCACACCGATTCCAA
CATTGTCGTGATGTTGGTTGGGAACAAGTCTGACCTGCGCCATTTAAGAGCTGTTTCTGCTGATGATGGT
CAATCATTCTCTGAGAAGGAGGGCCTTTTCTTCATGGAGACCTCAGCTCTTGAATCAACAAATGTGGAGA
ATGCTTTCAAACAGATATTGACTCAGATATACCGGGTTGTGAGCAAGAAGGCACTGGATGTTGGAGATGA
TCCATCAACTGTGCCAGGGAAGGGGCAAACCATAAGTGTTGGAAACAAGGATGATGTTACTGCAACAAAA
AAGGTTGGGTGCTGTTCTGCATAG


C4 Alignment:
------------
         Query: TRINITY_DN8004_c0_g1_i1.p1 TRINITY_DN8004_c0_g1~~TRINITY_DN8004_c0_g1_i1.p1  ORF type:complete len:218 (-),score=21.8:[revcomp]DN8004_c0_g1_i1:1007-1660(-)
        Target: HiC_scaffold_9 [revcomp]
         Model: coding2genome
     Raw score: 787
   Query range: 650 -> 8
  Target range: 26882252 -> 26880476

      650 : GCTGAGCAGCATCCTACTTTTTTGGTGGCTGTAACTTCATCTTTGTTGCCGACT :      599
            AlaGluGlnHisProThrPheLeuValAlaValThrSerSerLeuLeuProThr
            ||+||+|||||+||+||+|||!!.||+||+|||||+|||||+|||!!.||+||+
            AlaGluGlnHisProThrPhePheValAlaValThrSerSerLeuPheProThr
 26882252 : GCAGAACAGCACCCAACCTTTTTTGTTGCAGTAACATCATCCTTGTTTCCAACA : 26882201

      598 : GTAATAGTTTGACCCTTCCCAGGGCCAGCAGATGGATCGTCACCAACGTCCAGA :      545
            ValIleVal***ProPheProGlyProAlaAspGlySerSerProThrSerArg
            :!:!!:|||!! ||||||||+||+ !!!..||||||||+||+|||||+|||!!
            LeuMetValCysProPheProGlyThrValAspGlySerSerProThrSerSer
 26882200 : CTTATGGTTTGCCCCTTCCCTGGCACAGTTGATGGATCATCTCCAACATCCAGT : 26882147

      544 : GCCTTTTTACTTACCACCCGGTAGATCTGCGTCAGTATCTGCTTGAAGGCATTT :      491
            AlaPheLeuLeuThrThrArg***IleCysValSerIleCysLeuLysAlaPhe
            |||||+||+||+||+||||||!! |||!! |||!:!|||||+|||||+|||||+
            AlaPheLeuLeuThrThrArgTyrIle***ValAsnIleCysLeuLysAlaPhe
 26882146 : GCCTTCTTGCTCACAACCCGGTATATCTGAGTCAATATCTGTTTGAAAGCATTC : 26882093

      490 : TCAACATTCGTAGATTCAAGCGCAGATGTCTCCATGAAGAAAAGACCTTCCTTC :      437
            SerThrPheValAspSerSerAlaAspValSerMetLysLysArgProSerPhe
            ||+|||||+||+||||||!! ||+!!:|||||||||||||||||+||+||||||
            SerThrPheValAspSerArgAlaGluValSerMetLysLysArgProSerPhe
 26882092 : TCCACATTTGTTGATTCAAGAGCTGAGGTCTCCATGAAGAAAAGGCCCTCCTTC : 26882039

      436 : TCCGAAAAGGATTGACCATCGTCAGTTGAAACGGCTCTTAGATGTCGCAGATCT :      383
            SerGluLysAsp***ProSerSerValGluThrAlaLeuArgCysArgArgSer
            ||+||+!!.|||||||||||+|||!..|||||+||||||!:!!! |||||+||+
            SerGluAsnAsp***ProSerSerAlaGluThrAlaLeuLysTrpArgArgSer
 26882038 : TCAGAGAATGATTGACCATCATCAGCAGAAACAGCTCTTAAATGGCGCAGGTCA : 26881985

      382 : GACTTGTTACCAACTAGCATAACCACAATATTAGAATCAGTGTGATCCTTGAGT :      329
            AspLeuLeuProThrSerIleThrThrIleLeuGluSerVal***SerLeuSer
            ||||||!!.|||||+!:!||+||+|||!!:||+|||||+||||||||||||||+
            AspLeuPheProThrAsnIleThrThrMetLeuGluSerVal***SerLeuSer
 26881984 : GACTTGTTCCCAACCAACATCACGACAATGTTGGAATCGGTGTGATCCTTGAGC : 26881931

      328 : TCTTTTAGCCATCTCTCCACATTCTCGAAGGTTACGTGCCGTGTAATATCATAG :      275
            SerPheSerHisLeuSerThrPheSerLysValThrCysArgValIleSer***
            |||||+!:!|||||+|||||+||+||+||+||+||+||+||+||+!!:||+!!
            SerPheAsnHisLeuSerThrPheSerLysValThrCysArgValMetSerTyr
 26881930 : TCTTTCAACCATCTTTCCACGTTTTCAAAAGTCACATGTCGAGTTATGTCGTAC : 26881877

      274 : ACCAGTAGAGCTCCGACAGCCCCACGATAATACGCACTTGTGATTGCC{CT}   :      222
            ThrSerArgAlaProThrAlaProArg***TyrAlaLeuValIleAla{Le}
            ||+!::!..|||||+||+||+||+!  |||!! ||||||||+||+|||{  }
            ThrAsnAsnAlaProThrAlaProLeu******AlaLeuValIleAla{Ar}++
 26881876 : ACTAACAATGCTCCAACTGCTCCTCTGTAATAGGCACTTGTAATAGCC{CG}gt : 26881824

      221 : >>>> Target Intron 1 >>>>  {G}TACCTTTCTTGACCTGCAGTGTCC :      200
                                       {u}TyrLeuSer***ProAlaValSer
                     1134 bp           { }:!!|||||||||||+|||||+|||
                                     +-{g}HisLeuSer***ProAlaValSer
 26881823 : .........................ac{A}CACCTTTCTTGACCAGCAGTATCC : 26880668

      199 : CATATCTGCGCCTTGATGAGTTTGCTGTCCACATTGATGCTACGAGTAGCAAAC :      146
            HisIleCysAlaLeuMetSerLeuLeuSerThrLeuMetLeuArgValAlaAsn
            ||||||||+||||||!!:||+||||||||+|||!!.|||||+!  ||+||||||
            HisIleCysAlaLeuIleSerLeuLeuSerThrPheMetLeuLeuValAlaAsn
 26880667 : CATATCTGTGCCTTGATCAGCTTGCTGTCAACATTTATGCTCCTCGTCGCAAAC : 26880614

      145 : TCCACGCCAATAGTTGATTTCGATTCCAGACTGAACTCATTCCGGGTAAATCTA :       92
            SerThrProIleValAspPheAspSerArgLeuAsnSerPheArgValAsnLeu
            |||||+||+!!:||+|||||+||+||+||+||||||||+!!.|||||+|||||+
            SerThrProMetValAspPheAspSerArgLeuAsnSerLeuArgValAsnLeu
 26880613 : TCCACCCCGATGGTCGATTTTGACTCTAGGCTGAACTCGTTGCGGGTGAATCTG : 26880560

       91 : GACAACAGATTTGATTTACCTACTCCAGAGTCTCCAATAAGAACCACTTTGAAC :       38
            AspAsnArgPheAspLeuProThrProGluSerProIleArgThrThrLeuAsn
            !!:!..|||||||||!!.||+||+|||||+||+||+||+!..|||||+|||!!.
            GluArgArgPheAspPheProThrProGluSerProIleAsnThrThrLeuLys
 26880559 : GAAAGAAGATTTGATTTTCCAACACCAGAATCGCCGATCAACACCACCTTGAAA : 26880506

       37 : AAATAGTCATAATCATCGTCCGATCTA :        9
            Lys***Ser***SerSerSerAspLeu
            !:!||+||||||||+||+||+||+||+
            Arg***Ser***SerSerSerAspLeu
 26880505 : AGATAATCATAATCGTCATCTGACCTG : 26880477

vulgar: TRINITY_DN8004_c0_g1_i1.p1 650 8 - HiC_scaffold_9 26882252 26880476 - 787 C 426 426 S 2 2 5 0 2 I 0 1130 3 0 2 S 1 1 C 213 213
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-05-14
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:coding2genome gene    26880477        26882252        787     -       .       gene_id 1 ; sequence TRINITY_DN8004_c0_g1_i1.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     26881825        26882252        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    26881825        26882252        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 26881823        26881824        .       -       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  26880691        26881824        .       -       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 26880691        26880692        .       -       .       intron_id 0 ; splice_site "AC"
HiC_scaffold_9  exonerate:coding2genome cds     26880477        26880690        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    26880477        26880690        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome similarity      26880477        26882252        787     -       .       alignment_id 1 ; Query TRINITY_DN8004_c0_g1_i1.p1 ; Align 26882253 651 426 ; Align 26880690 222 213
# --- END OF GFF DUMP ---
#
>TRINITY_DN8004_c0_g1_i1.p1
GCAGAACAGCACCCAACCTTTTTTGTTGCAGTAACATCATCCTTGTTTCCAACACTTATGGTTTGCCCCT
TCCCTGGCACAGTTGATGGATCATCTCCAACATCCAGTGCCTTCTTGCTCACAACCCGGTATATCTGAGT
CAATATCTGTTTGAAAGCATTCTCCACATTTGTTGATTCAAGAGCTGAGGTCTCCATGAAGAAAAGGCCC
TCCTTCTCAGAGAATGATTGACCATCATCAGCAGAAACAGCTCTTAAATGGCGCAGGTCAGACTTGTTCC
CAACCAACATCACGACAATGTTGGAATCGGTGTGATCCTTGAGCTCTTTCAACCATCTTTCCACGTTTTC
AAAAGTCACATGTCGAGTTATGTCGTACACTAACAATGCTCCAACTGCTCCTCTGTAATAGGCACTTGTA
ATAGCCCGACACCTTTCTTGACCAGCAGTATCCCATATCTGTGCCTTGATCAGCTTGCTGTCAACATTTA
TGCTCCTCGTCGCAAACTCCACCCCGATGGTCGATTTTGACTCTAGGCTGAACTCGTTGCGGGTGAATCT
GGAAAGAAGATTTGATTTTCCAACACCAGAATCGCCGATCAACACCACCTTGAAAAGATAATCATAATCG
TCATCTGACCTG

-- completed exonerate analysis




TIS IS THE GOOD HOMOLOG ogf MpUg00350

##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-05-14
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:coding2genome gene    26880469        26882256        1057    +       .       gene_id 1 ; sequence TRINITY_DN8004_c0_g1_i1.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     26880469        26880686        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    26880469        26880686        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 26880687        26880688        .       +       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  26880687        26881820        .       +       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 26881819        26881820        .       +       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     26881821        26882256        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    26881821        26882256        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome similarity      26880469        26882256        1057    +       .       alignment_id 1 ; Query TRINITY_DN8004_c0_g1_i1.p1 ; Align 26880469 1 216 ; Align 26881822 220 435
# --- END OF GFF DUMP ---
#
>Halsinki_Male_MpUg00350_TRINITY_DN8004_c0_g1_i1.p1
ATGGCGTACAGGTCAGATGACGATTATGATTATCTTTTCAAGGTGGTGTTGATCGGCGATTCTGGTGTTG
GAAAATCAAATCTTCTTTCCAGATTCACCCGCAACGAGTTCAGCCTAGAGTCAAAATCGACCATCGGGGT
GGAGTTTGCGACGAGGAGCATAAATGTTGACAGCAAGCTGATCAAGGCACAGATATGGGATACTGCTGGT
CAAGAAAGGTACCGGGCTATTACAAGTGCCTATTACAGAGGAGCAGTTGGAGCATTGTTAGTGTACGACA
TAACTCGACATGTGACTTTTGAAAACGTGGAAAGATGGTTGAAAGAGCTCAAGGATCACACCGATTCCAA
CATTGTCGTGATGTTGGTTGGGAACAAGTCTGACCTGCGCCATTTAAGAGCTGTTTCTGCTGATGATGGT
CAATCATTCTCTGAGAAGGAGGGCCTTTTCTTCATGGAGACCTCAGCTCTTGAATCAACAAATGTGGAGA
ATGCTTTCAAACAGATATTGACTCAGATATACCGGGTTGTGAGCAAGAAGGCACTGGATGTTGGAGATGA
TCCATCAACTGTGCCAGGGAAGGGGCAAACCATAAGTGTTGGAAACAAGGATGATGTTACTGCAACAAAA
AAGGTTGGGTGCTGTTCTGCATAG










# MpUg00410


## against Helsinki genome

Query= Blasia_pusilla_c333784

Length=711
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

mRNA18903 gene=gene16809                                              1314    0.0

>mRNA18903 gene=gene16809
ATGTTCTCTTCTTTTAAGACCACGAGTTTTATGAGCACGAGCATGAGGGGAGGCTCATCACCTATATCGAGAAAATCGCTGAATTTACCTCCTGCCAGGTCCAGCGCCTCAACAGCGGCGTCAGCAGCTGGATCCAAGAAGGCCAGCACCCATCAGCCGCAGCATCAGCAAGAGCAACATTCGGCGGAGGAGGAGCCCAAGACGGCCGAAACAACGACAAGTGCTGCAGCATCTATTGCTGCAGCGGCAGCAGCAGCAGTGGGAGGAGAAGGAGCTGCAGCTACAGAGCCGCGACCACCATCGCCGGGTCCCGGCACCGGGGGACGGAGGGGAGAAGGTTCAGGCTCTCTCGCGAATCCGACCTCCTCATCTGCAGGGCTCAACGAGATCATGCCTTCGTCGGAGGCGGAGGCAGAGGCAGAAGCAGAGGCCGGGGTTACTTCCTTACCCCTTGAAGACGAGGACATTCAAAGTGAGCAGGGGAGTGAACTGAGTGACGGAGGCCTGGAAAAGCTCTCCCTTGGTCCACGGTACCTGAAGGGGGAAGCTGATCGTTACGATGGAATCATCGTTGATCCTAGCTGCTTACCTGCGGACATACCCACGTTTGTCGATGCCCTCCGTGCGTCGCTGGACTTGTGGAAATCACAGGAAAGGAAAGGAGTGTGGCTCAAATTACCCACAGCCAATGCCAATCTTGTGTTTCCAGCAATTGAGGCAGGCTTTGGTTATCACCACGCTGAGCCGACTTATGTAATGCTGACGTATTGGATACCTGAAAGTCCGTGTACAATCCCATCAAATGCCTCACATCAAGTTGGTGTTGGGGCTTTTGTGCTGAATGACCAAAGACAGGTGCTTGCAGTGCAAGAAAAGAATGGACCTTTGAGAGGTTCAGGAATCTGGAAGATGCCGACAGGATTGATTAATCAGGGTGAAGATATTCACGATGGTGCTGTGAGGGAAGTGAAGGAGGAAACAGGGGTGGATACAGAGTTTTTGGAAGTTATTGGATTCCGGCAAGGGCATCAAGTATCGTTTGAAAAGTCAGATCTGTTCTTCCTGTGTGTTCTACGACCGCTTTCAAGCACCATAGTGAAACAAGATTCGGAAATTGAAGCAGCACAGTGGATGCCGTTGGACGAATTTGCAGTACAACCTTTCTTCCAAAGAAGCATGCTGCGAAAAATGCTGGATATTTGCGTGGCAAGCACTGAAGGCCAATACAGAGGATTTGGTAGCCGAGCATTGCATGCTGGTTTCCAAAGGTTGCCTTCGTATTTTTACTACAATGTACAAGACTCCAAGGAAAACTCGAACCTGTAG



## against Potsdam 

Query= Blasia_pusilla_c333784

Length=711
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

asmbl_38346.p1 GENE.asmbl_38346~~asmbl_38346.p1 ORF type:complete...  1314    0.0

>asmbl_38346.p1 GENE.asmbl_38346~~asmbl_38346.p1  ORF type:complete len:442 (+),score=87.49 asmbl_38346:66-1391(+)
ATGTTCTCTTCTTTTAAGACCACGAGTTTTATGAGCACGAGCATGAGGGGAGGCTCATCACCTATATCGAGAAAATCGCTGAATTTACCTCCTGCCAGGTCCAGCGCCTCAACAGCGGCGTCAGCAGCTGGATCCAAGAAGGCCAGCACCCATCAGCCGCAGCATCAGCAAGAGCAACATTCGGCGGAGGAGGAGCCCAAGACGGCCGAAACAACGACAAGTGCTGCAGCATCTATTGCTGCAGCGGCAGCAGCAGCAGTGGGAGGAGAAGGAGCTGCAGCTACAGAGCCGCGACCACCATCGCCGGGTCCCGGCACCGGGGGACGGAGGGGAGAAGGTTCAGGCTCTCTCGCGAATCCGACCTCCTCATCTGCAGGGCTCAACGAGATCATGCCTTCGTCGGAGGCGGAGGCAGAGGCAGAAGCAGAGGCCGGGGTTACTTCCTTACCCCTTGAAGACGAGGACATTCAAAGTGAGCAGGGGAGTGAACTGAGTGACGGAGGCCTGGAAAAGCTCTCCCTTGGTCCACGGTACCTGAAGGGGGAAGCTGATCGTTACGATGGAATCATCGTTGATCCTAGCTGCTTACCTGCGGACATACCCACGTTTGTCGATGCCCTCCGTGCGTCGCTGGACTTGTGGAAATCACAGGAAAGGAAAGGAGTGTGGCTCAAATTACCCACAGCCAATGCCAATCTTGTGTTTCCAGCAATTGAGGCAGGCTTTGGTTATCACCACGCTGAGCCGACTTATGTAATGCTGACGTATTGGATACCTGAAAGTCCGTGTACAATCCCATCAAATGCCTCACATCAAGTTGGTGTTGGGGCTTTTGTGCTGAATGACCAAAGACAGGTGCTTGCAGTGCAAGAAAAGAATGGACCTTTGAGAGGTTCAGGAATCTGGAAGATGCCGACAGGATTGATTAATCAGGGTGAAGATATTCACGATGGTGCTGTGAGGGAAGTGAAGGAGGAAACAGGGGTGGATACAGAGTTTTTGGAAGTTATTGGATTCCGGCAAGGGCATCAAGTATCGTTTGAAAAGTCAGATCTGTTCTTCCTGTGTGTTCTACGACCGCTTTCAAGCACCATAGTGAAACAAGATTCGGAAATTGAAGCAGCACAGTGGATGCCGTTGGACGAATTTGCAGTACAACCTTTCTTCCAAAGAAGCATGCTGCGAAAAATGCTGGATATTTGCGTGGCAAGCACTGAAGGCCAATACAGAGGATTTGGTAGCCGAGCATTGCATGCTGGTTTCCAAAGGTTGCCTTCGTATTTTTACTACAATGTACAAGACTCCAAGGAAAACTCGAACCTGTAG

## against Aistralia

Query= Blasia_pusilla_c333784

Length=711
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.7168.1.p1 GENE.STRG.7168.1~~STRG.7168.1.p1 ORF type:complete...  608     5e-174


>STRG.7168.1.p1 GENE.STRG.7168.1~~STRG.7168.1.p1  ORF type:complete (+),score=26.11 len:522 STRG.7168.1:2709-3230(+)
ATGCCTTCGTCGGAGGCGGAGGCAGAGGCAGAAGCAGAGGCCGGGGTTACTTCCTTACCCCTTGAAGACGAGGACATTCAAAGTGAGCAGGGGAGTGAACTGAGTGACGGAGGCCTGGAAAAGCTCTCCCTTGGTCCACGGTACCTGAAGGGGGAAGCTGATCGTTACGATGGAATCATCGTTGATCCTAGCTGCTTACCTGCGGACATACCCACGTTTGTCGATGCCCTCCGTGCGTCGCTGGACTTGTGGAAATCACAGGAAAGGAAAGGAGTGTGGCTCAAATTACCCACAGCCAATGCCAATCTTGTGTTTCCAGCAATTGAGGCAGGCTTTGGTTATCACCACGCTGAGCCGACTTATGTAATGCTGACGTATTGGATACCTGAAAGTCCGTGTACAATCCCATCAAATGCCTCACATCAAGTTGGTGTTGGGGCTTTTGTGCTGAATGACCAAAGACAGGTACATGACAGAATCACCGGCAAATCAAACACTAAAGCCTCCATGTACTTGTGGTAA





# MpUg00430


## against Helsinki

Query= Blasia_pusilla_c334273

Length=1026
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

mRNA19123 gene=gene17011                                              363     1e-99
mRNA19124 gene=gene17012                                              318     2e-86

>mRNA19123 gene=gene17011
ATGATTGCAAATTATGTGTTTGTTCAGACGCGTCTGTGTGTGCCATCAAGTAGTGGAGTTGTTGAGCTGGGCTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTTTTTGCTGAGATGAACGAGATTAGCGGAAACTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGGCTTGCCCAGCAAGCCCCTCTCTGGTCAGCATTCATGGAACAAGAAGGCATGAGTCCTTGGACAGGGGACCTGATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTAGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTCGCTGATGTTTCTATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATTTCTGGATCATTCCGTGGAAGAAGCTCTGGTCAAGTAGACAAGGTTGACTCTATTGGAGTGGTGAAGAAAGCTCAGCCTGGTGTTCAAGCTCAAAATCAAAGATGTGGAATAGAAGGTGAAATTTTAAATCCACCACCAGATATGAGACAAGGCAGAAGGGAGGTGTTGCATCCGAATAGTGTGTCAACAACTCACATCAAAGAAGAAGGGCCAAGGACATCAAAGCACAAGGTCTCGCCATCTATGAAGATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAGTGCTTATGTCAGATCAAGTATTGAGTCTGAACAAGACTTGGATTCAGAGGCAGAAGTTTCATACAAAGAAGTTGAGTGCAGTTTCACTGTTGGACAAAAGCCACCTAGAAAACGTGGAAGAAAGCCTGCGAATGACAGGGAAGAACCTTTAAATCATGTACAGGCTGAACGACAAAGACGAGAGAAACTCAACCAGCGGTTCTATGCCTTGCGCTCTGTCGTTCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTACAGGATATGGAGTTTCAAATCAAGGAACTCGAGGGTCAAGCAAAATTGCTGAGAGAAAAATCATCTGAGACAGAGATTGCAAGAGGAGGCATGACTAAGTTGTCTAGAGACATTTCACATCCAAAAACCTTTTCTGGTGGAAGCTCGATGACTTCTGGAGTCAAGATTTCTACGGATCAAAGACCAATGGTGTCTGTGCATGTCTTACGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCTGGAATCGCAAACTTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGCATGCCAATCTATCAATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAGTAAGTAGTCCCTATTCTTAG

>mRNA19124 gene=gene17012
ATGGGAACGGATGGCGTTATTCGAAGCTTATGGGACGAGGATAATGCGATGATAGAAGCATTCATGGGATCGGCTGTGTATAACTTATCCTGTAGCTCTCAAGTGGATTTGACGGGAACAGAAACACCAGCTTACAGCGAGACGGCGTTACAACAAAAACTCCAGCTTCTGGTTGAGTCATCGAGTATGAACTGGACCTACGCAATCTTTTGGCAGATTTCAAATTCATCGAGTGGAGAGATGATGCTAGGTTGGGGTGACGGTTACTTCAAGGGACCAAAAGATAGTGAGGTGAATGATATGAAGCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTAAGGCGAAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGATGCTGGAGGTTCTGGCGGGCTTGATTATGTAACAGATACAGAGTGGTTCTACCTTGTTTCGATGTCATGTTTCTTTCCTCCTGGTGTTGGGATTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGGCTAATGGAAGCAAACAAGGCTCCTAGTAGTGTTTGTACACGAGCGGATCTTGCTAAGGTGTGA


191123 is a very strange and improper translation of the genomic sequence
19124 is ok but incompete tto short;; must be replaced by the following sequence

>Postadam_female_TRINITY_DN1489_c0_g2_i3.p1
ATGGGAACGGATGGCGTTATTCGAAGCTTATGGGACGAGGATAATGCGATGATAGAAGCATTCATGGGAT
CGGCTGTGTATAACTTATCCTGTAGCTCTCAAGTGGATTTGACGGGAACAGAAACACCAGCTTACAGCGA
GACGGCGTTACAACAAAAACTCCAGCTTCTGGTTGAGTCATCGAGTATGAACTGGACCTACGCAATCTTT
TGGCAGATTTCAAATTCATCGAGTGGAGAGATGATGCTAGGTTGGGGTGACGGTTACTTCAAGGGACCAA
AAGATAGTGAGGTGAATGATATGAAGCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTAAGGCG
AAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGATGCTGGAGGTTCTGGCGGGCTT
GATTATGTAACAGATACAGAGTGGTTCTACCTTGTTTCGATGTCATGTTTCTTTCCTCCTGGTGTTGGGA
TTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGGCTAATGGAAGCAAACAAGGCTCCTAGTAGTGT
TTGTACACGAGCGGATCTTGCTAAGTATGTGTTTGTTCAGACGCGTCTGTGTGTGCCATCAAGTAGTGGA
GTTGTTGAGCTGGGCTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTTT
TTGCTGAGATGAACGAGATTAGCGGAAACTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGGC
TTGCCCAGCAAGCCCCTCTCTGGTCAGCATTCATGGAACAAGAAGGCATGAGTCCTTGGACAGGGGACCT
GATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTAGACT
CTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTCGCTGATGTTTCTATTGACAATGAAAGGGAGCTTCA
GAGTGGAACTACAATTTCTGGATCATTCCGTGGAAGAAGCTCTGGTCAAGTAGACAAGGTTGACTCTATT
GGAGTGGTGAAGAAAGCTCAGCCTGGTGTTCAAGCTCAAAATCAAAGATGTGGAATAGAAGGTGAAATTT
TAAATCCACCACCAGATATGAGACAAGGCAGAAGGGAGGTGTTGCATCCGAATAGTGTGTCAACAACTCA
CATCAAAGAAGAAGGGCCAAGGACATCAAAGCACAAGGTCTCGCCATCTATGAAGATCCCACATCCAGAA
GGGGTGGGACCTCTTCCCAGTGCTTATGTCAGATCAAGTATTGAGTCTGAACAAGACTTGGATTCAGAGG
CAGAAGTTTCATACAAAGAAGTTGAGTGCAGTTTCACTGTTGGACAAAAGCCACCTAGAAAACGTGGAAG
AAAGCCTGCGAATGACAGGGAAGAACCTTTAAATCATGTACAGGCTGAACGACAAAGACGAGAGAAACTC
AACCAGCGGTTCTATGCCTTGCGCTCTGTCGTTCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAG
GTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTACAGGATATGGAGTTTCAAATCAAGGAACT
CGAGGGTCAAGCAAAATTGCTGAGAGAAAAATCATCTGAGACAGAGATTGCAAGAGGAGGCATGACTAAG
TTGTCTAGAGACATTTCACATCCAAAAACCTTTTCTGGTGGAAGCTCGATGACTTCTGGAGTCAAGATTT
CTACGGATCAAAGACCAATGGTGTCTGTGCATGTCTTACGAAATGAGGCAATGATTCGAGTACATAGCAC
CAAGGAGTCATCTGGAATCGCAAACTTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGCATGCC
AATCTATCAATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAACGCACCAT
CAGAGGCGCAGCTGATTAGTGCAATTGAAAAGTCATTTGACACAGGTGTTATAAACCCAATTAAAATAAA
TCATGATCCATACCTTGTAGAACTCAAACCCGACAATGGAAAATGTAGTATTCATTGA


## against Potsdam

Query= Blasia_pusilla_c334273

Length=1026
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

TRINITY_DN1489_c0_g2_i3.p1 TRINITY_DN1489_c0_g2~~TRINITY_DN1489_c...  579     1e-164
TRINITY_DN9772_c2_g1_i1.p1 TRINITY_DN9772_c2_g1~~TRINITY_DN9772_c...  106     3e-22

>TRINITY_DN1489_c0_g2_i3.p1 TRINITY_DN1489_c0_g2~~TRINITY_DN1489_c0_g2_i3.p1  ORF type:complete len:696 (+),score=102.21 TRINITY_DN1489_c0_g2_i3:202-2289(+)
ATGGGAACGGATGGAGGTATTCGAAGCTTATGGGACGAGGATAATGCGATGATAGAAGCATTCATGGGATCGGCTGTGTATAACCTATCTTGTAGCTCTCAAGTGGATTTCCCGGGAGCAGAAAGACCTGCTTACAGCGAGACCGCGTTACAACAAAAACTCCAGCTTTTGGTTGAGTCATCGACTGTGAATTGGACCTACGCAATCTTTTGGCAGATTTCGAATTCGTCGAGTGGAGAGATGATGCTAGGTTGGGGTGATGGTTACTTTAAGGGACCAAAAGATAGTGAGGTGAATGATCTGAATCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTAAGGCGAAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGAAGCTGGAGCTTCAGCCGGGCTTGATTATGTAACAGATACAGAGTGGTTCTACCTTGTTTCGATGTCATGTTTCTTTCCTCCTGGTGTTGGGATTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGGCTAATGGAAGCTAACAAGGCTCCTAGTAATATTTGCACACAAGCCGATCTTGCTAAGAAGGCAGGGATCCAGACGCGTTTGTGTGTACCATCAACTAGTGGAGTTGTTGAGCTGGGGTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTGTTTGCTGAGATGAACGAGATCAGCGGAAATTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGCCTTGCCCAGCAAGTCCCTCTCTGGTCAGCATTCATGGATCAAGAAGGCATGAATCCTTGGACAGGGGACCTGATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTGGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTTGCTGATGTTTCCATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATCTCTGGACCATATCGTGGAAGAAACTCTGGTCAAGTAGACAAGGTCGACTCTATGGTAGTGGTGAAGAAAGCTCAGCCTGGTGTCCAAGCTCAAAATCAAAGGTGTGGAATTGAGGGTGAAATTTTAAATCCACCACCCGATATGAGACAGGGCAGAGGGGAGGTGTTGCATCTGAACAGTGTTTCAACAACTCACATCAAAGAGGAAGGGCCTAGGTCATCAAAGCAAAAGGTCTCTCCATCTATGAAAATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAGTGCTTACGTCCGATCAAGTATTGAGTCTGAACAAGACCTGGATTCAGAGGCAGAAGTTTCATACAAAGAAGTAGAGTGCAGTTTCACTGTTGGACAAAAGCCACCGCGAAAACGTGGAAGAAAGCCTGCAAATGACAGGGAAGAACCTTTAAACCATGTACAGGCTGAACGACAAAGACGAGAGAAGCTCAACCAGCGATTCTACGCCTTGCGCTCTGTTGTGCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTGCAGGATATGGAGTTTCAAATCAAGGAACTCGAGGCTCAAGCTAAATTGCTGACAGAAAAATCATCTGAGACAGAGGTTGGAAGAGGAGGCATGAACAAGTTGTCTAATGAGATTTTACATCCCAAAACCTTTTCTGGCAGAAGCTCAATGACTTCTGGAGTCAAGATTTCTACGGATCAGAGACCAATGGTATCTGTGCATGTCTTGCGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCCGGAATGGCAAATTTGATGTGGACTTTGGAAGATTTAAAGCTGCAGGTTCAGCACGCCAACCTATCAATTCATGCGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAATGCACTATCAGAAGCTCAGTTGATTAGTGCTATCGAAAAGTCATTTGAGGTAGGTGATGTGACTTCAATGAGGACAAATCATGATCCATACCTTTTAGAACTCAAACCTGACAATGGCAAATGTAGCATTCATTGA

>TRINITY_DN9772_c2_g1_i1.p1 TRINITY_DN9772_c2_g1~~TRINITY_DN9772_c2_g1_i1.p1  ORF type:complete len:104 (-),score=5.48 TRINITY_DN9772_c2_g1_i1:677-988(-)
ATGTCCCTTTTCCCTGCAAGTGTAACAGATTTGTGTGTATGTTCTTATGTCTCTCCTCTCATTAGGGGTCCACTTTGCAGGTTGAACTTGAATCGGGATAACAGAGAGCTGAATCTTCATTTTCTTCCATTCTGCATCCTCTCTAATAACGTTTGTTATCAGATGGAGGAAGACAATGCACTATCAGAAGCTCAGTTGATTAGTGCTATCGAAAAGTCATTTGAGGTAGGTGATGTGACTTCAATGAGGACAAATCATGATCCATACCTTTTAGAACTCAAACCTGACAATGGCAAATGTAGCATTCATTGA




## Australia

Query= Blasia_pusilla_c334273

Length=1026
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

STRG.11460.1.p1 GENE.STRG.11460.1~~STRG.11460.1.p1 ORF type:compl...  466     5e-131

>STRG.11460.1.p1 GENE.STRG.11460.1~~STRG.11460.1.p1  ORF type:complete (+),score=95.20 len:1515 STRG.11460.1:4773-6287(+)
ATGATTGCAAATTGTGTCTTTGTTCAGACGCGTTTGTGTGTACCATCAACTAGTGGAGTTGTTGAGCTGGGGTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTGTTTGCTGAGATGAACGAGATCAGCGGAAATTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGCCTTGCCCAGCAAGTCCCTCTCTGGTCAGCATTCATGGATCAAGAAGGCATGAATCCTTGGACAGGGGACCTGATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTGGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTTGCTGATGTTTCCATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATCTCTGGACCATATCGTGGAAGAAACTCTGGTCAAGTAGACAAGGTCGACTCTATGGTAGTGGTGAAGAAAGCTCAGCCTGGTGTCCAAGCTCAAAATCAAAGGTGTGGAATTGAGGGTGAAATTTTAAATCCACCACCCGATATGAGACAGGGCAGAGGGGAGGTGTTGCATCTGAACAGTGTTTCAACAACTCACATCAAAGAGGAAGGGCCTAGGTCATCAAAGCAAAAGGTCTCTCCATCTATGAAAATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAGTGCTTACGTCCGATCAAGTATTGAGTCTGAACAAGACCTGGATTCAGAGGCAGAAGTTTCATACAAAGAAGTAGAGTGCAGTTTCACTGTTGGACAAAAGCCACCGCGAAAACGTGGAAGAAAGCCTGCAAATGACAGGGAAGAACCTTTAAACCATGTACAGGCTGAACGACAAAGACGAGAGAAGCTCAACCAGCGATTCTACGCCTTGCGCTCTGTTGTGCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTGCAGGATATGGAGTTTCAAATCAAGGAACTCGAGGCTCAAGCTAAATTGCTGACAGAAAAATCATCTGAGACAGAGGTTGGAAGAGGAGGCATGAACAAGTTGTCTAATGAGATTTTACATCCCAAAACCTTTTCTGGCAGAAGCTCAATGACTTCTGGAGTCAAGATTTCTACGGATCAGAGACCAATGGTATCTGTGCATGTCTTGCGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCCGGAATGGCAAATTTGATGTGGACTTTGGAAGATTTAAAGCTGCAGGTTCAGCACGCCAACCTATCAATTCATGCGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAATGCACTATCAGAAGCTCAGTTGATTAGTGCTATCGAAAAGTCATTTGAGGTAGGTGATGTGACTTCAATGAGGACAAATCATGATCCATACCTTTTAGAACTCAAACCTGACAATGGCAAATGTAGCATTCATTGA


SEARCHED FOR THIS IN MALE GENOME FISRT USING BALSIA SEQ FROM ALIGNMENT FILR (BOWMAN) exonerate

Command line: [/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model est2genome --showtargetgff --percent 50 Blasiaseq_from_MpgU00430.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta]
Hostname: [bio226-oct23-ubuntu22]

C4 Alignment:
------------
         Query: Blasia_pusilla_c334273
        Target: HiC_scaffold_9 [revcomp]
         Model: est2genome
     Raw score: 3879
   Query range: 0 -> 1026
  Target range: 21395233 -> 21343514

        1 : TTATGGGACGAG------GCGATGATAGAAGCATT-  >>>> Target Intr :       30
            ||||||||||||      ||||||||||||||||| -+          74 bp
 21395233 : TTATGGGACGAGGATAATGCGATGATAGAAGCATTCat................ : 21395195

       31 : on 1 >>>>  CGAGACCGCGTTACAACAAAAACTCCAGCTTTTGGTTGAGTCA :       72
                     ++|||||| |||||||||||||||||||||||| |||||||||||
 21395194 : .........agCGAGACGGCGTTACAACAAAAACTCCAGCTTCTGGTTGAGTCA : 21395081

       73 : TCGACTGTGAATTGGACCTACGCAATCTTTTGGCAGATTTCGAAT---TCGAGT :      123
            |||| | |||| ||||||||||||||||||||||||||||| |||   ||||||
 21395080 : TCGAGTATGAACTGGACCTACGCAATCTTTTGGCAGATTTCAAATTCATCGAGT : 21395027

      124 : GGAGAGAT  >>>> Target Intron 2 >>>>  GATGCTAGGTTGGGGTG :      148
            ||||||||++         3660 bp         ++|||||||||||||||||
 21395026 : GGAGAGATgt.........................agGATGCTAGGTTGGGGTG : 21391342

      149 : ATGGTTACTTTAAGGGACCAAAAGATAGTGAG  >>>> Target Intron 3 :      181
            | |||||||| |||||||||||||||||||||++          30 bp
 21391341 : ACGGTTACTTCAAGGGACCAAAAGATAGTGAGgt.................... : 21391307

      182 :  >>>>  GACAAGGAAGAAGATCAACAGCTAAGGCGAAAGGTTCTTCGTGAGTT :      227
                 --|||||||||||||||||||||||||||||||||||||||||||||||
 21391306 : .....gtGACAAGGAAGAAGATCAACAGCTAAGGCGAAAGGTTCTTCGTGAGTT : 21391233

      228 : GCAAGCTCTTGTTTGCAGTACAGAAGATCTTGATTATGTAACAGATACAGAGTG :      281
            |||||||||||||||||||||||||||   ||||  ||    || | | |   |
 21391232 : GCAAGCTCTTGTTTGCAGTACAGAAGA---TGATGCTG---GAGGTTCTGGCGG : 21391185

      282 : GTTCTACCTTGTTTC-GATGTCATGTTTCTTTCCTCCTGGTGTTGG  >>>> T :      327
            | |  |   |||  | |||  || |  |  |||  ||| || || |-+
 21391184 : GCTTGATTATGTAACAGAT-ACA-GAGTGGTTCTACCTTGT-TTCGat...... : 21391139

      328 : arget Intron 4 >>>>  GATTCCTGGACAAGCATTGGCAAGTGGAAACTA :      359
               5628 bp         ++|||||||||||||||||||||||||||||||||
 21391138 : ...................agGATTCCTGGACAAGCATTGGCAAGTGGAAACTA : 21385481

      360 : TGTGTGGCTAATGGAAGCTAACAAGGCTCCTAGTAATATTTGCACACAAGCCGA :      413
            |||||||||||||||||| |||||||||||||||| | |||| |||| ||| ||
 21385480 : TGTGTGGCTAATGGAAGCAAACAAGGCTCCTAGTAGTGTTTGTACACGAGCGGA : 21385427

      414 : TCTTGCTAAGAAGGCAGGGATCCAG  >>>> Target Intron 5 >>>>   :      439
            ||||||||||  |   | ||  || ++         9877 bp         ++
 21385426 : TCTTGCTAAG--G--TGTGACTCATgt.........................ag : 21375530

      440 : ACGCGTTTGTGTGTACCATCAACTAGTGGAGTTGTTGAGCTGGGGTCCACTGAT :      492
            |||||| ||||||| ||||||| ||||||||||||||||||||| |||||||||
 21375529 : ACGCGTCTGTGTGTGCCATCAAGTAGTGGAGTTGTTGAGCTGGGCTCCACTGAT : 21375475

      493 : CTA  >>>> Target Intron 6 >>>>  ATTGCAGAGAGTTTGGACATCA :      517
            |||++          434 bp         ++||||||||||||||||||||||
 21375474 : CTAgt.........................agATTGCAGAGAGTTTGGACATCA : 21375016

      518 : TCGAAAATGTTAAGATGGTGTTTGCTGAG  >>>> Target Intron 7 >> :      547
            ||||||||||||||||||| |||||||||-+          666 bp
 21375015 : TCGAAAATGTTAAGATGGTTTTTGCTGAGat....................... : 21374984

      548 : >>  CAAAAGCCACCGCGAAAACGTGGAAGAAAGCCTGCAAATGACAGGGAAGA :      596
              --|||||||||||  |||||||||||||||||||||| ||||||||||||||
 21374983 : ..gaCAAAAGCCACCTAGAAAACGTGGAAGAAAGCCTGCGAATGACAGGGAAGA : 21374271

      597 : ACCTTTAAACCATGTACAGGCTGAACGACAAAGACGAGAGAAGCTCAACCAGCG :      650
            ||||||||| |||||||||||||||||||||||||||||||| |||||||||||
 21374270 : ACCTTTAAATCATGTACAGGCTGAACGACAAAGACGAGAGAAACTCAACCAGCG : 21374217

      651 : ATTCTACGCCTTGCGCTCTGTTGTGCCCAATGTCTCAAAG  >>>> Target  :      691
             ||||| |||||||||||||| || |||||||||||||||++         448
 21374216 : GTTCTATGCCTTGCGCTCTGTCGTTCCCAATGTCTCAAAGgt............ : 21374174

      692 : Intron 8 >>>>  ATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCAT :      729
            6 bp         ++|||||||||||||||||||||||||||||||||||||||
 21374173 : .............agATGGACAAGGCATCCTTGCTAGGTGATGCGATTGCCCAT : 21369652

      730 : ATCCGAGACTTGCACTGCAAGGTGCAGGATATGGAGTTTCAA---AAG  >>>> :      775
            ||||||||||||||||||||||| ||||||||||||||||||   |||+-
 21369651 : ATCCGAGACTTGCACTGCAAGGTACAGGATATGGAGTTTCAAATCAAGga.... : 21369601

      776 :  Target Intron 9 >>>>  ATTTCTACGGATCAGAGACCAATGGTATCTG :      805
                  141 bp         ++|||||||||||||| ||||||||||| ||||
 21369600 : .....................agATTTCTACGGATCAAAGACCAATGGTGTCTG : 21369432

      806 : TGCATGTCTTGCGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCAT :      859
            |||||||||| |||||||||||||||||||||||||||||||||||||||||||
 21369431 : TGCATGTCTTACGAAATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCAT : 21369378

      860 : CCGGAATGGCAAATTTGATGTGGACTTTGGAAGATTTAAAGCTGCAGGTTCAGC :      913
            | ||||| ||||| |||||||||| ||||||||| ||||||||| |||||||||
 21369377 : CTGGAATCGCAAACTTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGC : 21369324

      914 : ACGCCAACCTATCAATTCATGCGGACTCCATGGTCTACATCATCCTTGTAAAA  :      967
            | ||||| ||||||||||||| |||||||||||||||||||||||||||||||+
 21369323 : ATGCCAATCTATCAATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAg : 21369270

      968 :  >>>> Target Intron 10 >>>>  ATGGAGGAAGACAATGCACTATCAG :      991
            +         25696 bp         ++|||||||||||||| |||| |||||
 21369269 : t..........................agATGGAGGAAGACAACGCACCATCAG : 21343550

      992 : AAGTTCAGTTGATTAGTGCTATCGAAAAGTCATTT :     1026
            | |  ||| |||||||||| || ||||||||||||
 21343549 : AGGCGCAGCTGATTAGTGCAATTGAAAAGTCATTT : 21343515

vulgar: Blasia_pusilla_c334273 0 1026 + HiC_scaffold_9 21395233 21343514 - 3879 M 12 12 G 0 6 M 17 17 G 0 1 5 0 2 I 0 70 3 0 2 M 88 88 G 0 3 M 14 14 5 0 2 I 0 3656 3 0 2 M 49 49 5 0 2 I 0 26 3 0 2 M 74 74 G 3 0 M 8 8 G 3 0 M 28 28 G 0 1 M 3 3 G 1 0 M 3 3 G 1 0 M 17 17 G 1 0 M 4 4 5 0 2 I 0 5624 3 0 2 M 97 97 G 2 0 M 1 1 G 2 0 M 10 10 5 0 2 I 0 9873 3 0 2 M 57 57 5 0 2 I 0 430 3 0 2 M 51 51 5 0 2 I 0 662 3 0 2 M 144 144 5 0 2 I 0 4482 3 0 2 M 81 81 G 0 3 M 3 3 5 0 2 I 0 137 3 0 2 M 192 192 5 0 2 I 0 25692 3 0 2 M 60 60
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:est2genome 2.2.0
##date 2024-03-19
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:est2genome    gene    21343515        21395233        3879    -       .       gene_id 1 ; sequence Blasia_pusilla_c334273 ; gene_orientation +
HiC_scaffold_9  exonerate:est2genome    utr5    21395198        21395233        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21395198        21395233        .       -       .       insertions 7 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21395196        21395197        .       -       .       intron_id 1 ; splice_site "AT"
HiC_scaffold_9  exonerate:est2genome    intron  21395124        21395197        .       -       .       intron_id 1
HiC_scaffold_9  exonerate:est2genome    splice3 21395124        21395125        .       -       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21395019        21395123        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21395019        21395123        .       -       .       insertions 3 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21395017        21395018        .       -       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  21391359        21395018        .       -       .       intron_id 2
HiC_scaffold_9  exonerate:est2genome    splice3 21391359        21391360        .       -       .       intron_id 1 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21391310        21391358        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21391310        21391358        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21391308        21391309        .       -       .       intron_id 3 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  21391280        21391309        .       -       .       intron_id 3
HiC_scaffold_9  exonerate:est2genome    splice3 21391280        21391281        .       -       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    utr5    21391142        21391279        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21391142        21391279        .       -       .       insertions 1 ; deletions 9
HiC_scaffold_9  exonerate:est2genome    splice5 21391140        21391141        .       -       .       intron_id 4 ; splice_site "AT"
HiC_scaffold_9  exonerate:est2genome    intron  21385514        21391141        .       -       .       intron_id 4
HiC_scaffold_9  exonerate:est2genome    splice3 21385514        21385515        .       -       .       intron_id 3 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21385406        21385513        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21385406        21385513        .       -       .       insertions 0 ; deletions 4
HiC_scaffold_9  exonerate:est2genome    splice5 21385404        21385405        .       -       .       intron_id 5 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  21375529        21385405        .       -       .       intron_id 5
HiC_scaffold_9  exonerate:est2genome    splice3 21375529        21375530        .       -       .       intron_id 4 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21375472        21375528        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21375472        21375528        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21375470        21375471        .       -       .       intron_id 6 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  21375038        21375471        .       -       .       intron_id 6
HiC_scaffold_9  exonerate:est2genome    splice3 21375038        21375039        .       -       .       intron_id 5 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21374987        21375037        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21374987        21375037        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21374985        21374986        .       -       .       intron_id 7 ; splice_site "AT"
HiC_scaffold_9  exonerate:est2genome    intron  21374321        21374986        .       -       .       intron_id 7
HiC_scaffold_9  exonerate:est2genome    splice3 21374321        21374322        .       -       .       intron_id 6 ; splice_site "GA"
HiC_scaffold_9  exonerate:est2genome    utr5    21374177        21374320        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21374177        21374320        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21374175        21374176        .       -       .       intron_id 8 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  21369691        21374176        .       -       .       intron_id 8
HiC_scaffold_9  exonerate:est2genome    splice3 21369691        21369692        .       -       .       intron_id 7 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21369604        21369690        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21369604        21369690        .       -       .       insertions 3 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21369602        21369603        .       -       .       intron_id 9 ; splice_site "GA"
HiC_scaffold_9  exonerate:est2genome    intron  21369463        21369603        .       -       .       intron_id 9
HiC_scaffold_9  exonerate:est2genome    splice3 21369463        21369464        .       -       .       intron_id 8 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    utr5    21369271        21369462        .       -       .
HiC_scaffold_9  exonerate:est2genome    exon    21369271        21369462        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    splice5 21369269        21369270        .       -       .       intron_id 10 ; splice_site "GT"
HiC_scaffold_9  exonerate:est2genome    intron  21343575        21369270        .       -       .       intron_id 10
HiC_scaffold_9  exonerate:est2genome    splice3 21343575        21343576        .       -       .       intron_id 9 ; splice_site "AG"
HiC_scaffold_9  exonerate:est2genome    exon    21343515        21343574        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:est2genome    similarity      21343515        21395233        3879    -       .       alignment_id 1 ; Query Blasia_pusilla_c334273 ; Align 21395234 1 12 ; Align 21395216 13 17 ; Align 21395124 30 88 ; Align 21395033 118 14 ; Align 21391359 132 49 ; Align 21391280 181 74 ; Align 21391206 258 8 ; Align 21391198 269 28 ; Align 21391169 297 3 ; Align 21391166 301 3 ; Align 21391163 305 17 ; Align 21391146 323 4 ; Align 21385514 327 97 ; Align 21385417 426 1 ; Align 21385416 429 10 ; Align 21375529 439 57 ; Align 21375038 496 51 ; Align 21374321 547 144 ; Align 21369691 691 81 ; Align 21369607 772 3 ; Align 21369463 775 192 ; Align 21343575 967 60
# --- END OF GFF DUMP ---
#
-- completed exonerate analysis


So this gives a valid alignment to the genoem. The genomic seq has a NNNN stretch in the middle the gene does not seem to be complete thoguth.

Therefore, I also decided to try to use the DN1489 Potsdam seq for the alignment which is longer than the seq
Blasiaseq_from_MpgU00430_Potsdamseq.fasta

/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 50 --ryo ">%qi\n%tcs\n" Blasiaseq_from_MpgU00430_Potsdamseq.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta
Command line: [/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 50 --ryo >%qi\n%tcs\n Blasiaseq_from_MpgU00430_Potsdamseq.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta]
Hostname: [bio226-oct23-ubuntu22]

C4 Alignment:
------------
         Query: Postadam_female_TRINITY_DN1489_c0_g2_i3.p1 TRINITY_DN1489_c0_g2~~TRINITY_DN1489_c0_g2_i3.p1  ORF type:complete len:696 (+),score=102.21 TRINITY_DN1489_c0_g2_i3:202-2289(+)
        Target: HiC_scaffold_9 [revcomp]
         Model: coding2genome
     Raw score: 3202
   Query range: 0 -> 2088
  Target range: 21395260 -> 21343424

        1 : ATGGGAACGGATGGAGGTATTCGAAGCTTATGGGACGAGGATAATGCGATGATA :       52
            MetGlyThrAspGlyGlyIleArgSerLeuTrpAspGluAspAsnAlaMetIle
            ||||||||||||||+! !||||||||||||||||||||||||||||||||||||
            MetGlyThrAspGlyValIleArgSerLeuTrpAspGluAspAsnAlaMetIle
 21395260 : ATGGGAACGGATGGCGTTATTCGAAGCTTATGGGACGAGGATAATGCGATGATA : 21395209

       53 : GAAGCATTCATGGGATCGGCTGTGTATAACCTATCTTGTAGCTCTCAAGTGGAT :      106
            GluAlaPheMetGlySerAlaValTyrAsnLeuSerCysSerSerGlnValAsp
            ||||||||||||||||||||||||||||||+||||+||||||||||||||||||
            GluAlaPheMetGlySerAlaValTyrAsnLeuSerCysSerSerGlnValAsp
 21395208 : GAAGCATTCATGGGATCGGCTGTGTATAACTTATCCTGTAGCTCTCAAGTGGAT : 21395155

      107 : TTCCCGGGAGCAGAAAGACCTGCTTACAGCGAGACCGCGTTACAACAAAAACTC :      160
            PheProGlyAlaGluArgProAlaTyrSerGluThrAlaLeuGlnGlnLysLeu
            !!. !!|||.!!|||! !||+||||||||||||||+||||||||||||||||||
            LeuThrGlyThrGluThrProAlaTyrSerGluThrAlaLeuGlnGlnLysLeu
 21395154 : TTGACGGGAACAGAAACACCAGCTTACAGCGAGACGGCGTTACAACAAAAACTC : 21395101

      161 : CAGCTTTTGGTTGAGTCATCGACTGTGAATTGGACCTACGCAATCTTTTGGCAG :      214
            GlnLeuLeuValGluSerSerThrValAsnTrpThrTyrAlaIlePheTrpGln
            ||||||+||||||||||||||!:!:!!||+||||||||||||||||||||||||
            GlnLeuLeuValGluSerSerSerMetAsnTrpThrTyrAlaIlePheTrpGln
 21395100 : CAGCTTCTGGTTGAGTCATCGAGTATGAACTGGACCTACGCAATCTTTTGGCAG : 21395047

      215 : ATTTCGAATTCGTCGAGTGGAGAG{AT}  >>>> Target Intron 1 >>> :      243
            IleSerAsnSerSerSerGlyGlu{Me}
            |||||+|||||+||||||||||||{||}           3660 bp
            IleSerAsnSerSerSerGlyGlu{Me}++
 21395046 : ATTTCAAATTCATCGAGTGGAGAG{AT}gt........................ : 21395016

      244 : >  {G}ATGCTAGGTTGGGGTGATGGTTACTTTAAGGGACCAAAAGATAGTGAG :      289
               {t}MetLeuGlyTrpGlyAspGlyTyrPheLysGlyProLysAspSerGlu
               {|}|||||||||||||||||+||||||||+|||||||||||||||||||||
             ++{t}MetLeuGlyTrpGlyAspGlyTyrPheLysGlyProLysAspSerGlu
 21395015 : .ag{G}ATGCTAGGTTGGGGTGACGGTTACTTCAAGGGACCAAAAGATAGTGAG : 21391312

      290 : GTGAATGATCTGAATCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTA :      343
            ValAsnAspLeuAsnHisLeuGluArgSerAspLysGluGluAspGlnGlnLeu
            |||||||||:!!!!.|||||||||||||||||||||||||||||||||||||||
            ValAsnAspMetLysHisLeuGluArgSerAspLysGluGluAspGlnGlnLeu
 21391311 : GTGAATGATATGAAGCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTA : 21391258

      344 : AGGCGAAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGAA :      397
            ArgArgLysValLeuArgGluLeuGlnAlaLeuValCysSerThrGluAspGlu
            |||||||||||||||||||||||||||||||||||||||||||||||||||!!:
            ArgArgLysValLeuArgGluLeuGlnAlaLeuValCysSerThrGluAspAsp
 21391257 : AGGCGAAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGAT : 21391204

      398 : GCTGGAGCTTCAGCCGGGCTTGATTATGTAACAGATACAGAGTGGTTCTACCTT :      451
            AlaGlyAlaSerAlaGlyLeuAspTyrValThrAspThrGluTrpPheTyrLeu
            ||||||!.!||+!.!|||||||||||||||||||||||||||||||||||||||
            AlaGlyGlySerGlyGlyLeuAspTyrValThrAspThrGluTrpPheTyrLeu
 21391203 : GCTGGAGGTTCTGGCGGGCTTGATTATGTAACAGATACAGAGTGGTTCTACCTT : 21391150

      452 : GTTTCGATGTCATGTTTCTTTCCTCCTGGTGTT{GG}  >>>> Target Int :      489
            ValSerMetSerCysPhePheProProGlyVal{Gl}
            |||||||||||||||||||||||||||||||||{||}           5599 b
            ValSerMetSerCysPhePheProProGlyVal{Gl}++
 21391149 : GTTTCGATGTCATGTTTCTTTCCTCCTGGTGTT{GG}gt............... : 21391110

      490 : ron 2 >>>>  {G}ATTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGG :      526
                        {y}IleProGlyGlnAlaLeuAlaSerGlyAsnTyrValTrp
            p           {|}|||||||||||||||||||||||||||||||||||||||
                      ++{y}IleProGlyGlnAlaLeuAlaSerGlyAsnTyrValTrp
 21391109 : ..........ag{G}ATTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGG : 21385476

      527 : CTAATGGAAGCTAACAAGGCTCCTAGTAATATTTGCACACAAGCCGATCTTGCT :      580
            LeuMetGluAlaAsnLysAlaProSerAsnIleCysThrGlnAlaAspLeuAla
            |||||||||||+|||||||||||||||!:!:!!||+|||!:!||+|||||||||
            LeuMetGluAlaAsnLysAlaProSerSerValCysThrArgAlaAspLeuAla
 21385475 : CTAATGGAAGCAAACAAGGCTCCTAGTAGTGTTTGTACACGAGCGGATCTTGCT : 21385422

      581 : AAG  >>>> Target Intron 3 >>>>  AAGGCAGGGATCCAGACGCGTT :      607
            Lys                             LysAlaGlyIleGlnThrArgL
            |||           9873 bp            ! !..   :!:|||||||||+
            Lys++                         +-TyrValPheValGlnThrArgL
 21385421 : AAGgt.........................atTATGTGTTTGTTCAGACGCGTC : 21375522

      608 : TGTGTGTACCATCAACTAGTGGAGTTGTTGAGCTGGGGTCCACTGATCTA  >> :      658
            euCysValProSerThrSerGlyValValGluLeuGlySerThrAspLeu
            |||||||+||||||!:!||||||||||||||||||||+||||||||||||
            euCysValProSerSerSerGlyValValGluLeuGlySerThrAspLeu++
 21375521 : TGTGTGTGCCATCAAGTAGTGGAGTTGTTGAGCTGGGCTCCACTGATCTAgt.. : 21375469

      659 : >> Target Intron 4 >>>>  ATTGCAGAGAGTTTGGACATCATCGAAAA :      685
                                     IleAlaGluSerLeuAspIleIleGluAs
                    434 bp           |||||||||||||||||||||||||||||
                                   ++IleAlaGluSerLeuAspIleIleGluAs
 21375468 : .......................agATTGCAGAGAGTTTGGACATCATCGAAAA : 21375010

      686 : TGTTAAGATGGTGTTTGCTGAGATGAACGAGATCAGCGGAAATTCCCAGGCACT :      739
            nValLysMetValPheAlaGluMetAsnGluIleSerGlyAsnSerGlnAlaLe
            ||||||||||||+||||||||||||||||||||+||||||||+|||||||||||
            nValLysMetValPheAlaGluMetAsnGluIleSerGlyAsnSerGlnAlaLe
 21375009 : TGTTAAGATGGTTTTTGCTGAGATGAACGAGATTAGCGGAAACTCCCAGGCACT : 21374956

      740 : TCTAATGACTGAAGACTCGGCTTTCTTGCCTTGCCCAGCAAGTCCCTCTCTGGT :      793
            uLeuMetThrGluAspSerAlaPheLeuProCysProAlaSerProSerLeuVa
            |||||||||||||||||||||||||||| !!|||||||||||+|||||||||||
            uLeuMetThrGluAspSerAlaPheLeuAlaCysProAlaSerProSerLeuVa
 21374955 : TCTAATGACTGAAGACTCGGCTTTCTTGGCTTGCCCAGCAAGCCCCTCTCTGGT : 21374902

      794 : CAGCATTCATGGATCAAGAAGGCATGAATCCTTGGACAGGGGACCTGATTCAAG :      847
            lSerIleHisGlySerArgArgHisGluSerLeuAspArgGlyProAspSerSe
            |||||||||||||:!!|||||||||||+||||||||||||||||||||||||||
            lSerIleHisGlyThrArgArgHisGluSerLeuAspArgGlyProAspSerSe
 21374901 : CAGCATTCATGGAACAAGAAGGCATGAGTCCTTGGACAGGGGACCTGATTCAAG : 21374848

      848 : TTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAA :      901
            rSerSerPheArgProIleHisLeuAspLysValAsnSerSerValCysThrAs
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            rSerSerPheArgProIleHisLeuAspLysValAsnSerSerValCysThrAs
 21374847 : TTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAA : 21374794

      902 : TGTGGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTTGCTGATGTTTC :      955
            nValAspSerPheAspTyrLeuPheSerGluAspPheAlaPheAlaAspValSe
            |||+||||||||||||||||||||||||||||||||||||||+|||||||||||
            nValAspSerPheAspTyrLeuPheSerGluAspPheAlaPheAlaAspValSe
 21374793 : TGTAGACTCTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTCGCTGATGTTTC : 21374740

      956 : CATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATCTCTGGACCATATCG :     1009
            rIleAspAsnGluArgGluLeuGlnSerGlyThrThrIleSerGlyProTyrAr
            +||||||||||||||||||||||||||||||||||||||+|||||| !!!::||
            rIleAspAsnGluArgGluLeuGlnSerGlyThrThrIleSerGlySerPheAr
 21374739 : TATTGACAATGAAAGGGAGCTTCAGAGTGGAACTACAATTTCTGGATCATTCCG : 21374686

     1010 : TGGAAGAAACTCTGGTCAAGTAGACAAGGTCGACTCTATGGTAGTGGTGAAGAA :     1063
            gGlyArgAsnSerGlyGlnValAspLysValAspSerMetValValValLysLy
            |||||||!:!||||||||||||||||||||+||||||!!:! !|||||||||||
            gGlyArgSerSerGlyGlnValAspLysValAspSerIleGlyValValLysLy
 21374685 : TGGAAGAAGCTCTGGTCAAGTAGACAAGGTTGACTCTATTGGAGTGGTGAAGAA : 21374632

     1064 : AGCTCAGCCTGGTGTCCAAGCTCAAAATCAAAGGTGTGGAATTGAGGGTGAAAT :     1117
            sAlaGlnProGlyValGlnAlaGlnAsnGlnArgCysGlyIleGluGlyGluIl
            |||||||||||||||+|||||||||||||||||+||||||||+||+||||||||
            sAlaGlnProGlyValGlnAlaGlnAsnGlnArgCysGlyIleGluGlyGluIl
 21374631 : AGCTCAGCCTGGTGTTCAAGCTCAAAATCAAAGATGTGGAATAGAAGGTGAAAT : 21374578

     1118 : TTTAAATCCACCACCCGATATGAGACAGGGCAGAGGGGAGGTGTTGCATCTGAA :     1171
            eLeuAsnProProProAspMetArgGlnGlyArgGlyGluValLeuHisLeuAs
            |||||||||||||||+|||||||||||+|||||| !!||||||||||||! !||
            eLeuAsnProProProAspMetArgGlnGlyArgArgGluValLeuHisProAs
 21374577 : TTTAAATCCACCACCAGATATGAGACAAGGCAGAAGGGAGGTGTTGCATCCGAA : 21374524

     1172 : CAGTGTTTCAACAACTCACATCAAAGAGGAAGGGCCTAGGTCATCAAAGCAAAA :     1225
            nSerValSerThrThrHisIleLysGluGluGlyProArgSerSerLysGlnLy
            +|||||+||||||||||||||||||||+||||||||+|||:!!||||||!!.||
            nSerValSerThrThrHisIleLysGluGluGlyProArgThrSerLysHisLy
 21374523 : TAGTGTGTCAACAACTCACATCAAAGAAGAAGGGCCAAGGACATCAAAGCACAA : 21374470

     1226 : GGTCTCTCCATCTATGAAAATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAG :     1279
            sValSerProSerMetLysIleProHisProGluGlyValGlyProLeuProSe
            ||||||+|||||||||||+|||||||||||||||||||||||||||||||||||
            sValSerProSerMetLysIleProHisProGluGlyValGlyProLeuProSe
 21374469 : GGTCTCGCCATCTATGAAGATCCCACATCCAGAAGGGGTGGGACCTCTTCCCAG : 21374416

     1280 : TGCTTACGTCCGATCAAGTATTGAGTCTGAACAAGACCTGGATTCAGAGGCAGA :     1333
            rAlaTyrValArgSerSerIleGluSerGluGlnAspLeuAspSerGluAlaGl
            ||||||+|||+||||||||||||||||||||||||||+||||||||||||||||
            rAlaTyrValArgSerSerIleGluSerGluGlnAspLeuAspSerGluAlaGl
 21374415 : TGCTTATGTCAGATCAAGTATTGAGTCTGAACAAGACTTGGATTCAGAGGCAGA : 21374362

     1334 : AGTTTCATACAAAGAAGTAGAGTGCAGTTTCACTGTTGGACAAAAGCCACCGCG :     1387
            uValSerTyrLysGluValGluCysSerPheThrValGlyGlnLysProProAr
            ||||||||||||||||||+||||||||||||||||||||||||||||||||++|
            uValSerTyrLysGluValGluCysSerPheThrValGlyGlnLysProProAr
 21374361 : AGTTTCATACAAAGAAGTTGAGTGCAGTTTCACTGTTGGACAAAAGCCACCTAG : 21374308

     1388 : AAAACGTGGAAGAAAGCCTGCAAATGACAGGGAAGAACCTTTAAACCATGTACA :     1441
            gLysArgGlyArgLysProAlaAsnAspArgGluGluProLeuAsnHisValGl
            |||||||||||||||||||||+|||||||||||||||||||||||+||||||||
            gLysArgGlyArgLysProAlaAsnAspArgGluGluProLeuAsnHisValGl
 21374307 : AAAACGTGGAAGAAAGCCTGCGAATGACAGGGAAGAACCTTTAAATCATGTACA : 21374254

     1442 : GGCTGAACGACAAAGACGAGAGAAGCTCAACCAGCGATTCTACGCCTTGCGCTC :     1495
            nAlaGluArgGlnArgArgGluLysLeuAsnGlnArgPheTyrAlaLeuArgSe
            ||||||||||||||||||||||||+|||||||||||+|||||+|||||||||||
            nAlaGluArgGlnArgArgGluLysLeuAsnGlnArgPheTyrAlaLeuArgSe
 21374253 : GGCTGAACGACAAAGACGAGAGAAACTCAACCAGCGGTTCTATGCCTTGCGCTC : 21374200

     1496 : TGTTGTGCCCAATGTCTCAAAG  >>>> Target Intron 5 >>>>  ATG :     1519
            rValValProAsnValSerLys                             Met
            |||+||+|||||||||||||||           4486 bp           |||
            rValValProAsnValSerLys++                         ++Met
 21374199 : TGTCGTTCCCAATGTCTCAAAGgt.........................agATG : 21369690

     1520 : GACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGC :     1573
            AspLysAlaSerLeuLeuGlyAspAlaIleAlaHisIleArgAspLeuHisCys
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            AspLysAlaSerLeuLeuGlyAspAlaIleAlaHisIleArgAspLeuHisCys
 21369689 : GACAAGGCATCCTTGCTAGGTGATGCGATTGCCCATATCCGAGACTTGCACTGC : 21369636

     1574 : AAGGTGCAGGATATGGAGTTTCAAATCAAGGAACTCGAGGCTCAAGCTAAATTG :     1627
            LysValGlnAspMetGluPheGlnIleLysGluLeuGluAlaGlnAlaLysLeu
            |||||+|||||||||||||||||||||||||||||||||!.!|||||+||||||
            LysValGlnAspMetGluPheGlnIleLysGluLeuGluGlyGlnAlaLysLeu
 21369635 : AAGGTACAGGATATGGAGTTTCAAATCAAGGAACTCGAGGGTCAAGCAAAATTG : 21369582

     1628 : CTGACAGAAAAATCATCTGAGACAGAGGTTGGAAGAGGAGGCATGAACAAGTTG :     1681
            LeuThrGluLysSerSerGluThrGluValGlyArgGlyGlyMetAsnLysLeu
            |||! !|||||||||||||||||||||:!!!.!||||||||||||!..||||||
            LeuArgGluLysSerSerGluThrGluIleAlaArgGlyGlyMetThrLysLeu
 21369581 : CTGAGAGAAAAATCATCTGAGACAGAGATTGCAAGAGGAGGCATGACTAAGTTG : 21369528

     1682 : TCTAATGAGATTTTACATCCCAAAACCTTTTCTGGCAGAAGCTCAATGACTTCT :     1735
            SerAsnGluIleLeuHisProLysThrPheSerGlyArgSerSerMetThrSer
            |||!..!!:|||! !|||||+||||||||||||||+ !!|||||+|||||||||
            SerArgAspIleSerHisProLysThrPheSerGlyGlySerSerMetThrSer
 21369527 : TCTAGAGACATTTCACATCCAAAAACCTTTTCTGGTGGAAGCTCGATGACTTCT : 21369474

     1736 : GGAGTCAAGATTTCTACGGATCAGAGACCAATGGTATCTGTGCATGTCTTGCGA :     1789
            GlyValLysIleSerThrAspGlnArgProMetValSerValHisValLeuArg
            |||||||||||||||||||||||+|||||||||||+||||||||||||||+|||
            GlyValLysIleSerThrAspGlnArgProMetValSerValHisValLeuArg
 21369473 : GGAGTCAAGATTTCTACGGATCAAAGACCAATGGTGTCTGTGCATGTCTTACGA : 21369420

     1790 : AATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCCGGAATGGCAAAT :     1843
            AsnGluAlaMetIleArgValHisSerThrLysGluSerSerGlyMetAlaAsn
            |||||||||||||||||||||||||||||||||||||||||+|||!!:|||||+
            AsnGluAlaMetIleArgValHisSerThrLysGluSerSerGlyIleAlaAsn
 21369419 : AATGAGGCAATGATTCGAGTACATAGCACCAAGGAGTCATCTGGAATCGCAAAC : 21369366

     1844 : TTGATGTGGACTTTGGAAGATTTAAAGCTGCAGGTTCAGCACGCCAACCTATCA :     1897
            LeuMetTrpThrLeuGluAspLeuLysLeuGlnValGlnHisAlaAsnLeuSer
            |||||||||!.!||||||!!:|||||||||:!!||||||||+|||||+||||||
            LeuMetTrpAsnLeuGluGluLeuLysLeuGluValGlnHisAlaAsnLeuSer
 21369365 : TTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGCATGCCAATCTATCA : 21369312

     1898 : ATTCATGCGGACTCCATGGTCTACATCATCCTTGTAAAA  >>>> Target I :     1939
            IleHisAlaAspSerMetValTyrIleIleLeuValLys
            ||||||!.!||||||||||||||||||||||||||||||           2569
            IleHisGlyAspSerMetValTyrIleIleLeuValLys++
 21369311 : ATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAgt............. : 21369268

     1940 : ntron 6 >>>>  ATGGAGGAAGACAATGCACTATCAGAAGCTCAGTTGATTA :     1978
                          MetGluGluAspAsnAlaLeuSerGluAlaGlnLeuIleS
            6 bp          ||||||||||||||+|||! !|||||+||+|||+||||||
                        ++MetGluGluAspAsnAlaProSerGluAlaGlnLeuIleS
 21369267 : ............agATGGAGGAAGACAACGCACCATCAGAGGCGCAGCTGATTA : 21343535

     1979 : GTGCTATCGAAAAGTCATTTGAGGTAGGTGATGTGACTTCAATGAGGACAAATC :     2032
            erAlaIleGluLysSerPheGluValGlyAspValThrSerMetArgThrAsnH
            ||||+||+||||||||||||!!:..!|||! !:!:!.. !!!!:!::! !||||
            erAlaIleGluLysSerPheAspThrGlyValIleAsnProIleLysIleAsnH
 21343534 : GTGCAATTGAAAAGTCATTTGACACAGGTGTTATAAACCCAATTAAAATAAATC : 21343481

     2033 : ATGATCCATACCTTTTAGAACTCAAACCTGACAATGGCAAATGTAGCATTCATT :     2086
            isAspProTyrLeuLeuGluLeuLysProAspAsnGlyLysCysSerIleHis*
            ||||||||||||||:!!|||||||||||+||||||||+||||||||+|||||||
            isAspProTyrLeuValGluLeuLysProAspAsnGlyLysCysSerIleHis*
 21343480 : ATGATCCATACCTTGTAGAACTCAAACCCGACAATGGAAAATGTAGTATTCATT : 21343427

     2087 : GA :     2088
            **
            ||
            **
 21343426 : GA : 21343425

vulgar: Postadam_female_TRINITY_DN1489_c0_g2_i3.p1 0 2088 + HiC_scaffold_9 21395260 21343424 - 3202 C 240 240 S 2 2 5 0 2 I 0 3656 3 0 2 S 1 1 C 243 243 S 2 2 5 0 2 I 0 5595 3 0 2 S 1 1 C 96 96 5 0 2 I 0 9869 3 0 2 C 72 72 5 0 2 I 0 430 3 0 2 C 861 861 5 0 2 I 0 4482 3 0 2 C 420 420 5 0 2 I 0 25692 3 0 2 C 150 150
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-03-19
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:coding2genome gene    21343425        21395260        3202    -       .       gene_id 1 ; sequence Postadam_female_TRINITY_DN1489_c0_g2_i3.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     21395019        21395260        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21395019        21395260        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21395017        21395018        .       -       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21391359        21395018        .       -       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 21391359        21391360        .       -       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21391113        21391358        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21391113        21391358        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21391111        21391112        .       -       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21385514        21391112        .       -       .       intron_id 2
HiC_scaffold_9  exonerate:coding2genome splice3 21385514        21385515        .       -       .       intron_id 1 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21385417        21385513        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21385417        21385513        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21385415        21385416        .       -       .       intron_id 3 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21375544        21385416        .       -       .       intron_id 3
HiC_scaffold_9  exonerate:coding2genome splice3 21375544        21375545        .       -       .       intron_id 2 ; splice_site "AT"
HiC_scaffold_9  exonerate:coding2genome cds     21375472        21375543        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21375472        21375543        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21375470        21375471        .       -       .       intron_id 4 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21375038        21375471        .       -       .       intron_id 4
HiC_scaffold_9  exonerate:coding2genome splice3 21375038        21375039        .       -       .       intron_id 3 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21374177        21375037        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21374177        21375037        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21374175        21374176        .       -       .       intron_id 5 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21369691        21374176        .       -       .       intron_id 5
HiC_scaffold_9  exonerate:coding2genome splice3 21369691        21369692        .       -       .       intron_id 4 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21369271        21369690        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21369271        21369690        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 21369269        21369270        .       -       .       intron_id 6 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  21343575        21369270        .       -       .       intron_id 6
HiC_scaffold_9  exonerate:coding2genome splice3 21343575        21343576        .       -       .       intron_id 5 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     21343425        21343574        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    21343425        21343574        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome similarity      21343425        21395260        3202    -       .       alignment_id 1 ; Query Postadam_female_TRINITY_DN1489_c0_g2_i3.p1 ; Align 21395261 1 240 ; Align 21391358 244 243 ; Align 21385513 490 96 ; Align 21375544 586 72 ; Align 21375038 658 861 ; Align 21369691 1519 420 ; Align 21343575 1939 150
# --- END OF GFF DUMP ---
#
>Postadam_female_TRINITY_DN1489_c0_g2_i3.p1
ATGGGAACGGATGGCGTTATTCGAAGCTTATGGGACGAGGATAATGCGATGATAGAAGCATTCATGGGAT
CGGCTGTGTATAACTTATCCTGTAGCTCTCAAGTGGATTTGACGGGAACAGAAACACCAGCTTACAGCGA
GACGGCGTTACAACAAAAACTCCAGCTTCTGGTTGAGTCATCGAGTATGAACTGGACCTACGCAATCTTT
TGGCAGATTTCAAATTCATCGAGTGGAGAGATGATGCTAGGTTGGGGTGACGGTTACTTCAAGGGACCAA
AAGATAGTGAGGTGAATGATATGAAGCATTTGGAGAGAAGTGACAAGGAAGAAGATCAACAGCTAAGGCG
AAAGGTTCTTCGTGAGTTGCAAGCTCTTGTTTGCAGTACAGAAGATGATGCTGGAGGTTCTGGCGGGCTT
GATTATGTAACAGATACAGAGTGGTTCTACCTTGTTTCGATGTCATGTTTCTTTCCTCCTGGTGTTGGGA
TTCCTGGACAAGCATTGGCAAGTGGAAACTATGTGTGGCTAATGGAAGCAAACAAGGCTCCTAGTAGTGT
TTGTACACGAGCGGATCTTGCTAAGTATGTGTTTGTTCAGACGCGTCTGTGTGTGCCATCAAGTAGTGGA
GTTGTTGAGCTGGGCTCCACTGATCTAATTGCAGAGAGTTTGGACATCATCGAAAATGTTAAGATGGTTT
TTGCTGAGATGAACGAGATTAGCGGAAACTCCCAGGCACTTCTAATGACTGAAGACTCGGCTTTCTTGGC
TTGCCCAGCAAGCCCCTCTCTGGTCAGCATTCATGGAACAAGAAGGCATGAGTCCTTGGACAGGGGACCT
GATTCAAGTTCATCCTTTAGACCCATCCATTTGGATAAAGTGAACTCATCAGTCTGTACTAATGTAGACT
CTTTTGACTATCTTTTCTCAGAAGACTTCGCCTTCGCTGATGTTTCTATTGACAATGAAAGGGAGCTTCA
GAGTGGAACTACAATTTCTGGATCATTCCGTGGAAGAAGCTCTGGTCAAGTAGACAAGGTTGACTCTATT
GGAGTGGTGAAGAAAGCTCAGCCTGGTGTTCAAGCTCAAAATCAAAGATGTGGAATAGAAGGTGAAATTT
TAAATCCACCACCAGATATGAGACAAGGCAGAAGGGAGGTGTTGCATCCGAATAGTGTGTCAACAACTCA
CATCAAAGAAGAAGGGCCAAGGACATCAAAGCACAAGGTCTCGCCATCTATGAAGATCCCACATCCAGAA
GGGGTGGGACCTCTTCCCAGTGCTTATGTCAGATCAAGTATTGAGTCTGAACAAGACTTGGATTCAGAGG
CAGAAGTTTCATACAAAGAAGTTGAGTGCAGTTTCACTGTTGGACAAAAGCCACCTAGAAAACGTGGAAG
AAAGCCTGCGAATGACAGGGAAGAACCTTTAAATCATGTACAGGCTGAACGACAAAGACGAGAGAAACTC
AACCAGCGGTTCTATGCCTTGCGCTCTGTCGTTCCCAATGTCTCAAAGATGGACAAGGCATCCTTGCTAG
GTGATGCGATTGCCCATATCCGAGACTTGCACTGCAAGGTACAGGATATGGAGTTTCAAATCAAGGAACT
CGAGGGTCAAGCAAAATTGCTGAGAGAAAAATCATCTGAGACAGAGATTGCAAGAGGAGGCATGACTAAG
TTGTCTAGAGACATTTCACATCCAAAAACCTTTTCTGGTGGAAGCTCGATGACTTCTGGAGTCAAGATTT
CTACGGATCAAAGACCAATGGTGTCTGTGCATGTCTTACGAAATGAGGCAATGATTCGAGTACATAGCAC
CAAGGAGTCATCTGGAATCGCAAACTTGATGTGGAATTTGGAAGAGTTAAAGCTGGAGGTTCAGCATGCC
AATCTATCAATTCATGGGGACTCCATGGTCTACATCATCCTTGTAAAAATGGAGGAAGACAACGCACCAT
CAGAGGCGCAGCTGATTAGTGCAATTGAAAAGTCATTTGACACAGGTGTTATAAACCCAATTAAAATAAA
TCATGATCCATACCTTGTAGAACTCAAACCCGACAATGGAAAATGTAGTATTCATTGA

This seems to be suüer similar to the female sequence!!!!




# MpUg00410  Here the male sequence was identical with the female sequence, we run exonerate to see whether something else could be recovered!!!

 /home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 80 --ryo ">%qi\n%tcs\n" Blasiaseq_from_MpgU00410_Postdamfemale.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta
Command line: [/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 80 --ryo >%qi\n%tcs\n Blasiaseq_from_MpgU00410_Postdamfemale.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta]
Hostname: [bio226-oct23-ubuntu22]

C4 Alignment:
------------
         Query: Potsdam_female_asmbl_38346.p1 GENE.asmbl_38346~~asmbl_38346.p1  ORF type:complete len:442 (+),score=87.49 asmbl_38346:66-1391(+)
        Target: HiC_scaffold_9
         Model: coding2genome
     Raw score: 2227
   Query range: 2 -> 1325
  Target range: 16044354 -> 16051356

        3 : GTTCTCTTCTTTTAAGACCACGAGTTTTATGAGCACGAGCATGAGGGGAGGCTC :       54
            ValLeuPhePhe***AspHisGluPheTyrGluHisGluHisGluGlyArgLeu
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            ValLeuPhePhe***AspHisGluPheTyrGluHisGluHisGluGlyArgLeu
 16044355 : GTTCTCTTCTTTTAAGACCACGAGTTTTATGAGCACGAGCATGAGGGGAGGCTC : 16044406

       55 : ATCACCTATATCGAGAAAATCGCTGAATTTACCTCCTGCCAGGTCCAGCGCCTC :      108
            IleThrTyrIleGluLysIleAlaGluPheThrSerCysGlnValGlnArgLeu
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            IleThrTyrIleGluLysIleAlaGluPheThrSerCysGlnValGlnArgLeu
 16044407 : ATCACCTATATCGAGAAAATCGCTGAATTTACCTCCTGCCAGGTCCAGCGCCTC : 16044460

      109 : AACAGCGGCGTCAGCAGCTGGATCCAAGAAGGCCAGCACCCATCAGCCGCAGCA :      162
            AsnSerGlyValSerSerTrpIleGlnGluGlyGlnHisProSerAlaAlaAla
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            AsnSerGlyValSerSerTrpIleGlnGluGlyGlnHisProSerAlaAlaAla
 16044461 : AACAGCGGCGTCAGCAGCTGGATCCAAGAAGGCCAGCACCCATCAGCCGCAGCA : 16044514

      163 : TCAGCAAGAGCAACATTCGGCGGAGGAGGAGCCCAAGACGGCCGAAACAACGAC :      216
            SerAlaArgAlaThrPheGlyGlyGlyGlyAlaGlnAspGlyArgAsnAsnAsp
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            SerAlaArgAlaThrPheGlyGlyGlyGlyAlaGlnAspGlyArgAsnAsnAsp
 16044515 : TCAGCAAGAGCAACATTCGGCGGAGGAGGAGCCCAAGACGGCCGAAACAACGAC : 16044568

      217 : AAGTGCTGCAGCATCTATTGCTGCAGCGGCAGCAGCAGCAGTGGGAGGAGAAGG :      270
            LysCysCysSerIleTyrCysCysSerGlySerSerSerSerGlyArgArgArg
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            LysCysCysSerIleTyrCysCysSerGlySerSerSerSerGlyArgArgArg
 16044569 : AAGTGCTGCAGCATCTATTGCTGCAGCGGCAGCAGCAGCAGTGGGAGGAGAAGG : 16044622

      271 : AGCTGCAGCTACAGAGCCGCGACCACCATCGCCGGGTCCCGGCACCGGGGGACG :      324
            SerCysSerTyrArgAlaAlaThrThrIleAlaGlySerArgHisArgGlyThr
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            SerCysSerTyrArgAlaAlaThrThrIleAlaGlySerArgHisArgGlyThr
 16044623 : AGCTGCAGCTACAGAGCCGCGACCACCATCGCCGGGTCCCGGCACCGGGGGACG : 16044676

      325 : GAGGGGAGAAGGTTCAGGCTCTCTCGCGAATCCGACCTCCTCATCTGCAGGGCT :      378
            GluGlyArgArgPheArgLeuSerArgGluSerAspLeuLeuIleCysArgAla
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            GluGlyArgArgPheArgLeuSerArgGluSerAspLeuLeuIleCysArgAla
 16044677 : GAGGGGAGAAGGTTCAGGCTCTCTCGCGAATCCGACCTCCTCATCTGCAGGGCT : 16044730

      379 : CAACGA{G}  >>>> Target Intron 1 >>>>  {AT}CATGCCTTCGTC :      399
            GlnArg{A}                             {sp}HisAlaPheVal
            ||||||{|}           2374 bp           {||}||||||||||||
            GlnArg{A}++                         ++{sp}HisAlaPheVal
 16044731 : CAACGA{G}gt.........................ag{AT}CATGCCTTCGTC : 16047125

      400 : GGAGGCGGAGGCAGAGGCAGAAGCAGAGGCCGGGGTTACTTCCTTACCCCTTGA :      453
            GlyGlyGlyGlyArgGlyArgSerArgGlyArgGlyTyrPheLeuThrPro***
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            GlyGlyGlyGlyArgGlyArgSerArgGlyArgGlyTyrPheLeuThrPro***
 16047126 : GGAGGCGGAGGCAGAGGCAGAAGCAGAGGCCGGGGTTACTTCCTTACCCCTTGA : 16047179

      454 : AGACGAGGACATTCAAAGTGAGCAGGGGAGTGAACTGAGTGACGGAGGCCTGGA :      507
            ArgArgGlyHisSerLys***AlaGlyGlu***ThrGlu***ArgArgProGly
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            ArgArgGlyHisSerLys***AlaGlyGlu***ThrGlu***ArgArgProGly
 16047180 : AGACGAGGACATTCAAAGTGAGCAGGGGAGTGAACTGAGTGACGGAGGCCTGGA : 16047233

      508 : AAAGCTCTCCCTTGGTCCACGGTACCTGAAGGGGGAAGCTGATCGTTACGATGG :      561
            LysAlaLeuProTrpSerThrValProGluGlyGlySer***SerLeuArgTrp
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            LysAlaLeuProTrpSerThrValProGluGlyGlySer***SerLeuArgTrp
 16047234 : AAAGCTCTCCCTTGGTCCACGGTACCTGAAGGGGGAAGCTGATCGTTACGATGG : 16047287

      562 : AATCATCGTTGATCCTAGCTGCTTACCTGCGGACATACCCACGTTTGTCGATGC :      615
            AsnHisArg***Ser***LeuLeuThrCysGlyHisThrHisValCysArgCys
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            AsnHisArg***Ser***LeuLeuThrCysGlyHisThrHisValCysArgCys
 16047288 : AATCATCGTTGATCCTAGCTGCTTACCTGCGGACATACCCACGTTTGTCGATGC : 16047341

      616 : CCTCCGTGCGTCGCTGGACTTGTGGAAATCACA{G}  >>>> Target Intr :      652
            ProProCysValAlaGlyLeuValGluIleThr{G}
            |||||||||||||||||||||||||||||||||{|}            104 bp
            ProProCysValAlaGlyLeuValGluIleThr{G}++
 16047342 : CCTCCGTGCGTCGCTGGACTTGTGGAAATCACA{G}gt................ : 16047380

      653 : on 2 >>>>  {GA}AAGGAAAGGAGTGTGGCTCAAATTACCCACAGCCAATGC :      690
                       {ly}LysGluArgSerValAlaGlnIleThrHisSerGlnCys
                       {||}|||||||||||||||||||||||||||||||||||||||
                     ++{ly}LysGluArgSerValAlaGlnIleThrHisSerGlnCys
 16047381 : .........ag{GA}AAGGAAAGGAGTGTGGCTCAAATTACCCACAGCCAATGC : 16047520

      691 : CAATCTTGTGTTTCCAGCAATTGA{G}  >>>> Target Intron 3 >>>> :      718
            GlnSerCysValSerSerAsn***{G}
            ||||||||||||||||||||||||{|}            323 bp
            GlnSerCysValSerSerAsn***{G}++
 16047521 : CAATCTTGTGTTTCCAGCAATTGA{G}gt......................... : 16047550

      719 :   {GC}AGGCTTTGGTTATCACCACGCTGAGCCGACTTATGTAATGCTGACGTA :      765
              {ly}ArgLeuTrpLeuSerProArg***AlaAspLeuCysAsnAlaAspVal
              {||}||||||||||||||||||||||||||||||||||||||||||||||||
            ++{ly}ArgLeuTrpLeuSerProArg***AlaAspLeuCysAsnAlaAspVal
 16047551 : ag{GC}AGGCTTTGGTTATCACCACGCTGAGCCGACTTATGTAATGCTGACGTA : 16047918

      766 : TTGGATACCTGAAAGTCCGTGTACAATCCCATCAAATGCCTCACATCAAGTTGG :      819
            LeuAspThr***LysSerValTyrAsnProIleLysCysLeuThrSerSerTrp
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            LeuAspThr***LysSerValTyrAsnProIleLysCysLeuThrSerSerTrp
 16047919 : TTGGATACCTGAAAGTCCGTGTACAATCCCATCAAATGCCTCACATCAAGTTGG : 16047972

      820 : TGTTGGGGCTTTTGTGCTGAATGACCAAAGACA{G}  >>>> Target Intr :      856
            CysTrpGlyPheCysAlaGlu***ProLysThr{G}
            |||||||||||||||||||||||||||||||||{|}            833 bp
            CysTrpGlyPheCysAlaGlu***ProLysThr{G}++
 16047973 : TGTTGGGGCTTTTGTGCTGAATGACCAAAGACA{G}gt................ : 16048011

      857 : on 4 >>>>  {GT}GCTTGCAGTGCAAGAAAAGAATGGACCTTTGAGAGGTTC :      894
                       {ly}AlaCysSerAlaArgLysGluTrpThrPheGluArgPhe
                       {||}|||||||||||||||||||||||||||||||||||||||
                     ++{ly}AlaCysSerAlaArgLysGluTrpThrPheGluArgPhe
 16048012 : .........ag{GT}GCTTGCAGTGCAAGAAAAGAATGGACCTTTGAGAGGTTC : 16048880

      895 : AGGAATCTGGAAGATGCCGACAGGATTGATTAATCA{G}  >>>> Target I :      934
            ArgAsnLeuGluAspAlaAspArgIleAsp***Ser{G}
            ||||||||||||||||||||||||||||||||||||{|}            447
            ArgAsnLeuGluAspAlaAspArgIleAsp***Ser{G}++
 16048881 : AGGAATCTGGAAGATGCCGACAGGATTGATTAATCA{G}gt............. : 16048922

      935 : ntron 5 >>>>  {GG}TGAAGATATTCACGATGGTGCTGTGAGGGAAGTGAA :      969
                          {ly}***ArgTyrSerArgTrpCysCysGluGlySerGlu
             bp           {||}||||||||||||||||||||||||||||||||||||
                        ++{ly}***ArgTyrSerArgTrpCysCysGluGlySerGlu
 16048923 : ............ag{GG}TGAAGATATTCACGATGGTGCTGTGAGGGAAGTGAA : 16049402

      970 : GGAGGAAACAGG{G}  >>>> Target Intron 6 >>>>  {GT}GGATAC :      990
            GlyGlyAsnArg{G}                             {ly}GlyTyr
            ||||||||||||{|}            828 bp           {||}||||||
            GlyGlyAsnArg{G}++                         ++{ly}GlyTyr
 16049403 : GGAGGAAACAGG{G}gt.........................ag{GT}GGATAC : 16050251

      991 : AGAGTTTTTGGAAGTTATTGGATTCCG  >>>> Target Intron 7 >>>> :     1020
            ArgValPheGlySerTyrTrpIlePro
            |||||||||||||||||||||||||||            237 bp
            ArgValPheGlySerTyrTrpIlePro++
 16050252 : AGAGTTTTTGGAAGTTATTGGATTCCGgt......................... : 16050283

     1021 :   GCAAGGGCATCAAGTATCGTTTGAAAAGTCAGATCTGTTCTTCCTGTGTGTT :     1071
              AlaArgAlaSerSerIleVal***LysValArgSerValLeuProValCysS
              ||||||||||||||||||||||||||||||||||||||||||||||||||||
            ++AlaArgAlaSerSerIleVal***LysValArgSerValLeuProValCysS
 16050284 : agGCAAGGGCATCAAGTATCGTTTGAAAAGTCAGATCTGTTCTTCCTGTGTGTT : 16050569

     1072 : CTACGACCGCTTTCAAGCACCATAGTGAAACAAGATTCGGAAATTGAAGCAGCA :     1125
            erThrThrAlaPheLysHisHisSerGluThrArgPheGlyAsn***SerSerT
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            erThrThrAlaPheLysHisHisSerGluThrArgPheGlyAsn***SerSerT
 16050570 : CTACGACCGCTTTCAAGCACCATAGTGAAACAAGATTCGGAAATTGAAGCAGCA : 16050623

     1126 : CA{G}  >>>> Target Intron 8 >>>>  {TG}GATGCCGTTGGACGAA :     1146
            hr{V}                             {al}AspAlaValGlyArgI
            ||{|}            533 bp           {||}||||||||||||||||
            hr{V}++                         ++{al}AspAlaValGlyArgI
 16050624 : CA{G}gt.........................ag{TG}GATGCCGTTGGACGAA : 16051177

     1147 : TTTGCAGTACAACCTTTCTTCCAAAGAAGCATGCTGCGAAAAATGCTGGATATT :     1200
            leCysSerThrThrPheLeuProLysLysHisAlaAlaLysAsnAlaGlyTyrL
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            leCysSerThrThrPheLeuProLysLysHisAlaAlaLysAsnAlaGlyTyrL
 16051178 : TTTGCAGTACAACCTTTCTTCCAAAGAAGCATGCTGCGAAAAATGCTGGATATT : 16051231

     1201 : TGCGTGGCAAGCACTGAAGGCCAATACAGAGGATTTGGTAGCCGAGCATTGCAT :     1254
            euArgGlyLysHis***ArgProIleGlnArgIleTrp***ProSerIleAlaC
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            euArgGlyLysHis***ArgProIleGlnArgIleTrp***ProSerIleAlaC
 16051232 : TGCGTGGCAAGCACTGAAGGCCAATACAGAGGATTTGGTAGCCGAGCATTGCAT : 16051285

     1255 : GCTGGTTTCCAAAGGTTGCCTTCGTATTTTTACTACAATGTACAAGACTCCAAG :     1308
            ysTrpPheProLysValAlaPheValPheLeuLeuGlnCysThrArgLeuGlnG
            ||||||||||||||||||||||||||||||||||||||||||||||||||||||
            ysTrpPheProLysValAlaPheValPheLeuLeuGlnCysThrArgLeuGlnG
 16051286 : GCTGGTTTCCAAAGGTTGCCTTCGTATTTTTACTACAATGTACAAGACTCCAAG : 16051339

     1309 : GAAAACTCGAACCTGTA :     1325
            lyLysLeuGluProVal
            |||||||||||||||||
            lyLysLeuGluProVal
 16051340 : GAAAACTCGAACCTGTA : 16051356

vulgar: Potsdam_female_asmbl_38346.p1 2 1325 + HiC_scaffold_9 16044354 16051356 + 2227 C 384 384 S 1 1 5 0 2 I 0 2370 3 0 2 S 2 2 C 261 261 S 1 1 5 0 2 I 0 100 3 0 2 S 2 2 C 63 63 S 1 1 5 0 2 I 0 319 3 0 2 S 2 2 C 135 135 S 1 1 5 0 2 I 0 829 3 0 2 S 2 2 C 75 75 S 1 1 5 0 2 I 0 443 3 0 2 S 2 2 C 48 48 S 1 1 5 0 2 I 0 824 3 0 2 S 2 2 C 33 33 5 0 2 I 0 233 3 0 2 C 108 108 S 1 1 5 0 2 I 0 529 3 0 2 S 2 2 C 195 195
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-03-19
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:coding2genome gene    16044355        16051356        2227    +       .       gene_id 1 ; sequence Potsdam_female_asmbl_38346.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     16044355        16044739        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16044355        16044739        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16044740        16044741        .       +       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16044740        16047113        .       +       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 16047112        16047113        .       +       .       intron_id 0 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16047114        16047377        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16047114        16047377        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16047378        16047379        .       +       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16047378        16047481        .       +       .       intron_id 2
HiC_scaffold_9  exonerate:coding2genome splice3 16047480        16047481        .       +       .       intron_id 1 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16047482        16047547        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16047482        16047547        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16047548        16047549        .       +       .       intron_id 3 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16047548        16047870        .       +       .       intron_id 3
HiC_scaffold_9  exonerate:coding2genome splice3 16047869        16047870        .       +       .       intron_id 2 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16047871        16048008        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16047871        16048008        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16048009        16048010        .       +       .       intron_id 4 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16048009        16048841        .       +       .       intron_id 4
HiC_scaffold_9  exonerate:coding2genome splice3 16048840        16048841        .       +       .       intron_id 3 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16048842        16048919        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16048842        16048919        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16048920        16048921        .       +       .       intron_id 5 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16048920        16049366        .       +       .       intron_id 5
HiC_scaffold_9  exonerate:coding2genome splice3 16049365        16049366        .       +       .       intron_id 4 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16049367        16049417        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16049367        16049417        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16049418        16049419        .       +       .       intron_id 6 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16049418        16050245        .       +       .       intron_id 6
HiC_scaffold_9  exonerate:coding2genome splice3 16050244        16050245        .       +       .       intron_id 5 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16050246        16050280        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16050246        16050280        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16050281        16050282        .       +       .       intron_id 7 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16050281        16050517        .       +       .       intron_id 7
HiC_scaffold_9  exonerate:coding2genome splice3 16050516        16050517        .       +       .       intron_id 6 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16050518        16050626        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16050518        16050626        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 16050627        16050628        .       +       .       intron_id 8 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  16050627        16051159        .       +       .       intron_id 8
HiC_scaffold_9  exonerate:coding2genome splice3 16051158        16051159        .       +       .       intron_id 7 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     16051160        16051356        .       +       .
HiC_scaffold_9  exonerate:coding2genome exon    16051160        16051356        .       +       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome similarity      16044355        16051356        2227    +       .       alignment_id 1 ; Query Potsdam_female_asmbl_38346.p1 ; Align 16044355 3 384 ; Align 16047116 390 261 ; Align 16047484 654 63 ; Align 16047873 720 135 ; Align 16048844 858 75 ; Align 16049369 936 48 ; Align 16050248 987 33 ; Align 16050518 1020 108 ; Align 16051162 1131 195
# --- END OF GFF DUMP ---
#
>Potsdam_female_asmbl_38346.p1
GTTCTCTTCTTTTAAGACCACGAGTTTTATGAGCACGAGCATGAGGGGAGGCTCATCACCTATATCGAGA
AAATCGCTGAATTTACCTCCTGCCAGGTCCAGCGCCTCAACAGCGGCGTCAGCAGCTGGATCCAAGAAGG
CCAGCACCCATCAGCCGCAGCATCAGCAAGAGCAACATTCGGCGGAGGAGGAGCCCAAGACGGCCGAAAC
AACGACAAGTGCTGCAGCATCTATTGCTGCAGCGGCAGCAGCAGCAGTGGGAGGAGAAGGAGCTGCAGCT
ACAGAGCCGCGACCACCATCGCCGGGTCCCGGCACCGGGGGACGGAGGGGAGAAGGTTCAGGCTCTCTCG
CGAATCCGACCTCCTCATCTGCAGGGCTCAACGAGATCATGCCTTCGTCGGAGGCGGAGGCAGAGGCAGA
AGCAGAGGCCGGGGTTACTTCCTTACCCCTTGAAGACGAGGACATTCAAAGTGAGCAGGGGAGTGAACTG
AGTGACGGAGGCCTGGAAAAGCTCTCCCTTGGTCCACGGTACCTGAAGGGGGAAGCTGATCGTTACGATG
GAATCATCGTTGATCCTAGCTGCTTACCTGCGGACATACCCACGTTTGTCGATGCCCTCCGTGCGTCGCT
GGACTTGTGGAAATCACAGGAAAGGAAAGGAGTGTGGCTCAAATTACCCACAGCCAATGCCAATCTTGTG
TTTCCAGCAATTGAGGCAGGCTTTGGTTATCACCACGCTGAGCCGACTTATGTAATGCTGACGTATTGGA
TACCTGAAAGTCCGTGTACAATCCCATCAAATGCCTCACATCAAGTTGGTGTTGGGGCTTTTGTGCTGAA
TGACCAAAGACAGGTGCTTGCAGTGCAAGAAAAGAATGGACCTTTGAGAGGTTCAGGAATCTGGAAGATG
CCGACAGGATTGATTAATCAGGGTGAAGATATTCACGATGGTGCTGTGAGGGAAGTGAAGGAGGAAACAG
GGGTGGATACAGAGTTTTTGGAAGTTATTGGATTCCGGCAAGGGCATCAAGTATCGTTTGAAAAGTCAGA
TCTGTTCTTCCTGTGTGTTCTACGACCGCTTTCAAGCACCATAGTGAAACAAGATTCGGAAATTGAAGCA
GCACAGTGGATGCCGTTGGACGAATTTGCAGTACAACCTTTCTTCCAAAGAAGCATGCTGCGAAAAATGC
TGGATATTTGCGTGGCAAGCACTGAAGGCCAATACAGAGGATTTGGTAGCCGAGCATTGCATGCTGGTTT
CCAAAGGTTGCCTTCGTATTTTTACTACAATGTACAAGACTCCAAGGAAAACTCGAACCTGTA

This is exactly the same seqe which we already have in the aligment.






# MpUg00090

There was no male seuqnece available in the alignment. I took the postdam transcuprt seq and did an exonertae search

/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 50 --ryo ">%qi\n%tcs\n" Blasiaseq_from_MpgU00090_Postdam.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta
Command line: [/home/ubuntu/USERS/Peter/bio226_2/Peter_II/BLAST/exonerate-2.2.0-x86_64/bin/./exonerate --model coding2genome --showtargetgff --percent 50 --ryo >%qi\n%tcs\n Blasiaseq_from_MpgU00090_Postdam.fasta assembly.fasta.PolcaCorrected.FINAL_9scaffolds.fasta]
Hostname: [bio226-oct23-ubuntu22]

C4 Alignment:
------------
         Query: Postdam_female_TRINITY_DN3906_c0_g2_i7.p1 TRINITY_DN3906_c0_g2~~TRINITY_DN3906_c0_g2_i7.p1  ORF type:complete len:672 (+),score=131.23 TRINITY_DN3906_c0_g2_i7:585-2600(+)
        Target: HiC_scaffold_9 [revcomp]
         Model: coding2genome
     Raw score: 2203
   Query range: 222 -> 2016
  Target range: 27117522 -> 27101873

      223 : AGATCGTCTCCTAAAAGAATCCAGGGCCTTGATGCAAGAAGCTTACGGCTTCAG :      274
            ArgSerSerProLysArgIleGlnGlyLeuAspAlaArgSerLeuArgLeuGln
            |||||+|||||||||||||||||+||+!  |||:!:||+!:!+|+||+||+|||
            ArgSerSerProLysArgIleGlnGlyProAspSerArgAsnLeuArgLeuGln
 27117522 : AGATCATCTCCTAAAAGAATCCAAGGTCCCGATTCTAGGAACCTGCGCCTACAG : 27117471

      275 : TTCAGAAATAAACTGGCTCTTCCTTTATTCACAGGGAGCAAGGTTGAAGGTGAA :      328
            PheArgAsnLysLeuAlaLeuProLeuPheThrGlySerLysValGluGlyGlu
            ||++|+||+|||+||||+||||||||+||+||+||+||||||||+||||||||+
            PheArgAsnLysLeuAlaLeuProLeuPheThrGlySerLysValGluGlyGlu
 27117470 : TTTCGTAACAAATTGGCCCTTCCTTTGTTTACTGGAAGCAAGGTAGAAGGTGAG : 27117417

      329 : CAAGGATCAACTATTCATGTTGTCTTGCAAGATGCAACCTCGGGTCAGGTTGTG :      382
            GlnGlySerThrIleHisValValLeuGlnAspAlaThrSerGlyGlnValVal
            |||||+|||.!.||+|||||+||++|+||||||||+||+:!:||+||+||+||+
            GlnGlySerAlaIleHisValValLeuGlnAspAlaThrAlaGlyGlnValVal
 27117416 : CAAGGGTCAGCCATACATGTAGTGCTACAAGATGCCACTGCTGGCCAAGTGGTA : 27117363

      383 : ACAGGGGGTGCAGAAGCCTCAGCAAAGCTTGAAGTAGTGGTACTTGAAGGTGAT :      436
            ThrGlyGlyAlaGluAlaSerAlaLysLeuGluValValValLeuGluGlyAsp
            |||!..||+||+||+|||||+||+|||||||||||+|||||++|+|||||+||+
            ThrAlaGlyAlaGluAlaSerAlaLysLeuGluValValValLeuGluGlyAsp
 27117362 : ACAGCTGGAGCTGAGGCCTCTGCCAAGCTTGAAGTTGTGGTCTTAGAAGGGGAC : 27117309

      437 : TTTTCGGCAGATGATGAAGAGGACTGGCCACAAGAAGAATTTGAAAACCATGAA :      490
            PheSerAlaAspAspGluGluAspTrpProGlnGluGluPheGluAsnHisGlu
            |||||+|||||||||||+|||||||||! !||+||+||+|||||+||+||||||
            PheSerAlaAspAspGluGluAspTrpLeuGlnGluGluPheGluAsnHisGlu
 27117308 : TTTTCTGCAGATGATGAGGAGGACTGGCTACAGGAGGAGTTTGAGAATCATGAA : 27117255

      491 : GTTCGTGAGCGTGATGGTAAGAGACCTCTTTTGACAGGAGATCTTTTTGTGACT :      544
            ValArgGluArgAspGlyLysArgProLeuLeuThrGlyAspLeuPheValThr
            ||++|+||+||+|||||+||||||||+||||||||+||+||||||!  ||+|||
            ValArgGluArgAspGlyLysArgProLeuLeuThrGlyAspLeuSerValThr
 27117254 : GTAAGAGAACGAGATGGAAAGAGACCCCTTTTGACTGGGGATCTTTCAGTAACT : 27117201

      545 : TTGAAAGAAGGTGTGGGAACTCTTGGAGAACTGACATTCACAGACAATTCGAGC :      598
            LeuLysGluGlyValGlyThrLeuGlyGluLeuThrPheThrAspAsnSerSer
            +|+||||||||+||+||+||+|||||+||++|||||||+|||||+||+||+||+
            LeuLysGluGlyValGlyThrLeuGlyGluLeuThrPheThrAspAsnSerSer
 27117200 : CTTAAAGAAGGGGTTGGCACCCTTGGGGAGTTGACATTTACAGATAACTCAAGT : 27117147

      599 : TGGATAAGGAGCAGAAAGTTTCGGCTTGGAGTAAAAATTTCCTCAGGGTACTGT :      652
            TrpIleArgSerArgLysPheArgLeuGlyValLysIleSerSerGlyTyrCys
            |||||++||||||||||||||||+||+||+||+||||||! !||+||+||||||
            TrpIleArgSerArgLysPheArgLeuGlyValLysIleCysSerGlyTyrCys
 27117146 : TGGATCCGGAGCAGAAAGTTTCGTCTGGGTGTGAAAATTTGCTCCGGATACTGT : 27117093

      653 : GAGGGACTCCGAATCCGAGAGGCAAAGACTGAGGCTTTTACTGTTAAAGACCAT :      706
            GluGlyLeuArgIleArgGluAlaLysThrGluAlaPheThrValLysAspHis
            ||+|||+|+||+||+||+|||||+||+||+||+|||||+||+||+||+||||||
            GluGlyLeuArgIleArgGluAlaLysThrGluAlaPheThrValLysAspHis
 27117092 : GAAGGATTACGCATTCGCGAGGCCAAAACAGAAGCTTTCACAGTGAAGGACCAT : 27117039

      707 : CGT{G}  >>>> Target Intron 1 >>>>  {GA}GAATTGTATAAAAAG :      727
            Arg{G}                             {ly}GluLeuTyrLysLys
            |||{|}            251 bp           {||}:!!:!!||+||+|||
            Arg{G}++                         -+{ly}LysValTyrLysLys
 27117038 : CGT{G}gt.........................gg{GA}AAAGTGTACAAGAAG : 27116767

      728 : CATTACCCACCAGCATTGCACGATGAAGTGTGGCGTCTTGACAAGATTGGGAAG :      781
            HisTyrProProAlaLeuHisAspGluValTrpArgLeuAspLysIleGlyLys
            ||+||+||+||+||+||+|||||||||||+|||||++|+|||||+||+||+||+
            HisTyrProProAlaLeuHisAspGluValTrpArgLeuAspLysIleGlyLys
 27116766 : CACTATCCTCCTGCCTTACACGATGAAGTTTGGCGGTTGGACAAAATCGGTAAA : 27116713

      782 : GATGGTGCTTTCCACAAGCGACTAAACCAGTCTGGAATACAGACTGTGGAGGAC :      835
            AspGlyAlaPheHisLysArgLeuAsnGlnSerGlyIleGlnThrValGluAsp
            |||||||||||+||||||+||+|+||||||||+|||||+!  ||+||+||+|||
            AspGlyAlaPheHisLysArgLeuAsnGlnSerGlyIleLeuThrValGluAsp
 27116712 : GATGGTGCTTTTCACAAGAGATTGAACCAGTCCGGAATCCTCACAGTAGAAGAC : 27116659

      836 : TTTTTAAGGCTTGTCGTAATGGATCCTCAAAAACTTCGCAAT  >>>> Targe :      880
            PheLeuArgLeuValValMetAspProGlnLysLeuArgAsn
            ||++|++||||+||+||||||||||||||+||+||+||||||           1
            PheLeuArgLeuValValMetAspProGlnLysLeuArgAsn++
 27116658 : TTCCTCCGGCTGGTGGTAATGGATCCTCAGAAGCTGCGCAATgt.......... : 27116612

      881 : t Intron 2 >>>>  ATTTTGGGCAATGGAATGTCCAACAAGATGTGGGAGG :      916
                             IleLeuGlyAsnGlyMetSerAsnLysMetTrpGluG
            3333 bp          |||+|+||+|||||+|||||+||||||||||||||+|
                           ++IleLeuGlyAsnGlyMetSerAsnLysMetTrpGluG
 27116611 : ...............agATTCTCGGGAATGGCATGTCGAACAAGATGTGGGAAG : 27103245

      917 : GAACTGTGGAGCACGCCAAAACATGTGTCCTTAGTGGCAAGCTACATGTGTACT :      970
            lyThrValGluHisAlaLysThrCysValLeuSerGlyLysLeuHisValTyrT
            |+|||||||||||+|||||+||+||||||||||||||||||||+||||||||||
            lyThrValGluHisAlaLysThrCysValLeuSerGlyLysLeuHisValTyrT
 27103244 : GGACTGTGGAGCATGCCAAGACTTGTGTCCTTAGTGGCAAGCTTCATGTGTACT : 27103191

      971 : ATGCTGATGATAAGCAGAATATTGGAGTTATATTCAACAACATTTTCCAGCTCA :     1024
            yrAlaAspAspLysGlnAsnIleGlyValIlePheAsnAsnIlePheGlnLeuM
            |+||+|||!!:|||||||||||+||+|||||+||||||||||||||||||||||
            yrAlaAspGluLysGlnAsnIleGlyValIlePheAsnAsnIlePheGlnLeuM
 27103190 : ACGCAGATGAGAAGCAGAATATAGGTGTTATTTTCAACAACATTTTCCAGCTCA : 27103137

     1025 : TGGGCCTTATTGCAGATGGACAATACATGTCTGTGGATTCTCTATCAGACAGTG :     1078
            etGlyLeuIleAlaAspGlyGlnTyrMetSerValAspSerLeuSerAspSerG
            ||||+||+||||||||||||||+||||||||+||+|||||+||+|||||+||||
            etGlyLeuIleAlaAspGlyGlnTyrMetSerValAspSerLeuSerAspSerG
 27103136 : TGGGACTGATTGCAGATGGACAGTACATGTCAGTTGATTCCCTGTCAGATAGTG : 27103083

     1079 : AGAAG  >>>> Target Intron 3 >>>>  GTGTATGTGGATAAACTTGT :     1102
            luLys                             ValTyrValAspLysLeuVa
            |||||            138 bp           ||+|||||||||||+||+||
            luLys++                         ++ValTyrValAspLysLeuVa
 27103082 : AGAAGgt.........................agGTTTATGTGGATAAGCTGGT : 27102921

     1103 : AAAGGTGGCGTATGAGAACTGGGAGAACGTGGTGGAGTATGATGGTGAAGCTCT :     1156
            lLysValAlaTyrGluAsnTrpGluAsnValValGluTyrAspGlyGluAlaLe
            ||||||+|||||+|||||+||||||||+||+|||||+||||||||||||||+||
            lLysValAlaTyrGluAsnTrpGluAsnValValGluTyrAspGlyGluAlaLe
 27102920 : AAAGGTTGCGTACGAGAATTGGGAGAATGTTGTGGAATATGATGGTGAAGCACT : 27102867

     1157 : CATTGGTGTGAAACCATATAAGCTCAGGGGAATTGATGCCCATACTGAGGATCC :     1210
            uIleGlyValLysProTyrLysLeuArgGlyIleAspAlaHisThrGluAspPr
            +|||||||||||+||+||+||+||++|||||||+|||||+||+||||||||+||
            uIleGlyValLysProTyrLysLeuArgGlyIleAspAlaHisThrGluAspPr
 27102866 : TATTGGTGTGAAGCCCTACAAACTTCGGGGAATCGATGCTCACACTGAGGACCC : 27102813

     1211 : AATTAATGGCCAAGGA<->CCAAGTGGGCATCAATCCCAGACAAGTGCATCGAA :     1261
            oIleAsnGlyGlnGly<->ProSerGlyHisGlnSerGlnThrSerAlaSerAs
            +||+|||||+!!.  !     !     !:!!   :!:...      ||+||+!:
            oIleAsnGlyHisProPheValValGlnAsnLeuThrSerGlyValAlaSerSe
 27102812 : TATCAATGGACATCCATTTGTAGTGCAGAATTTGACAAGTGGTGTAGCTTCAAG : 27102759

     1262 : TCAGCAAAGTGTCTCTACTACTCAAATAATGGCAATGCAGCGGCCTTCAGCTGG :     1315
            nGlnGlnSerValSerThrThrGlnIleMetAlaMetGlnArgProSerAlaGl
            !||+     !!  :!:...||+:!!   !!::!!:!!...      :!!:!:.!
            rGlnIleProAspAlaValThrLysSerIleSerLeuSerAlaMetAlaSerSe
 27102758 : TCAAATTCCTGATGCGGTCACAAAATCCATATCACTGTCAGCAATGGCATCCAG : 27102705

     1316 : T{G}  >>>> Target Intron 4 >>>>  {GT}GGTCAGGTTTACACACC :     1336
            y{G}                             {ly}GlyGlnValTyrThrPr
            !{|}            157 bp           {||} ! !!.!  ||+...||
            r{G}++                         ++{ly}ArgHisGlyTyrValPr
 27102704 : T{G}gt.........................ag{GG}AGACATGGATATGTCCC : 27102527

     1337 : ACCAGAACAGAAAGTGCACATGAAGTATCTGAACCAAGCAAATTCTTTATCTGA :     1390
            oProGluGlnLysValHisMetLysTyrLeuAsnGlnAlaAsnSerLeuSerGl
            +   |||:!:|||:!!:!::!:!..      .!.      :!:||+:!!   !!
            oLeuGluGluLysLeuTyrValSer<-><->GlnGlyHisHisSerIleMetAs
 27102526 : TTTGGAAGAAAAACTGTATGTCAGT<-><->CAGGGTCATCACTCAATAATGGA : 27102479

     1391 : GCATCCAAGTAGCCAGGCACTCCTGAGTACAGGGATAAGTTTCCCTCCCAGTCC :     1444
            uHisProSerSerGlnAlaLeuLeuSerThrGlyIleSerPheProProSerPr
            :!!.|||   ||||||::::!:   .!!   .!.! !||+.!! ! ||+!! !
            pGlnProTyrSerGlnSerMetSerGlyLeuSerThrSerIleSerProArgHi
 27102478 : TCAGCCATACAGCCAGAGTATGTCAGGTTTGAGTACAAGCATCTCACCAAGACA : 27102425

     1445 : ATCTTCTATGATCAGCATTTCACATCAGAATTTCTTGCATGGTATCAATGCCCC :     1498
            oSerSerMetIleSerIleSerHisGlnAsnPheLeuHisGlyIleAsnAlaPr
               !|||:!::!!!:!||+||+||||||||+||++|||||   !  !.!||+ !
            sValSerValValAsnIleSerHisGlnAsnPheLeuHisProSerThrAlaAl
 27102424 : TGTTTCTGTAGTCAACATATCCCATCAGAACTTTCTGCATCCAAGTACTGCAGC : 27102371

     1499 : TATGACTGGACTGGCACTTGGTCTTCCTCAAACCAACATGGGTTCATCTTCAGG :     1552
            oMetThrGlyLeuAlaLeuGlyLeuProGlnThrAsnMetGlySerSerSerGl
             |||||+||||||||+|||||+! !|||||+:!:!.!:!:  ! ! :!:|||
            aMetThrGlyLeuAlaLeuGlyProProGlnSerThrLeuIleProThrSer<-
 27102370 : CATGACGGGACTGGCCCTTGGCCCTCCTCAGTCAACCCTCATTCCTACCTCA<- : 27102317

     1553 : ACAGCCAATTTCAGTAGCCAATCCAGTTCATAGTATGCCCAGTGGTGTTACCTC :     1606
            yGlnProIleSerValAlaAsnProValHisSerMetProSerGlyValThrSe
                      :::||+:!:  !||+:!:  !.!.    ! !::!.!||+   ||
            ><-><-><->AsnValSerPheProIleThrGlyHisAlaThrAlaValGlySe
 27102316 : ><-><-><->AATGTGTCGTTTCCTATCACTGGCCATGCTACGGCTGTGGGATC : 27102275

     1607 : AGTAAGTGACTGGCCGCGATATAAAGAGCTCAGAGGACAGGAAAGTTTGTCACG :     1660
            rValSerAspTrpProArgTyrLysGluLeuArgGlyGlnGluSerLeuSerAr
            +   !::!!:|||||+||+||+||+!!::!:+|+||+||+|||   :!!||+||
            rSerThrGluTrpProArgTyrLysAspValArgGlyGlnGlu<->MetSerAr
 27102274 : CAGTACAGAGTGGCCCCGCTACAAGGATGTTCGTGGTCAAGAA<->ATGTCTCG : 27102224

     1661 : AGGCGTGCCGATGGATGAATTGTTTACAGAGGAAGAACTACGGGCAAAGAGTCT :     1714
            gGlyValProMetAspGluLeuPheThrGluGluGluLeuArgAlaLysSerLe
            |!..||+! !:!:||||||+|+|||||+|||||+||||||||+||+||+|||+|
            gAlaValGlnValAspGluLeuPheThrGluGluGluLeuArgAlaLysSerLe
 27102223 : AGCTGTTCAGGTTGATGAACTTTTTACTGAGGAGGAACTACGAGCCAAAAGTTT : 27102170

     1715 : GGAGATTCTTGAAAATGAAGACATGCATTTACAGATTCAGCAACTTCTACGCAT :     1768
            uGluIleLeuGluAsnGluAspMetHisLeuGlnIleGlnGlnLeuLeuArgMe
            +||+|||+|+||+||||||||+|||||+||||||||||||||++|+||++|+||
            uGluIleLeuGluAsnGluAspMetHisLeuGlnIleGlnGlnLeuLeuArgMe
 27102169 : AGAAATTTTGGAGAATGAAGATATGCACTTACAGATTCAGCAGTTGCTGAGAAT : 27102116

     1769 : GTTTAATGCTGCTACAGGAGAAACACCACCACCAAACCCATATCCTAATGTCCA :     1822
            tPheAsnAlaAlaThrGlyGluThrProProProAsnProTyrProAsnValGl
            |||+||+.!.|||:!!|||!!:|||       !!..! !!!:!||+|||||+||
            tPheAsnThrAlaSerGlyAspThr<-><->SerGlySerPheProAsnValGl
 27102115 : GTTCAACACAGCTTCAGGAGACACA<-><->TCAGGCTCATTTCCCAATGTTCA : 27102068

     1823 : AGGAGACGAAAGTTTTACTTTTGGAGCCTTCTCTCCTGCACCAGACTTGAATGT :     1876
            nGlyAspGluSerPheThrPheGlyAlaPheSerProAlaProAspLeuAsnVa
            +||+!!:!!:!:!!::||+!::||+:!:|||||||||||||||||||||||+||
            nGlyGluAspThrTyrThrTyrGlySerPheSerProAlaProAspLeuAsnVa
 27102067 : GGGTGAGGACACTTACACCTACGGGTCTTTCTCTCCTGCACCAGACTTGAACGT : 27102014

     1877 : TGGCATGGACAGAACTCGTGTGAATGGTAGGGCAAACGTTGGCTGGTTGAAACT :     1930
            lGlyMetAspArgThrArgValAsnGlyArgAlaAsnValGlyTrpLeuLysLe
            |||+!  ||||||.!.:::|||||+||++|+|||||+||+|||||||||||+||
            lGlyThrAspArgAlaLysValAsnGlyArgAlaAsnValGlyTrpLeuLysLe
 27102013 : TGGAACAGACAGAGCAAAAGTGAACGGGCGAGCAAATGTGGGCTGGTTGAAGCT : 27101960

     1931 : CAAGGCTGCTCTGCGATGGGGAATTTTTATTAGGAAGAGGGCAGCTGAACGAAG :     1984
            uLysAlaAlaLeuArgTrpGlyIlePheIleArgLysArgAlaAlaGluArgAr
            +||+||+||++|+||||||||+|||||+|||+|+||||||||||||! !+|+||
            uLysAlaAlaLeuArgTrpGlyIlePheIleArgLysArgAlaAlaAlaArgAr
 27101959 : TAAAGCAGCCTTACGATGGGGTATTTTCATTCGAAAGAGGGCAGCTGCAAGGAG : 27101906

     1985 : GGCCCAGCTTGAGGAGCTGGAGGATGAGTGA :     2016
            gAlaGlnLeuGluGluLeuGluAspGlu***
            +||||||||+|||||++||||+|||||+|||
            gAlaGlnLeuGluGluLeuGluAspGlu***
 27101905 : AGCCCAGCTGGAGGAATTGGAAGATGAATGA : 27101874

vulgar: Postdam_female_TRINITY_DN3906_c0_g2_i7.p1 222 2016 + HiC_scaffold_9 27117522 27101873 - 2203 C 489 489 S 1 1 5 0 2 I 0 247 3 0 2 S 2 2 C 165 165 5 0 2 I 0 13329 3 0 2 C 204 204 5 0 2 I 0 134 3 0 2 C 144 144 G 0 3 C 90 90 S 1 1 5 0 2 I 0 153 3 0 2 S 2 2 C 42 42 G 6 0 C 183 183 G 12 0 C 87 87 G 3 0 C 141 141 G 6 0 C 216 216
# --- START OF GFF DUMP ---
#
#
##gff-version 2
##source-version exonerate:coding2genome 2.2.0
##date 2024-03-19
##type DNA
#
#
# seqname source feature start end score strand frame attributes
#
HiC_scaffold_9  exonerate:coding2genome gene    27101874        27117522        2203    -       .       gene_id 1 ; sequence Postdam_female_TRINITY_DN3906_c0_g2_i7.p1 ; gene_orientation +
HiC_scaffold_9  exonerate:coding2genome cds     27117033        27117522        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27117033        27117522        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27117031        27117032        .       -       .       intron_id 1 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27116782        27117032        .       -       .       intron_id 1
HiC_scaffold_9  exonerate:coding2genome splice3 27116782        27116783        .       -       .       intron_id 0 ; splice_site "GG"
HiC_scaffold_9  exonerate:coding2genome cds     27116615        27116781        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27116615        27116781        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27116613        27116614        .       -       .       intron_id 2 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27103282        27116614        .       -       .       intron_id 2
HiC_scaffold_9  exonerate:coding2genome splice3 27103282        27103283        .       -       .       intron_id 1 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     27103078        27103281        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27103078        27103281        .       -       .       insertions 0 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27103076        27103077        .       -       .       intron_id 3 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27102940        27103077        .       -       .       intron_id 3
HiC_scaffold_9  exonerate:coding2genome splice3 27102940        27102941        .       -       .       intron_id 2 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     27102702        27102939        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27102702        27102939        .       -       .       insertions 3 ; deletions 0
HiC_scaffold_9  exonerate:coding2genome splice5 27102700        27102701        .       -       .       intron_id 4 ; splice_site "GT"
HiC_scaffold_9  exonerate:coding2genome intron  27102545        27102701        .       -       .       intron_id 4
HiC_scaffold_9  exonerate:coding2genome splice3 27102545        27102546        .       -       .       intron_id 3 ; splice_site "AG"
HiC_scaffold_9  exonerate:coding2genome cds     27101874        27102544        .       -       .
HiC_scaffold_9  exonerate:coding2genome exon    27101874        27102544        .       -       .       insertions 0 ; deletions 27
HiC_scaffold_9  exonerate:coding2genome similarity      27101874        27117522        2203    -       .       alignment_id 1 ; Query Postdam_female_TRINITY_DN3906_c0_g2_i7.p1 ; Align 27117523 223 489 ; Align 27116780 715 165 ; Align 27103282 880 204 ; Align 27102940 1084 144 ; Align 27102793 1228 90 ; Align 27102543 1321 42 ; Align 27102501 1369 183 ; Align 27102318 1564 87 ; Align 27102231 1654 141 ; Align 27102090 1801 216
# --- END OF GFF DUMP ---
#
>Postdam_female_TRINITY_DN3906_c0_g2_i7.p1
AGATCATCTCCTAAAAGAATCCAAGGTCCCGATTCTAGGAACCTGCGCCTACAGTTTCGTAACAAATTGG
CCCTTCCTTTGTTTACTGGAAGCAAGGTAGAAGGTGAGCAAGGGTCAGCCATACATGTAGTGCTACAAGA
TGCCACTGCTGGCCAAGTGGTAACAGCTGGAGCTGAGGCCTCTGCCAAGCTTGAAGTTGTGGTCTTAGAA
GGGGACTTTTCTGCAGATGATGAGGAGGACTGGCTACAGGAGGAGTTTGAGAATCATGAAGTAAGAGAAC
GAGATGGAAAGAGACCCCTTTTGACTGGGGATCTTTCAGTAACTCTTAAAGAAGGGGTTGGCACCCTTGG
GGAGTTGACATTTACAGATAACTCAAGTTGGATCCGGAGCAGAAAGTTTCGTCTGGGTGTGAAAATTTGC
TCCGGATACTGTGAAGGATTACGCATTCGCGAGGCCAAAACAGAAGCTTTCACAGTGAAGGACCATCGTG
GAAAAGTGTACAAGAAGCACTATCCTCCTGCCTTACACGATGAAGTTTGGCGGTTGGACAAAATCGGTAA
AGATGGTGCTTTTCACAAGAGATTGAACCAGTCCGGAATCCTCACAGTAGAAGACTTCCTCCGGCTGGTG
GTAATGGATCCTCAGAAGCTGCGCAATATTCTCGGGAATGGCATGTCGAACAAGATGTGGGAAGGGACTG
TGGAGCATGCCAAGACTTGTGTCCTTAGTGGCAAGCTTCATGTGTACTACGCAGATGAGAAGCAGAATAT
AGGTGTTATTTTCAACAACATTTTCCAGCTCATGGGACTGATTGCAGATGGACAGTACATGTCAGTTGAT
TCCCTGTCAGATAGTGAGAAGGTTTATGTGGATAAGCTGGTAAAGGTTGCGTACGAGAATTGGGAGAATG
TTGTGGAATATGATGGTGAAGCACTTATTGGTGTGAAGCCCTACAAACTTCGGGGAATCGATGCTCACAC
TGAGGACCCTATCAATGGACATCCATTTGTAGTGCAGAATTTGACAAGTGGTGTAGCTTCAAGTCAAATT
CCTGATGCGGTCACAAAATCCATATCACTGTCAGCAATGGCATCCAGTGGGAGACATGGATATGTCCCTT
TGGAAGAAAAACTGTATGTCAGTCAGGGTCATCACTCAATAATGGATCAGCCATACAGCCAGAGTATGTC
AGGTTTGAGTACAAGCATCTCACCAAGACATGTTTCTGTAGTCAACATATCCCATCAGAACTTTCTGCAT
CCAAGTACTGCAGCCATGACGGGACTGGCCCTTGGCCCTCCTCAGTCAACCCTCATTCCTACCTCAAATG
TGTCGTTTCCTATCACTGGCCATGCTACGGCTGTGGGATCCAGTACAGAGTGGCCCCGCTACAAGGATGT
TCGTGGTCAAGAAATGTCTCGAGCTGTTCAGGTTGATGAACTTTTTACTGAGGAGGAACTACGAGCCAAA
AGTTTAGAAATTTTGGAGAATGAAGATATGCACTTACAGATTCAGCAGTTGCTGAGAATGTTCAACACAG
CTTCAGGAGACACATCAGGCTCATTTCCCAATGTTCAGGGTGAGGACACTTACACCTACGGGTCTTTCTC
TCCTGCACCAGACTTGAACGTTGGAACAGACAGAGCAAAAGTGAACGGGCGAGCAAATGTGGGCTGGTTG
AAGCTTAAAGCAGCCTTACGATGGGGTATTTTCATTCGAAAGAGGGCAGCTGCAAGGAGAGCCCAGCTGG
AGGAATTGGAAGATGAATGA




# before running muscle alignlemtn, i ahve to decide which gene i want to use for blasia and marchanthia homologs.

so for others, we used the same as peter suggested, i changed 2 genes:
1 is U420/U90, i added the mRNA19155 back to the fasta file
2. is U 430, i deleted the one peter added, and only kept the mRNA19124, because that one loooks the same gene as the one peter added. 
3. for the U350, we do not like the mRNA13039, because that gene is outside and find another female gene on the female chromsomes as homologs, we consider that one as regular autosome genes, so we discared that one and adeed another one by blasting the female gene against the male transcriptome. 


conversation wirh peter:

Answer to the U90/420 mRNA19155 homolog. Yes, it indeed show up in the orthofinder analyses but to me it appears to be strange. 

--First, the mRNA19155 or 56 aligns very badly to the other sequences. 

--Second, the phylogenetic position of this sequence is also strange. But may be acceptable. May be it is mRNA 19155 is the right male homolog. 

This tree is for U450, not for U420?  Ah ja sorry copied the U420 below, so i will take mRNA19155 as the homolog, in this case we need to add back this sequence. 



Here is the tree for the U350, it has the homolog gene number mRNA 13039, that is located on chr6. 

Ah, i see, there is last gene in the end you added it. 
PETER: Hmm I am bit confused as when I create the tree using the mRNA 13039 it never clustered together with Sphaerocarpos but always with Pallavicinia (see tree copied below) and mRNA13039 is always on a long branch outside of the main tree. Furthermore, I also found that mRNA13039 has actually very close homologs in the female transcriptomes (these homologs are more closely related to the mRNA13039 seq than to the Blasia_c353810 female seq, which was originally present as a single homolog in the Bowman trees). Thats why I think mRNA 13039 may not be the right homolog as it appears to be (at least on my tree) highly divergent from all the other sequences but I may be wrong. What is the bootstrap support of the Sphaerocarpos-mRNA13039 branch? Here is my tree below. 

From your tree i see the helsinki male genes are clustered together with the females.  for me, This gene looks fine.  


U430 has two homologs, one is old 19124, one is newly generated by you, but i see they are almost same. Should i keep them both ,or only take the original one?  This is the last gene that i am uncertain about.  


# use the fast file to generate the tree files , first align them with muscles with default parameters.



for i in *.fasta; do muscle -align $i -output $i.afa; done

# then upload all the afa files onto the iqtree to get the tree files with bootstraps.

# then use R to visulize the trees.


# Load the libraries
library(ape)
library(ggtree)
library(viridis)
library(ggplot2)

# Function to parse node labels from the tree
parse_node_labels <- function(tree) {
  if (is.null(tree$node.label)) {
    warning("No node labels found in the tree.")
    return(tree)
  }
  
  node_labels <- tree$node.label
  bootstrap_values <- rep(NA, length(node_labels))
  
  for (i in seq_along(node_labels)) {
    if (!is.na(node_labels[i])) {
      parts <- strsplit(node_labels[i], "/")[[1]]
      if (length(parts) > 1) {
        bootstrap_values[i] <- as.numeric(parts[2])
      }
    }
  }
  
  tree$node.label <- bootstrap_values
  return(tree)
}

# Get a list of all tree files in the directory
tree_files <- list.files(pattern = "*.treefile", full.names = TRUE)

# Check if there are any tree files
if (length(tree_files) == 0) {
  stop("No tree files found in the specified directory.")
}

# Loop through each tree file
for (tree_file in tree_files) {
  # Debug print to check the current file being processed
  print(paste("Processing file:", tree_file))
  
  # Get the base name of the file (without the directory and extension)
  base_name <- tools::file_path_sans_ext(basename(tree_file))
  
  # Load the tree file
  tree <- read.tree(tree_file)
  
  # Parse node labels (bootstrap values)
  tree <- parse_node_labels(tree)
  print("Parsed node labels:")
  print(tree$node.label)
  
  # Convert tree to ggtree data frame
  ggtree_data <- fortify(tree)
  
  # Map the node labels to the appropriate nodes
  ggtree_data$bootstrap <- NA
  node_indices <- which(!is.na(ggtree_data$isTip) & !ggtree_data$isTip)
  if (length(node_indices) == length(tree$node.label)) {
    ggtree_data$bootstrap[node_indices] <- tree$node.label
  }
  
  # Shorten the branch lengths by setting them to a very small value
  tree$edge.length <- rep(0.15, nrow(tree$edge))
  
 
  # Create a ggtree object with rectangular layout
  p <- ggtree(tree, layout='rectangular', aes(color=label)) + 
    geom_tree(aes(color=label)) +  # Color the branches
    geom_tiplab(aes(color=label), size=3) +  # Color the text labels, increase text size
    theme_tree2() +
    ggtitle(paste("Phylogenetic Tree:", base_name)) +
    theme(plot.title = element_text(hjust = 0.5)) +
    xlim(0, 5)  # Adjust xlim to fit labels within the plot
  
  # Add bootstrap values to the plot if they exist and are valid
  if (!is.null(ggtree_data$bootstrap) && any(!is.na(ggtree_data$bootstrap))) {
    p <- p + geom_text2(aes(subset=!is.na(bootstrap) & bootstrap > 50, label=bootstrap), 
                        hjust=-0.3, size=3, color="black", data=ggtree_data)
  }
  
  # Customize the plot using viridis color palette
  p <- p + 
    geom_tippoint(aes(color=label), size=3, alpha=0.6) + 
    scale_color_viridis_d(option = "viridis") +
    theme(legend.position="none") +
    theme(plot.margin = unit(c(1, 5, 1, 1), "cm"))  # Add margin to the right
  
  # Increase the size of the plot to accommodate long labels
  plot_width <- 25
  plot_height <- 15
  
  # Display the plot (optional)
  print(p)
  
  # Save the plot to a file
  output_file <- file.path(paste0(base_name, "_plot.png"))
  ggsave(output_file, p, width=plot_width, height=plot_height, units="in", dpi=300)
}

# ks calculation

     git clone https://github.com/kullrich/kakscalculator2.git

     export PREFIX="."

     cd kakscalculator2;make clean;make

     ./kakscalculator2/bin/KaKs_Calculator -h

     ./bin/KaKs_Calculator -i ./examples/example.axt -o ./examples/example.axt.kaks -m YN


# ks calculator did not work really well.
# i used the wrapper software paraat to run whole analysis 


     ./ParaAT.pl --help

# remember to use the -g and -t, because this will generate the same length for the cds alignment.

     ./ParaAT.pl -h homologs.txt -a protein.fasta -n cds.fasta -p processor.txt -o output -f axt -g -t -k 

# first get out the gene list for the chrom1 from the gff file. do the same for all the chromsomes.

     grep "HiC_scaffold_9" BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff | grep "gene"| grep "mRNA"| cut -f9 | cut -f1 -d ";"| cut -f2 -d "=" > chrom9_genelist.csv


     seqtk subseq all_female_cds_seqs.fasta chrom1_genelist.txt > chrom1_female_cds_seqs.fasta
     seqtk subseq Blasia_Female_Potsdam_primary_dashremoved.fasta all_female_gene_list.csv > all_female_protein_seqs.fasta


     chrom1 gene number: 1598
     chrom2 gene number: 2454
     chrom3 gene number: 3639
     chrom4 gene number :1796
     chrom5 gene number: 2251
     chrom6 gene number: 3118
     chrom7 gene number: 1579
     chrom8 gene number: 1750
     chrom9 gene number: 546

# split the genes into different rows for the orthofinder output, make sure every gene has 1 gene in one line, not more.
# do this in python

     import pandas as pd

# Read data from a TSV file into a DataFrame
     df = pd.read_csv('Blasia_Male_Helsinki_primary_dotremoved__v__Blasia_Female_Potsdam_primary_dashremoved.tsv', delimiter='\t')

# Split the rows with multiple genes in the second and third columns

     df['Blasia_Male_Helsinki_primary_dotremoved'] = df['Blasia_Male_Helsinki_primary_dotremoved'].str.split(', ')
     df = df.explode('Blasia_Male_Helsinki_primary_dotremoved')

     df['Blasia_Female_Potsdam_primary_dashremoved'] = df['Blasia_Female_Potsdam_primary_dashremoved'].str.split(', ')
     df = df.explode('Blasia_Female_Potsdam_primary_dashremoved')

# Reset index

     df.reset_index(drop=True, inplace=True)

# Write the modified DataFrame to a new TSV file

     df.to_csv('modified_file.tsv', sep='\t', index=False)

     print("DataFrame has been written to modified_file.tsv")

     ~                                        

# then get the orthofinder second column and third column, which are the helsinki male and potsdam female gene list 

     helsinki: 14692
     potsdam 14692

# then extract these genes out from the protein and cds file from these two strains

# in total male helsinki has 29382 proteins and 29382 cds.
# potsdam female has 29382 proteins and 28926 cds 
# sth is wrong about the cds file for the female. 
# the cds file seems less genes than the protein file. i am trying to fix it with the bash scripts:
# get_longest_orf.sh to get the longest orf in the cds file too and see how much i get.

# then extract the genes out from the 29382 proteins using the chrom1 gene list.

     chrom1 gene number: 1598
     chrom2 gene number: 2454
     chrom3 gene number: 3639
     chrom4 gene number :1796
     chrom5 gene number: 2251
     chrom6 gene number: 3118
     chrom7 gene number: 1579
     chrom8 gene number: 1750
     chrom9 gene number: 546


     grep "HiC_scaffold_1" BlasiaPusilla_Final_annotations_validORF_sexGenesAppended_renamed_9scaffolds.gff | grep "gene"| grep "mRNA"| cut -f9 | cut -f1 -d ";"| cut -f2 -d "=" > chrom1_genelist.txt
     grep ">" chrom1_genelist.txt | wc -l
     1598 

     seqtk subseq Blasia_Male_Helsinki_primary_dotremoved.fasta all_male_gene_list.csv > all_male_protein_seqs.fasta 

     seqtk subseq all_male_protein_seqs.fasta chrom1_genelist.txt > chrom1_orthofinder_protein.fasta

     grep ">" chrom1_orthofinder_protein.fasta | wc -l
     1212 

## for the chrom1 ,gff file has 1598 genes, and orthofinder has 1212 proteins and cds that has the orthologes with the female.
    
     chrom2: gff file has 2454 gens, and orthogroups: 1912
     chrom3: gff file has 3639 genes, and orthogrousp: 2819
     chrom4 : gff file has 1796 genes, and orthogroups: 1337
     chrom5 : gff file has 2251 genes, and orthogroups: 1696
     chrom6: gff file has 3118 gene,and orthogroups: 2298
     chrom7: gff file has 1579 genes and orthogroups: 1247
     chrom8 gff file has 1750 genes, and orthogroups: 1343
     chrom9 gff file has 546 gens, and orthogroups: 535 

# collect all the genes and put them on the excel and mark them with the chrom infos.  

# after this, i will have to collect all the gens list from the female strain from the orthogroups csv file by left join the orthofinder file onto the male gene ids.


## then extract the protein and cds sequences from the 29382 protein and cds files.


     awk '/^>/ {sub(/ .*/, ""); print} !/^>/ {print}' concatenated_cds.fasta > final_cds.fasta

     ../ParaAT2.0/ParaAT.pl -h homologs.txt -a final_protein.fasta -n final_cds.fasta -p processor.txt -o paraat_output -f axt -g -t -k


# now i want to caculate the sliding average on the ks value, everh single point, take the 0.5Mbp before the point and after the point, caculate the average. 



# extract the hic scaffold 9 data and make the plot as follows: 

# Update the plot to remove the "Ks Value" label from the color bar
# in python

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.colors import ListedColormap

# Load the data
file_path = /Desktop/blasia_genome/ks_caculation/chrom9.csv  # Update this to your actual file path
data = pd.read_csv(file_path)

# Calculate the average position of the gene in base pairs and convert it to Mbp
data['average_position'] = (data['start'] + data['end']) / 2
data['average_position_mbp'] = data['average_position'] / 1_000_000

# Sort data by average position in Mbp
sorted_data = data.sort_values('average_position_mbp')

# Calculate the sliding average of the Ks values with a window of 1 Mbp
window_size = max(1, int(len(sorted_data) * (1_000_000 / (sorted_data['average_position'].max() - sorted_data['average_position'].min()))))
rolling_average_mbp = sorted_data['Ks'].rolling(window=window_size, min_periods=1, center=True).mean()

# Enhance the Viridis colormap by increasing color intensity
viridis_colors = cm.viridis(np.linspace(0, 1, 256))
enhanced_colors = []
for rgb in viridis_colors:
    hsv = colorsys.rgb_to_hsv(*rgb[:3])
    new_rgb = colorsys.hsv_to_rgb(hsv[0], min(1.0, hsv[1]*1.5), min(1.0, hsv[2]*1.2))
    enhanced_colors.append(new_rgb)
enhanced_viridis = ListedColormap(enhanced_colors)

# Create the plot
plt.figure(figsize=(10, 6))
scatter = plt.scatter(sorted_data['average_position_mbp'], sorted_data['Ks'], c=sorted_data['Ks'], cmap=enhanced_viridis, alpha=0.5, label='Individual Gene')
plt.plot(sorted_data['average_position_mbp'], rolling_average_mbp, color='red', label='1 Mbp Sliding Average')
plt.colorbar(scatter)  # Removed label for clarity
plt.title('Gene Locations and Ks Value with Sliding Average for Chromosome 9', pad=20)
plt.xlabel('Genomic Coordinate (Mbp)')
plt.ylabel('Ks Value')
plt.xlim(0, 30)
plt.legend()
plt.grid(True)

# Configure ticks
major_ticks = np.arange(0, 31, 5)
minor_ticks = np.arange(0, 31, 1)
plt.xticks(major_ticks, labels=[f'{tick:.0f}' for tick in major_ticks])
ax = plt.gca()
ax.set_xticks(minor_ticks, minor=True)
ax.set_xticklabels([''] * len(minor_ticks), minor=True)

plt.show()


# now need to change the y axis to make them 0-6 and also change the color plate to :

circosCols <- c("#F4A9A8", "#FFFFFF", "#91BFDB", "#4575B4","#BDBDBD","#E0E0E0")  # example hex codes

# this color will fit the genome papar color scheme .

# now generate all the other 8 chromsoems together with the chrom9 into 1 panel
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.colors import ListedColormap
import colorsys

# Enhance the Viridis colormap by increasing color intensity
viridis_colors = cm.viridis(np.linspace(0, 1, 256))
enhanced_colors = []
for rgb in viridis_colors:
    hsv = colorsys.rgb_to_hsv(*rgb[:3])
    new_rgb = colorsys.hsv_to_rgb(hsv[0], min(1.0, hsv[1]*1.5), min(1.0, hsv[2]*1.2))
    enhanced_colors.append(new_rgb)
enhanced_viridis = ListedColormap(enhanced_colors)

# File paths and chromosome labels
chrom_files_updated_ordered = [
    ('/mnt/data/chrom3_reordered.csv', 1),
    ('/mnt/data/chrom6_reordered.csv', 2),
    ('/mnt/data/chrom2_reordered.csv', 3),
    ('/mnt/data/chrom5_reordered.csv', 4),
    ('/mnt/data/chrom4_reordered.csv', 5),
    ('/mnt/data/chrom1_reordered.csv', 6),
    ('/mnt/data/chrom8_reordered.csv', 7),
    ('/mnt/data/chrom7_reordered.csv', 8),
    ('/mnt/data/chr9_reordered.csv', 9)
]

# Create a panel of plots with dynamic x-axis limits for each chromosome and with updated x-axis label
fig, axs = plt.subplots(3, 3, figsize=(18, 18))
fig.suptitle('Gene Locations and Ks Value with Sliding Average for All Chromosomes', fontsize=24)

for (file_path, chrom_label), ax in zip(chrom_files_updated_ordered, axs.ravel()):
    data = pd.read_csv(file_path)
    data['average_position'] = (data['start'] + data['end']) / 2
    data['average_position_mbp'] = data['average_position'] / 1_000_000
    sorted_data = data.sort_values('average_position_mbp')
    window_size = max(1, int(len(sorted_data) * (1_000_000 / (sorted_data['average_position'].max() - sorted_data['average_position'].min()))))
    rolling_average_mbp = sorted_data['Ks'].rolling(window=window_size, min_periods=1, center=True).mean()
    
    scatter = ax.scatter(sorted_data['average_position_mbp'], sorted_data['Ks'], c=sorted_data['Ks'], cmap=enhanced_viridis, alpha=0.5, label='Individual Gene')
    ax.plot(sorted_data['average_position_mbp'], rolling_average_mbp, color='red', label='1 Mbp Sliding Average')
    ax.set_title(f'Chromosome {chrom_label}', pad=10, fontsize=18)
    ax.set_xlim(0, sorted_data['average_position_mbp'].max())  # Adjust x-axis limit dynamically
    ax.grid(True)
    
    # Configure ticks
    major_ticks = np.arange(0, sorted_data['average_position_mbp'].max() + 1, 5)
    minor_ticks = np.arange(0, sorted_data['average_position_mbp'].max() + 1, 1)
    ax.set_xticks(major_ticks)
    ax.set_xticklabels([f'{tick:.0f}' for tick in major_ticks], fontsize=16)
    ax.set_xticks(minor_ticks, minor=True)
    ax.set_xticklabels([''] * len(minor_ticks), minor=True)
    ax.tick_params(axis='y', labelsize=16)
    ax.tick_params(axis='x', labelsize=16)

# Add a legend to the first subplot
handles, labels = scatter.legend_elements(prop="colors", alpha=0.6)
legend_labels = ["Individual Gene Ks Value", "1 Mbp Sliding Average"]
axs[0, 0].legend([handles[0], plt.Line2D([0], [0], color='red', lw=2)], legend_labels, loc='upper left', frameon=True, fontsize=16)

# Add a common x-axis label
fig.text(0.5, 0.04, 'Genomic Coordinates (Mbp)', ha='center', fontsize=18)

fig.tight_layout(rect=[0, 0.05, 1, 0.95])
fig.colorbar(scatter, ax=axs, orientation='vertical', fraction=0.02, pad=0.02)
plt.show()


# change color plate version :

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap

# Load chromosome data
chrom_files_updated_ordered = [
    ('/mnt/data/chrom3_reordered.csv', 1),
    ('/mnt/data/chrom6_reordered.csv', 2),
    ('/mnt/data/chrom2_reordered.csv', 3),
    ('/mnt/data/chrom5_reordered.csv', 4),
    ('/mnt/data/chrom4_reordered.csv', 5),
    ('/mnt/data/chrom1_reordered.csv', 6),
    ('/mnt/data/chrom8_reordered.csv', 7),
    ('/mnt/data/chrom7_reordered.csv', 8),
    ('/mnt/data/chr9_reordered.csv', 9)

# Load them into a dictionary of dataframes
chrom_data = {}
for chrom, path in chrom_files_updated_ordered.items():
    df_chrom = pd.read_csv(path)
    chrom_data[chrom] = df_chrom

# Define the extended circos color scheme (valid hex codes)
circosCols_extended = ["#E41A1C", "#F46D43", "#F4A9A8", "#FFA500", "#4575B4", "#91BFDB", "#B0B0B0"]  # 'gray70' as '#B0B0B0'
custom_cmap_normal = ListedColormap(circosCols_extended)  # Non-reversed

# Create a 3x3 panel plot
fig, axs = plt.subplots(3, 3, figsize=(18, 18))
fig.suptitle('Gene Locations and Ks Value with Sliding Average for All Chromosomes', fontsize=24)

for i, (chrom, ax) in enumerate(zip(chrom_data.keys(), axs.ravel())):
    data = chrom_data[chrom]
    data['average_position'] = (data['start'] + data['end']) / 2
    data['average_position_mbp'] = data['average_position'] / 1_000_000
    sorted_data = data.sort_values('average_position_mbp')
    
    # 1 Mbp sliding average
    window_size = max(1, int(len(sorted_data) * (1_000_000 / (sorted_data['average_position'].max() - sorted_data['average_position'].min()))))
    rolling_avg = sorted_data['Ks'].rolling(window=window_size, min_periods=1, center=True).mean()
    
    # Plot scatter and sliding average
    scatter = ax.scatter(sorted_data['average_position_mbp'], sorted_data['Ks'], c=sorted_data['Ks'],
                         cmap=custom_cmap_normal, alpha=0.7, s=20, label='Individual Gene')  # Increased dot size
    ax.plot(sorted_data['average_position_mbp'], rolling_avg, color='red', lw=2, label='1 Mbp Sliding Average')
    
    ax.set_title(f'Chromosome {chrom[-1]}', pad=10, fontsize=18)
    ax.set_xlim(0, sorted_data['average_position_mbp'].max())
    ax.set_ylim(0, 6)
    ax.grid(True)
    ax.set_facecolor('white')
    
    major_ticks = np.arange(0, sorted_data['average_position_mbp'].max() + 1, 5)
    minor_ticks = np.arange(0, sorted_data['average_position_mbp'].max() + 1, 1)
    ax.set_xticks(major_ticks)
    ax.set_xticklabels([f'{tick:.0f}' for tick in major_ticks], fontsize=16)
    ax.set_xticks(minor_ticks, minor=True)
    ax.set_xticklabels([''] * len(minor_ticks), minor=True)
    ax.tick_params(axis='y', labelsize=16)
    ax.tick_params(axis='x', labelsize=16)

# Add legend to the first subplot
axs[0, 0].legend(loc='upper left', frameon=True, fontsize=16)

# Colorbar on the right side
cbar_ax = fig.add_axes([0.92, 0.15, 0.02, 0.7])
cbar = fig.colorbar(scatter, cax=cbar_ax, orientation='vertical', label='Ks Value')
cbar.ax.tick_params(labelsize=16)

# Common x-axis label
fig.text(0.5, 0.04, 'Genomic Coordinates (Mbp)', ha='center', fontsize=18)

fig.tight_layout(rect=[0, 0.05, 0.9, 0.95])  # Leave space for colorbar on the right

# Save as SVG
output_path_final_normal_cmap_svg = "/mnt/data/Ks_9chromosomes_panel_white_bg_colorbar_normal_biggerdots.svg"
plt.savefig(output_path_final_normal_cmap_svg, format='svg')
plt.close()

print("Final SVG file saved at:", output_path_final_normal_cmap_svg)

## We are going to run two orthofinder analyses;

## -- a) One using only proteomes of sequenced sequenced genomes
## -- b) One using a lot of liverwort transcriptomic datasets

## For genome analyses one should use the following proteomoes from the following genomes
     
     \https://gitlab.com/peterszoevenyi/blasia_genome/-/blob/main/Orthofinder_analysis/1_Orthofinder_proteomes_list_species.xlsx

## PLUS some extra genomes of mosses and liverworts must be added:

## Pohlia nutans from this publication (https://www.frontiersin.org/articles/10.3389/fpls.2022.920138/full)

## Lunularia crutiata genomes from COGE (from this publication two proteomes https://academic.oup.com/gbe/article/15/3/evad014/7023155)
## Genome assemblies and annotations for L. cruciata were produced based on published data and are available at genomevolution.org (IDs 64629, 64630, and 64631).

## I think that it is. IMPORTANT ALL PROTEOMES MUST BE FILTERED FOR THE FIRST SPLICE VARIANT!!!!

## Let me know if you miss some of the excel table proteomes
## I have put almost all new proteomes into Stephanies folder on the stoarge. You can copy them over to your folder:

      W:\P_Szoevenyi\PROJECT\Stephanie\Manuscripts\First_paper\Orthofinder_analysis\Species



## The second anal should include the Blasia proteome plus all the liverwort transcritptomes and genomes listed in the 
## paper https://pubmed.ncbi.nlm.nih.gov/34735792/
## The transcriptomes can be downloaded (proteomes) from here https://www.onekp.com/public_data.html

## two files that need to be chose for the primary transcripts:
## Chara_braunii_iso_noTE_23456_pep.fasta  and blasia 

## for the species lunularia: extract the pep file with gffread and choose the primary transcript.


     sed -i 's/lcl|//g' Lunularia_cruciata_male.faa
     sed -i 's/lcl|//g' Lunularia_cruciata_female.faa


     mv Lunularia_cruciata_male.faa Lunularia_cruciata_male.fasta
     mv Lunularia_cruciata_female.faa Lunularia_cruciata_female.fasta


     gffread -w Lunularia_male_transcripts.fasta -x lunularia_male_cds.fasta -y lunularia_male_protein.fasta -g Lunularia_cruciata_male.fasta Lunularia_cruciata_male.gff


     gffread -w Lunularia_female_transcripts.fasta -x lunularia_female_cds.fasta -y lunularia_female_protein.fasta -g Lunularia_cruciata_female.fasta Lunularia_cruciata_female.gff

# choose the mRNA1 for lunularia female: and do the smme for the male.

     grep '\.mRNA1' lunularia_female_protein.fasta > output_file_headers.fasta

     awk 'FNR==NR {header[$0]; next} /^>/ {flag = ($0 in header)} flag' output_file_headers.fasta lunularia_female_protein.fasta  > output_file_sequences.fasta



# choose the t1 for chara:

     grep "^>.*\.2" your_file.gff

## keeop the header with *.t1 and drop the *.t2


     grep '\.t1' Chara_braunii_iso_noTE_23456_pep.fasta > output_file_headers.fasta

     awk 'FNR==NR {header[$0]; next} /^>/ {flag = ($0 in header)} flag' output_file_headers.fasta Chara_braunii_iso_noTE_23456_pep.fasta  > output_file_sequences.fasta

# doing the same for blasia with python.


def read_fasta(file_path):
    sequences = {}
    with open(file_path, 'r') as file:
        header = None
        seq = ''
        for line in file:
            if line.startswith('>'):
                if header is not None:
                    sequences[header] = seq
                header = line.strip()
                seq = ''
            else:
                seq += line.strip()
        if header is not None:
            sequences[header] = seq
    return sequences

def find_longest_mRNA(sequences):
    longest_mRNA = {}
    for header, seq in sequences.items():
        gene_id = header.split()[1]
        gene_id = gene_id[5:]  # Remove ">mRNA" prefix
        if gene_id not in longest_mRNA:
            longest_mRNA[gene_id] = (header, seq)
        else:
            _, longest_seq = longest_mRNA[gene_id]
            if len(seq) > len(longest_seq):
                longest_mRNA[gene_id] = (header, seq)
    return longest_mRNA

def write_fasta(output_path, longest_mRNA):
    with open(output_path, 'w') as file:
        for header, seq in longest_mRNA.values():
            file.write(header + '\n')
            file.write(seq + '\n')

if __name__ == "__main__":
    input_file = "feywei_sex_genes_appended_protein.fasta"
    output_file = "longest_mRNAs_protein.fasta"

    sequences = read_fasta(input_file)
    longest_mRNA = find_longest_mRNA(sequences)
    write_fasta(output_file, longest_mRNA)



## eventually it shows only 17382 mRNA, this is the same numbers as the gene numbers, so that mean each gene has only one mRNA.  , not 19529 mRNAs for the all scaffolds. for the 9 scaffolds. in total it is only 19160 mRNAs. 17046 genes. 
## also the script chose the longest one. 


## now i should run orthofinder for all the proteome files.

     conda activate base

     python3 orthofinder.py -f OrthoFinder/ExampleData

# works

# there is extra dash in the files, i have to remove them.

     sed 's/-//g' Atrichopoda_291_v1.0.protein_primaryTranscriptOnly.fa > Atrichopoda_291_v1.0.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Bdistachyon_556_v3.2.protein_primaryTranscriptOnly.fa > Bdistachyon_556_v3.2.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Boleraceacapitata_446_v1.0.protein_primaryTranscriptOnly.fa > Boleraceacapitata_446_v1.0.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Chara_braunii_primaryTranscript_noTE_23456_pep.fasta > Chara_braunii_primaryTranscript_noTE_23456_pep_dashremoved.fasta 

     sed 's/-//g' Cpapaya_113_ASGPBv0.4.protein_primaryTranscriptOnly.fa >Cpapaya_113_ASGPBv0.4.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' CpurpureusGG1_539_v1.1.protein_primaryTranscriptOnly.fa > CpurpureusGG1_539_v1.1.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' CpurpureusR40_538_v1.1.protein_primaryTranscriptOnly.fa > CpurpureusR40_538_v1.1.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g'  CreinhardtiiCC_4532_707_v6.1.protein_primaryTranscriptOnly.fa > CreinhardtiiCC_4532_707_v6.1.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Dcarota_388_v2.0.protein_primaryTranscriptOnly.fa > Dcarota_388_v2.0.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Fontinalis_antipyretica.pep.fa > Fontinalis_antipyretica.pep_dashremoved.fa

     sed 's/-//g'  Gmax_275_Wm82.a2.v1.protein_primaryTranscriptOnly.fa > Gmax_275_Wm82.a2.v1.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g'  Mguttatus_256_v2.0.protein_primaryTranscriptOnly.fa > Mguttatus_256_v2.0.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' MpusillaRCC299_229_v3.0.protein_primaryTranscriptOnly.fa > MpusillaRCC299_229_v3.0.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Mtruncatula_285_Mt4.0v1.protein_primaryTranscriptOnly.fa > Mtruncatula_285_Mt4.0v1.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g'  PdeltoidesWV94_445_v2.1.protein_primaryTranscriptOnly.fa > PdeltoidesWV94_445_v2.1.protein_primaryTranscriptOnly_dashremoved.fa


# change dash into underscore:

     sed 's/-/_/g'  Pleurozium_Schreberii_protein.fa_headermod_firstsplice.fa > Pleurozium_Schreberii_protein.fa_headermod_firstsplice_dashremoved.fa

     sed 's/-/_/g'  Pohlia_nutans_Protein.faa > Pohlia_nutans_Protein_dashremoved.faa


     sed 's/-//g' Ppatens_318_v3.3.protein_primaryTranscriptOnly.fa > Ppatens_318_v3.3.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g'  Ppersica_298_v2.1.protein_primaryTranscriptOnly.fa > Ppersica_298_v2.1.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Ptrichocarpa_533_v4.1.protein_primaryTranscriptOnly.fa > Ptrichocarpa_533_v4.1.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Sbicolor_730_v5.1.protein_primaryTranscriptOnly.fa > Sbicolor_730_v5.1.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Sfallax_522_v1.1.protein_primaryTranscriptOnly.fa > Sfallax_522_v1.1.protein_primaryTranscriptOnly_dashremoved.fa 

     sed 's/-//g' Slycopersicum_796_ITAG5.0.protein_primaryTranscriptOnly.fa > Slycopersicum_796_ITAG5.0.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Taestivum_296_v2.2.protein_primaryTranscriptOnly.fa > Taestivum_296_v2.2.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Tcacao_523_v2.1.protein_primaryTranscriptOnly.fa > Tcacao_523_v2.1.protein_primaryTranscriptOnly_dashremoved.fa

     sed 's/-//g' Vcarteri_317_v2.1.protein_primaryTranscriptOnly.fa > Vcarteri_317_v2.1.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Vvinifera_457_v2.1.protein_primaryTranscriptOnly.fa > Vvinifera_457_v2.1.protein_primaryTranscriptOnly_dashremoved.fa


     sed 's/-//g' Zmays_284_Ensembl-18_2010-01-MaizeSequence.protein_primaryTranscriptOnly.fa >Zmays_284_Ensembl-18_2010-01-MaizeSequence.protein_primaryTranscriptOnly_dashremoved.fa 


     sed 's/-//g' medicago_sativa_protein.fa > medicago_sativa_protein_dashremoved.fa

     sed 's/\./\*/g' Blasia_Final_protein_primarytranscript.fasta  > Blasia_Final_protein_primarytranscript_dotremoved.fasta 


     sed 's/\./\*/g' lunularia_female_protein_primarytranscript.fasta > lunularia_female_protein_primarytranscript_dotremoved.fasta


     sed 's/\./\*/g' lunularia_male_protein_primarytranscript.fasta > lunularia_male_protein_primarytranscript_dotremoved.fasta

# run orthofinder analysis: 

     conda activate base

     python3 orthofinder.py -f /home/ubuntu/Blasia_Genome/orthofinder_analysis_blasiaGenome/species/ -p /home/ubuntu/Blasia_genome/orthofinder_analysis/species/tmp_files




## now i should run orthofinder for all the proteome files.

## i got the feywei' s punctatus data again and re-run the orthofinder and the salmon.

## i got the gff file and the genome file, i wanted to add the uTR on, but there is no bam file available.

     gt gff3 -sort -tidy -checkids -addids -fixregionboundaries Anthoceros_punctatus_v2_gene_annotations.gff > Anthoceros_punctatus_v2_gene_annotations_fixed.gff 

     gt loccheck Anthoceros_punctatus_v2_gene_annotations_fixed.gff 

     gffread -w transcripts.fasta -x cds.fasta -y protein.fasta -g Anthoceros_punctatus_v2_genome.fna Anthoceros_punctatus_v2_gene_annotations.gff 

# keep only the .t1 from the protein file

     grep '\.t1' anpunctatus_feywei_protein.fasta > output_file_headers.fasta

     awk 'FNR==NR {header[$0]; next} /^>/ {flag = ($0 in header)} flag' output_file_headers.fasta  anpunctatus_feywei_protein.fasta > output_file_sequences.fasta

# now re-run the orthofinder with all the other species:


     conda activate base

     python3 orthofinder.py -f /home/ubuntu/Blasia_Genome/orthofinder_analysis_blasiaGenome/species/ -p /home/ubuntu/Blasia_Genome/orthofinder_analysis_blasiaGenome/species/tmp_files/


## before using r pakcage to visulize the tree files, convert the txt format into newick.
## this is the program that can do the job:



from Bio import Phylo

def convert_txt_to_newick(input_file, output_file):
    with open(input_file, 'r') as infile:
        tree_str = infile.read().strip()  # Read the tree string from the text file

    tree = Phylo.read(StringIO(tree_str), 'newick')  # Parse the tree from the string
    Phylo.write(tree, output_file, 'newick')  # Write the tree to a newick file

if __name__ == "__main__":
    # Provide the directory containing the text files and the output directory
    input_directory = "/home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/Gene_Trees"
    output_directory = "/home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/Gene_Trees"

    import os
    from io import StringIO

    # Loop through each file in the input directory
    for filename in os.listdir(input_directory):
        if filename.endswith(".txt"):
            input_file = os.path.join(input_directory, filename)
            output_file = os.path.join(output_directory, filename.replace(".txt", ".newick"))

            convert_txt_to_newick(input_file, output_file)


#!/bin/bash

# Define the directory containing the Newick files
directory_path="/home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/Gene_Trees/"

# Loop through each gene ID in the gene list file
while read -r gene_id; do

    # Use grep to search for the gene ID in all the Newick files

    grep -l "$gene_id" "$directory_path"/*.txt >> files_with_gene_ids.txt

done < /home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/UV_sex_chromsome_genes.txt
~                

# before the visualization, i want to select the list of U chromosome and V chromsome genes from the gene tress.


#!/bin/bash

# Define the directory containing the Newick files
input_directory="/home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/Gene_Trees/"

# Define the directory where you want to move the files containing gene IDs
output_directory="/home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/Gene_Trees/output_directory/"

# Create the output directory if it doesn't exist
mkdir -p "$output_directory"

# Read the gene IDs from the file and search for them in the Newick files
while read -r gene_id; do
    # Use find to search for the gene ID in all the Newick files
    files=$(find "$input_directory" -type f -name "*.newick" -exec grep -l "$gene_id" {} +)
    if [ -n "$files" ]; then
        echo "$files" | xargs -I{} mv "{}" "$output_directory"
    fi
done < /home/ubuntu/USERS/Yuling/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/OrthoFinder/Results_Feb26_5/Gene_Trees/UV_sex_chromsome_genes.txt


# now that i have all the newwick format sex chromosome genes i want to use R package ape to visualize the tree files:

# Install and load the 'ape' package if not already done
if (!requireNamespace("ape", quietly = TRUE)) {
  install.packages("ape")
  
}
library(ape)

# Directory containing your Newick files

     newick_dir <- "/Users/jolene/Desktop/blasia_genome/Blasia_paper_orthofinder_analysis/liverworts_species_3_gene_tree"

# List all files in the directory with a .txt extension (adjust as needed)

     newick_files <- list.files(newick_dir, pattern = "\\.newick$", full.names = TRUE)


# Loop through each Newick file, read the tree, and plot it
for (file in newick_files) {
  cat("Processing file:", file, "\n")
  
  # Read the Newick tree from the file
  tree_string <- readLines(file, warn = FALSE)
  tree <- read.tree(text = tree_string)
  
  # Plot the phylogenetic tree
  plot(tree, cex = 0.7, edge.width = 2)
  
  # Pause for a moment before proceeding to the next tree
  Sys.sleep(2)  # Adjust the sleep duration as needed
}


# get the new sex genes added annotation . run orthofinder again:
# this version includes the both helsinki and feywei's annotation. and the augustus CPG mode sex linked genss


     python3 orthofinder.py -f /home/ubuntu/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/ -p /home/ubuntu/Blasia_Genome/orthofinder_analysis_blasiaGenome/liverworts_species/tmp_files/


# now i want to add the blasia two genome sex linked genes onto the lunularia paper alignment files.
## download the alignkent file from the paper and convert the alignment file into fasta file:

     cat alignment.txt | fasconvert -i nexus > data.fas

     for i in *.nexus;

     do
      awk 'NR >5' ~/Blasia_Genome/orthofinder_analysis_blasiaGenome/$i | tr -d "'" | tr " " "\n" | sed 's/locus/>locus/g' > ~/Blasia_Genome/orthofinder_analysis_blasiaGenome/${i}.fasta
     done




