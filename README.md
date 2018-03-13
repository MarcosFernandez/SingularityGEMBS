# Singularity recipe for GEMBS Container.

GEMBS is a bioinformatic pipeline designed for hightroughput analysis of DNA methylation from whole genome bisulfites sequencing data (WGBS). It implements GEM3, a high performance read aligner and BScall, a variant caller specifically for bisulfite sequencing data.

GEMBS can be found on [https://github.com/heathsc/gemBS.git](https://github.com/heathsc/gemBS.git) or can be run through a Singularity Container.

## Singularity

Singularity enables users to have full control of their environment. Singularity container is used to package GEMBS and its dependencies. This means that you don’t have to ask your cluster admin to install anything for you, just get the container and run it.

Singularity is HPC friendly, it can be easily run on a cluster environment. The container itself, a file that sits on your computer, can be dropped into a folder on your cluster, and run like a script. 


### 1. Install Singularity

Instructions can be found on the [singularity site](https://singularityware.github.io).


### 2. Build Singularity Recipe

    sudo singularity build gemBS.simg Singularity

### 3. Run commands

Set of common GEMBS commands mapping a folder for the data output.

1. Index

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg index -i /data/folder_reference/reference.fa
```    

2. Create JSON Configuration file

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg prepare-config -t metadata.csv -j /data/metadata.json
```

3. Mapping

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg mapping -I folder_reference/reference.BS.gem -f flowcell_index_lane -j metadata.json -i ./fastq/ -o /data/mappings/ -t 2 -p
```

4. Merging mappings

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg merging-sample -i ./mappings/ -j metadata.json -s test -t 2 -o /data/sample_mappings/ -d /data/tmp/
```

5. Variant and Methylation Calling

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg bscall -r folder_reference/reference.fa -e Species -s test -c chr1 -i ./sample_mappings/test.bam -o /data/chr_snp_calls/

singularity run --bind /home/user/myfolder:/data/ gemBS.simg bscall -r folder_reference/reference.fa -e Species -s test -c chr2 -i ./sample_mappings/test.bam -o /data/chr_snp_calls/
```

6. Concatenation of calls

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg bscall-concatenate -s test -l ./chr_snp_calls/chr1.bcf ./chr_snp_calls/chr2.bcf -o /data/merged_calls/
```

7. Methylation Filtering

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg methylation-filtering -b ./merged_calls/ -o /data/filtered_meth_calls/
```

8. Mapping Report

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg bsMap-report -j metadata.json -i ./mappings/ -n TEST_GEMBS -o /data/reports/mappings/
```

9. Variants Report

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg variants-report -l chr1 chr2 -j metadata.json -i ./chr_snp_calls/ -n TEST_GEMBS -o /data/reports/variants/
```

10. CpG Report

```
singularity run --bind /home/user/myfolder:/data/ gemBS.simg cpg-bigwig -c ./filtered_meth_calls/test_cpg.txt.gz -l folder_reference/chr.len -n TEST_GEMBS -o /data/Tracks/
```

## Licensing

GEMBS is licensed under GPL.

