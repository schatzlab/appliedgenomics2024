## Assignment 3: BWT and Variant Calling
Assignment Date: Monday, September 23, 2024 <br>
Due Date: Monday, September 30, 2023 @ 11:59pm <br>

### Assignment Overview

In this assignment you will implement the BWT and explore the requirements for variant calling. The programming exercises can be computed in any programming language, although we recommend python (or C++, Java, or Rust). R is generally inefficient at string processing unless you take great care. See the resources at the bottom of the page for tips for the variant calling exercises.

As a reminder, any questions about the assignment should be posted to [Piazza](https://piazza.com/class/m09t5q6qles40a).


### Question 1. BWT Encoding [20 pts]

In the language of your choice, implement a BWT encoder and encode the string below. Faster (Linear time) methods exist for computing the BWT, although for this assignment you can use the simple method based on standard sorting techniques. Your solution does *not* need to be an optimal algorithm and can use O(n^2) space and O(n^2 lg n) time. 

Here is the recommended pseudo code (make sure to submit your code as well as the encoded string):

```
computeBWT(string s)
  ## add the magic end-of-string character
  s = s + "$"
 
  ## build up the BWM from the cyclic permutations
  ## note the ith cyclic permutation is just "s[i..n] + s[0..i]"
  rows = []
  for (i = 0; i < length(s); i++)
    rows.append(cyclic_permutation(s, i))

  ## just use the builtin sort command to sort the cyclic permutations
  sort(rows)

  ## now extract the last column
  bwt = ""
  for (i = 0; i < length(s); i++)
    bwt += substr(rows[i], length(s), 1)
  return bwt
```

String to encode:
```
I_am_fully_convinced_that_species_are_not_immutable;_but_that_those_belonging_to_what_are_called_the_same_genera_are_lineal_descendants_of_some_other_and_generally_extinct_species,_in_the_same_manner_as_the_acknowledged_varieties_of_any_one_species_are_the_descendants_of_that_species._Furthermore,_I_am_convinced_that_natural_selection_has_been_the_most_important,_but_not_the_exclusive,_means_of_modification.
```


### Question 2. BWT Decoder [20 pts]

In the language of your choice, implement a BWT decoder and decode the string below. 

One of the essential properties of the BWT is that it can be decoded back into the source text without any other additional information. This is accomplished by iteratively applying the Last-First property starting with the first character of the BWT until reaching the end of string character `'$'`. The Last-First property states there is an equivalence between the ith occurrence of a character in the first column and the ith occurrence of that character in the last column. This equivalence can be evaluated by counting how many occurrences of a character are present in the BWT string (the last column of the BWM) or by counting characters in the first column (which you will have to determine from the BWT itself). Again, faster methods exist (the FM-index) to determine the rank of each character but you can just count it explicitly here.

The pseudocode for decoding the string is as follows:

```
decodeBWT(String bwt) 
  firstCol = makeFirstColumn(bwt)
  text = ""
  
  ## By construction, '$' always starts the zeroth row
  row = 0;
  while (bwt.charAt(row) != '$')
      text.append(bwt.charAt(row));
      row = applyLF(firstCol, bwt.charAt(row), rank(bwt, row));
  
  return reverse(text)
```

String to decode:
```
.uspe_gexr_______$..,e.orrs,sdddeedkdsuoden-tf,tyewtktttt,sewteb_ce__ww__h_PPsm_u_naseueeennnrrlmwwhWcrskkmHwhttv_no_nnwttzKt_l_ocoo_be___aaaooaAakiiooett_oooi_sslllfyyD__uouuueceetenagan___rru_aasanIiatt__c__saacooor_ootjeae______ir__a
```

Hint: use your sourcecode from Q1 to debug Q2. Also start with simple strings like GATTACA or your own name.


### Question 3. Binomial Distribution [20 pts]

- 3a. For coverage n = 10 to 200, calculate the maximum number of minor allele reads (round down) that would make your one-sided binomial test reject the null hypothesis p=0.5 at 0.05 significance. Plot coverage on the x-axis and (number of reads)/(coverage) on the y-axis. Note this is the minimum number of reads that are necessary to believe we might have a heterozygous variant on the second haplotype rather than just mere sequencing error.

- 3b. What asymptote does the plot seem to approach? Why is this?



### Question 4. Identifying a Specific Variant [20 pts]

*Three friends, Ezra, Sabine, and Zeb go out to lunch together and, in discussing the menu, all realize that they all have a specific culinary preference. They believe the cause of this preference to be genetic. Can you identify what SNP causes this preference and what the preference is?*

Download the read set from here: [https://github.com/schatzlab/appliedgenomics2024/tree/main/assignments/assignment4/input_files]([https://github.com/schatzlab/appliedgenomics2022/tree/main/assignments/assignment4/input_files](https://github.com/schatzlab/appliedgenomics2024/tree/main/assignments/assignment3/input_files))

For this question, you may find this tutorial helpful: [https://hbctraining.github.io/In-depth-NGS-Data-Analysis-Course/sessionVI/lessons/02_variant-calling.html](https://hbctraining.github.io/In-depth-NGS-Data-Analysis-Course/sessionVI/lessons/02_variant-calling.html)

To answer the following questions, you will need to run several alignment and variant calling tools. You can run these on the command line or embed them into a jupyter notebook, but make sure to record the exact commands you used.

- 4a. You are able to narrow down the SNP to a specific region in chromosome 11. Using bowtie2, align their FASTQ files to the provided reference file. How many reads does each file have and how many are successfully mapped? [Hint: Use `samtools flagstat`.] 

- 4b. How many SNPs and indels does each file have? [Hint: Sort the SAM file first. Then, call variants with `freebayes`. Summarize using `bcftools stats`.]

- 4c. You now know which SNPs and indels each friend has. However, you want to know which variant they all share. How many variants are shared between all three friends? [Hint: Use `bcftools isec` after normalizing the variants.]

- 4d. Between the variants shared between all three friends, which is likeliest to cause a phenotype of interest? [Hint: The variant should be homozygous in all 3 samples. You can search for variants at a certain chromosome and position at https://www.ncbi.nlm.nih.gov/snp/advanced/. Remember, the position in the intersected VCF is the position within the region we're looking at, so you will have to find the starting location of the region! Use `bcftools view -i 'GT="1/1"' PREFIX.vcf` to filter for the correct genotype.]

- 4e. What is the phenotype? [Hint: Search the name of the gene associated with the variant you found in 4d.]


### Packaging

The solutions to the above questions should be submitted as a single PDF document that includes your name, email address, and all relevant figures (as needed). If you use ChatGPT for any of the code, also record the prompts used. Submit your solutions by uploading the PDF to [GradeScope](https://www.gradescope.com/courses/839343), and remember to select where in your submission each question/subquestion is. The Entry Code is: Z3J8YV. 

If you submit after this time, you will use your late days. Remember, you are only allowed 4 late days for the entire semester!



### Resources


#### [Bowtie2](https://github.com/BenLangmead/bowtie2) - Short read aligner

```
## Install bowtie2 and samtools via mamba/conda
$ mamba create -n assignment3 bowtie2
$ mamba activate assignment3
$ mamba install samtools

## Build a bowtie2 index (BWT)
$ bowtie2-build ref.fa ref

## Now align reads, sort, and compute alignment stats
$ bowtie2 -x ref -1 PREFIX.1.fq -2 PREFIX.2.fq -S PREFIX.sam
$ samtools sort PREFIX.sam -o PREFIX.bam
$ samtools stats PREFIX.bam > PREFIX.stats
```


#### [FreeBayes](https://github.com/ekg/freebayes) - Small variant identification

```
$ mamba install freebayes
$ freebayes -f ref.fa PREFIX.bam > PREFIX.vcf
```

#### [bcftools](https://samtools.github.io/bcftools/bcftools.html) - VCF summary

```
$ mamba install bcftools
$ bcftools stats PREFIX.vcf > PREFIX.vcf.stats

## normalize, compress, and index the variants before comparing
$ bcftools norm -f ref.fa PREFIX.vcf > PREFIX.norm.vcf
$ bgzip PREFIX.norm.vcf
$ bcftools index PREFIX.norm.vcf.gz
...
$ bcftools isec PREFIX1.norm.vcf.gz PREFIX2.norm.vcf.gz PREFIX3.norm.vcf.gz -p norm -n=3
```


