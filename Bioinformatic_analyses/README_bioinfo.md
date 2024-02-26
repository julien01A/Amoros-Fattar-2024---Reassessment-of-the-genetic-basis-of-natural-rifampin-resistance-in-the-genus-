# Genomic analyses 

## 1 - Construction of the rpoB complete gene sequence of *R. lusitaniae* R-Om
(MARIE)


## 2 - Annotation of the *Rickettsia* genomes
We collected 78 genomes of *Rickettsia*, *Orientia* and *Cd. Megaira* from public databases (in fasta format, see Table S2 for the accession numbers used). A part of these genomes were already annotated, but we proceed a standardized annoation for all the 78 genomes. Thus, we (re)annoted the 78 genomes using Prokka v1.14.6 (<https://github.com/tseemann/prokka>, Seemann T. (2014), Prokka: rapid prokaryotic genome annotation. Bioinformatics. doi: 10.1093/bioinformatics/btu153) using the following command for each genome:

```
prokka $sample_name-genome.fasta --locustag $sample_name --prefix $sample_name --outdir Prokka-$sample_name --rfam --compliant --cpus 6
```

with "sample_name" the name of the genome to annotate.


## 3 - Phylogenomic analyses



## 4 - Identification of orthogroups specific to naturally resistant-rifampin Rickettsia species


