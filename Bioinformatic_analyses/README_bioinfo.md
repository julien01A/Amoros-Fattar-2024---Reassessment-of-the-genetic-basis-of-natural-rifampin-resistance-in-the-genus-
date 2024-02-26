# Genomic analyses 

## 1 - Construction of the rpoB complete gene sequence of *R. lusitaniae* R-Om
(MARIE)

## 2 - Annotation of the Rickettsia genomes
We collected 78 genomes of Rickettsia, Orientia and Cd. Megaira from public databases (fasta format). A part of these genomes were already annotated, but we wanted to have a standardized annoation for all the 78 genomes. Thus, we (re)annoted the 78 genomes using Prokka v1.14.6 (https://github.com/tseemann/prokka, Seemann T. (2014), Prokka: rapid prokaryotic genome annotation. Bioinformatics. doi: 10.1093/bioinformatics/btu153) using the following command :

'prokka $sample_name-genome.fasta --locustag $sample_name --prefix $sample_name --outdir Prokka-$sample_name --rfam --compliant --cpus 6'

with "sample_name" the name of the genome to annotate.

## Titre analyse 3 - phylogenomy

## Titre analyse 4 - analyses orthofinder etc



In this study, we proceed statistical analysis of qPCR to determine the Rickettsia lusitaniae R-Om density in response to rifampin treatment. We then carry diverse genomic analysis in order to (i) construct the rpoB complete gene sequence of R. lusitaniae R-Om based on a previous metagenomic investigation (Duron et al. 2018), (ii) annotate 78 Rickettsia complete genomes from public datasets, (iii) perform phylogenomic analyses, and (iv) identify orthogroups specific to naturally resistant-rifampin Rickettsia species.

This GitHub page shares the command lines used for the statistical and genomic analysis, each in a dedicated file.

These GitHub page shares exclusively analysis carried out with command lines and/or script. Additionnal assays which have been realised in specific software without command line are not presented here (eg. the Unipro UGENE software for the rpoB alignment).
