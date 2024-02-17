
# Unix Assignment

## Data Inspection

### Attributes of  ``` fang_et_al genotypes ```

 ```Here is my snippet of code for data inspection```

```sh
  wc fang_et_al_genotypes.txt
  awk -F "\t" ' {print NF ; exit} ' fang_et_al_genotypes.txt
  du -h fang_et_al_genotypes.txt
  ```

By inspecting this file, I learned that

I. It contains 2783 lines, 2744038 words and 11051939 characters
II. It contains 986 columns
III. It has a file size of 6.7M


### Attributes of  ``` snp_position.txt ```

``` Here is my snippet of code for data inspection```

```sh
wc snp_position.txt
awk -F "\t" '{print NF; exit}' snp_position.txt
du -h snp_position.txt
```
By inspecting this file, I learned that.

I. It contains 984 lines, 13198 words, 82763 characters
II. It contains 15 columns
III. It has a file size of 49k
   

## Data Processing
### Maize data

Here is my snippet of code used for data processing

``` grep -E '^ZMP|^1' fang_et_al_genotypes.txt | cut -f 4-986 > Maize_phenotypes.txt.txt```

```awk -f transpose.awk Maize_phenotypes.txt > transposed_maize_genotypes.txt```

```awk '{print $0 | "sort -k1,1"}' transposed_maize_genotypes.txt > Maize_sorted.txt ```

```cut -f 1,3,4 snp_position.txt | sed '1d' | sort -k1,1 > snp_position_sorted.txt```

```join -t $'\t' -1 1 -2 1 snp_position_sorted.txt Maize_sorted.txt > Maize_snp_sorted.txt```

```awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Maize_snp_sorted.txt | awk 'NR==1 || /unknown/' > unknown_maize_snp.txt```

```awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"} NR==1 || /multiple/' Maize_snp_sorted.txt > multiple_maize_snp.txt```

```for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' Maize_snp_sorted.txt | sort -k3,3n | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_maize_increasing.txt; done```

 ```for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' Maize_snp_sorted.txt | sort -k3,3nr | sed 's/?/-/g' | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_maize_decreasing.txt; done```


Here is a brief description of what my code does
1. The SNP_ID and SNP data for the maize group (ZMMLR, ZMMIL, ZMMMR ) in the third column were pattern-selected and columns 4 through 986 were extracted with headers.
2. The data is transposed to convert rows into columns.
3. The contents of ``` transposed_maize_ genotypes.txt```   were sorted based on SNP_ID.
4. The files ```snp_position_sorted.txt``` and ```Maize_sorted.txt``` are joined together.
5. SNPs with unknown and multiple positions in the genome were extracted from the joint file. A header specifying SNP_ID, Chromosome, Position and Genotype Data was added.
6. A for loop is applied to separate the data according to chromosome and sorted based on increasing and decreasing positions on the third column. A header is included to specify SNP_ID, Chromosome, Position and Genotype all within the data file. 
For the files with decreasing positions, missing data encoded by ? is replaced by - within the for loop command.
7. A total of 20 files with SNPs were created for increasing and decreasing positions, 1 file with unknown positions and 1 file with multiple positions.


# Data Processing
## Teosinte Data

Here is my snippet of code used for data processing.

```grep -E 'NR==1|ZMP' fang_et_al_genotypes.txt | cut -f 4-986 > Teosinte_phenotypes.txt```

```awk -f transpose.awk Teosinte_phenotypes.txt > transposed_teosinte_genotypes.txt```

```sort -k1,1 transposed_teosinte_genotypes.txt > Teosinte_sorted.txt```

``` join -t $'\t' -1 1 -2 1 snp_position_sorted.txt Teosinte_sorted.txt > Teosinte_snp_sorted.txt```

``` awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Teosinte_snp_sorted.txt | awk 'NR==1 || /unknown/' > unknown_teosinte_snp.txt```

``` awk -v OFS='\t' 'BEGIN {print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' Teosinte_snp_sorted.txt | awk 'NR==1 || /multiple/' > multiple_teosinte_snp.txt```

``` for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' Teosinte_snp_sorted.txt | sort -k3,3n | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_teosinte_increasing.txt; done```


``` for ((i=1; i<=10; i++)); do awk -v i="$i" '{if ($2==i) print $0}' Teosinte_snp_sorted.txt | sort -k3,3nr | sed 's/?/-/g' | awk -v OFS='\t' 'BEGIN{print "SNP_ID", "Chromosome", "Position", "Genotype_data"}{print}' > chr"$i"_teosinte_decreasing.txt; done```

Here is a brief description of what my code does
1. The SNP_ID and SNP data for the maize group (ZMPBA, ZMPIL, ZMPJA) in the third column were pattern-selected and columns 4 through 986 were extracted with headers.
2. The data is transposed to convert rows into columns.
3. The contents of ``` transposed_teosinte_ genotypes.txt```   were sorted based on SNP_ID.
4. The files ```snp_position_sorted.txt``` and ```teosinte_sorted.txt``` are joined together.
5. SNPs with unknown and multiple positions in the genome were extracted from the joint file. A header specifying SNP_ID, Chromosome, Position and Genotype Data was added.
6. A for loop is applied to separate the data according to chromosome and sorted based on increasing and decreasing positions on the third column. A header is included to specify SNP_ID, Chromosome, Position and Genotype all within the data file. 
7. For the files with decreasing positions, missing data encoded by ? is replaced by - within the for loop command.
8. A total of 20 files with SNPs were created for increasing and decreasing positions, 1 file with unknown positions and 1 file with multiple positions.
