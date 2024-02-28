# Genomic analyses 

## 1 - Construction of the *rpoB* complete gene sequence of *R. lusitaniae* R-Om
(MARIE)


## 2 - Annotation of the *Rickettsia* genomes
We collected a set of **78 genomes of *Rickettsia*, *Orientia* and *Cd. Megaira*** from public databases (in fasta format, see Table S2 for the accession numbers used). A part of these genomes were already annotated, but we proceed a standardized annotation for all the 78 genomes using Prokka v1.14.6 (<https://github.com/tseemann/prokka>, Seemann T. (2014), Prokka: rapid prokaryotic genome annotation. Bioinformatics. doi: 10.1093/bioinformatics/btu153):

```
prokka $sample_name-genome.fasta --locustag $sample_name --prefix $sample_name --outdir Prokka-$sample_name --rfam --compliant --cpus 6
```

with ```"sample_name"``` the name of the genome to annotate.


## 3 - Phylogenomic analyses

First, single-copy orthologs (SCO) were identified using OrthoFinder v2.3.11 (<https://github.com/davidemms/OrthoFinder>, Emms D.M. and Kelly S. (2019), OrthoFinder: phylogenetic orthology inference for comparative genomics. Genome Biology. doi: 10.1186/s13059-019-1832-y) from the 78 annoted genomes based on their ```.ffa``` files :

```
orthofinder -f ./OrthoFinder_genomes/ -t 4 -S blast
```

For each SCO, sequences were individually aligned using MAFFT v7.450 (<https://github.com/GSLBiotech/mafft>, Katoh K. and Standley D.M. (213), MAFFT multiple sequence alignment software version 7: improvements in performance and usability. Molecular Biology and Evolution. doi: 10.1093/molbev/mst010):

```
for file in /Single_Copy_Orthologue_Sequences/*
do mafft "$file" > "$file"
done
```

For each SCO, ambigious hypervariable regions were removed using trimAl v1.2rev59 (<https://github.com/inab/trimal>, Capella-Gutiérrez S., Silla-Martínez J.M., and Gabaldón T. (2009), trimAl: a tool for automated alignment trimming in large-scale phylogenetic analyses. Bioinformatics. doi: 10.1093/bioinformatics/btp348):

```
cp ./Single_Copy_Orthologue_Sequences/*_align.fasta ./Single_Copy_Orthologue_Sequences_trimal/
for file in /Single_Copy_Orthologue_Sequences_trimal/*
do trimal -in "$file" -out "$file" -fasta -gt 1 -cons 50
done
```

Then, all SCO sequences were concatenated using Amas v1.0 (<https://github.com/marekborowiec/AMAS>, Borowiec M.L. (2016), AMAS: a fast tool for alignment manipulation and computing of summary statistics. PeerJ. doi: 10.7717/peerj.1660) in a single file:

```
for file in /Single_Copy_Orthologue_Sequences_trimal/*
do awk '/^>/{print ">organism" ++i; next}{print}' < "$file" > "${file%_align.fasta}_rename.fasta"
done
cp ./Single_Copy_Orthologue_Sequences_trimal/*_rename.fasta ./Single_Copy_Orthologue_Sequences_AMAS/
AMAS.py concat -f fasta -d aa --in-files ./Single_Copy_Orthologue_Sequences_AMAS/*.fasta
```

Substitution models were evaluated using modeltest v0.1.5 (<https://github.com/ddarriba/modeltest>, Darriba D., Posada D., Kozlov A.M., Stamatakis A., Morel B., and Flouri T. (2020), ModelTest-NG: A new and scalable tool for the selection of DNA and protein evolutionary models. Molecular Biology and Evolution. doi: 10.1093/molbev/msz189) in order to determinate the appropriate substitution model (according to AICc criterion) to use for the phylogenetic tree construction with RAxML v8.2.9 (<https://github.com/stamatak/standard-RAxML>, Stamatakis A. (2014) RAxML version 8: a tool for phylogenetic analysis and post-analysis of large phylogenies. Bioinformatics. doi: 10.1093/bioinformatics/btu033):

```
modeltest-ng -i SCO_concatenated.faa -p 12 -T raxml -d aa
raxmlHPC-PTHREADS -T 8 -f a -s SCO_concatenated.faa -n phylo -m PROTGAMMAIJTT -x 1234 -# 1000 -p 1234
```

Finally, the phylogenetic tree was visualized and modified using figtree (<https://github.com/rambaut/figtree/>).


## 4 - Identification of orthogroups specific to naturally resistant-rifampin Rickettsia species

### 4.1. Orthofinder

For this Orthofinder analysis, the command line are the same than the part above. However, we used a restricted part of the previous dataset, corresponding to **18 *Rickettsia* genomes** (n=3 resistant and n=15 suceptible, see TableS2 for details). 


### 4.2 Roary

This tool compares the pangenome of a genome dataset, identifying the core genome and accessory genome. Find out more about the tool here: <https://sanger-pathogens.github.io/Roary/>. 

First, we selected the ```.gff``` files generated previously with the Prokka analysis of the 18 restricted *Rickettsia* genomes, using this following command line in a new folder:

```
roary -f roary_out *.gff
```
Then, we execute the script ```roary_plots.py``` found in the Roary GitHub (<https://sanger-pathogens.github.io/Roary/>):

```
python $directory_script/roary_plots.py $directory_output_roary/roary_out/accessory_binary_genes.fa.newick $directory_output_roary/roary_out/gene_presence_absence.csv
```

Comparison between between the 3 resistant and the 15 *Rickettsia* strains were investigated using the ```query-pan-genome``` command line implemented in Roary (with the ```-a difference``` and ```-a intersection``` options)

```
## Differences :
query_pan_genome -a difference --input_set_one 1.gff,2.gff,3.gff --input_set_two 3.gff,4.gff,5.gff,6.gff,7.gff,8.gff,9.gff,10.gff,11.gff,12.gff,13.gff,14.gff,15.gff

## Intersection:
query_pan_genome -a intersection -i 1.gff,2.gff,3.gff 4.gff,5.gff,6.gff,7.gff,8.gff,9.gff,10.gff,11.gff,12.gff,13.gff,14.gff,15.gff
```

with ```"1" to "3"``` the name of the Resistant associated genomes, and with ```"4" to "15"``` the Susceptible associated genomes.



