## Assignment 1: Chromosome Structures
Assignment Date: Wednesday, August 28, 2024 <br>
Due Date: Wednesday, Sept. 4, 2024 @ 11:59pm <br>

### Assignment Overview

In this assignment you will profile the overall structure of the genomes of several important species and then study human chromosome 22 in more detail.
As a reminder, any questions about the assignment should be posted to [Piazza](https://piazza.com/class/m09t5q6qles40a).

### Question 1: Chromosome structures [10 pts]

Download the chomosome size files for the following genomes (Note these have been preprocessed to only include main chromosomes):

1. [E. coli (Escherichia coli K12)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/ecoli.chrom.sizes) - One of the most commonly studied bacteria [[info]](https://en.wikipedia.org/wiki/Escherichia_coli)
2. [Yeast (Saccharomyces cerevisiae, sacCer3)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/yeast.chrom.sizes) - an important eukaryotic model species, also good for bread and beer [[info]](https://en.wikipedia.org/wiki/Saccharomyces_cerevisiae)
3. [Worm (Caenorhabditis elegans, ce10)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/ce10.chrom.sizes) - One of the most important animal model species [[info]](https://en.wikipedia.org/wiki/Caenorhabditis_elegans)
4. [Fruit Fly (Drosophila melanogaster, dm6)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/dm6.chrom.sizes) - One of the most important model species for genetics [[info]](https://en.wikipedia.org/wiki/Drosophila_melanogaster)
5. [Arabidopsis thaliana (TAIR10)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/TAIR10.chrom.sizes) - An important plant model species [[info]](https://en.wikipedia.org/wiki/Arabidopsis_thaliana)
6. [Tomato (Solanum lycopersicum v4.00)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/tomato.chrom.sizes) - One of the most important food crops [[info]](https://en.wikipedia.org/wiki/Tomato)
7. [Human (hg38)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/hg38.chrom.sizes) - us :) [[info]](https://en.wikipedia.org/wiki/Homo_sapiens)
8. [Wheat (Triticum aestivum, IWGSC)](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/wheat.chrom.sizes) - The food crop which takes up the largest land area [[info]](https://en.wikipedia.org/wiki/Wheat)

Using these files, make a table with the following information per species:

- Question 1.1. Total genome size

- Question 1.2. Number of chromosomes

- Question 1.3. Largest chromosome size and name

- Question 1.4. Smallest chromosome size and name

- Question 1.5. Mean chromosome length


### Question 2. Coverage simulator [20 pts]

- Question 2.1. How many 100bp reads are needed to sequence a 1Mbp genome to 3x coverage?

- Question 2.2. In the language of your choice, simulate sequencing 3x coverage of a 1Mbp genome with 100bp reads and plot the histogram of coverage. Note you do not need to actually output the sequences of the reads, you can just uniform randomly sample positions in the genome and record the coverage. You do not need to consider the strand of each read. The start position of each read should have a uniform random probabilty at each possible starting position (1 through 999,901). You can record the coverage in an array of 1M positions. Overlay the histogram with a Poisson distribution with lambda=3. Also overlay the distribution with a Normal distribution with a mean of 3 and a standard deviation of 1.73 (which is the square root of 3). Here is the pseudocode for the simulator:

```
num_reads = calculate_number_of_reads(genomesize, readlength, coverage)

## use an array to keep track of the coverage at each position in the genome
genome_coverage = initialize_array_with_zero(genomesize)

for (i = 0; i < num_reads; i++)
{
  startpos = uniform_random(1,genomesize-readlength)
  endpos = startpos + readlength - 1
  for (x = startpos; x <= endpos; x++)
  {
    genomecoverage[x] = genomecoverage[x] + 1
  }
}

maxcoverage = max(genomecoverage)

## use an array count how many positions have 0x coverage, have 1x coverage, have 2x coverage, ...
histogram = initialize_array_with_zero(maxcoverage)

for (x = 0; x < genomesize; x++)
{
  cov = genomecoverage[x]
  histogram[cov] = histogram[cov] + 1
}

## now plot the histogram
...

```


- Question 2.3. Using the histogram from Q2.2, how much of the genome has not been sequenced (has 0x coverage)? How well does this match Poisson expectations? How well does the normal distribution fit the data?

- Question 2.4. Now repeat the analysis with 10x coverage: 1. simulate the appropriate number of reads, 2. make a histogram, 3. overlay a Poisson distribution with lambda=10, 4. overlay with a Normal distribution with mean=10, standard deviation=3.16. 5. compute the number of bases with 0x coverage, and 6. evaluate how well it matches the Poisson expectation and Normal expectations.

- Question 2.5. Now repeat the analysis with 30x coverage: 1. simulate the appropriate number of reads, 2. make a histogram, 3. overlay a Poisson distribution with lambda=30, 4. overlay with a Normal distribution with mean=30, standard deviation=5.47 5. compute the number of bases with 0x coverage, and 6. evaluate how well it matches the Poisson expectation and Normal expectations.




### Question 3: Kmer Uniqueness [20 pts]

Download the human chomosome 22 from here: [https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/chr22.fa.gz](https://schatz-lab.org/appliedgenomics2024/assignments/assignment1/chr22.fa.gz)

#### Notes: 

- A kmer is a substring of length k. For example, the string GATTACA, has these 3-mers: GAT, ATT, TTA, TAC, ACA

- A string of length G has G - k + 1 kmers. For long strings, G - k + 1 is nearly the same as G e.g. for human using 19mers, 3,000,000,000 vs 2,999,999,986

- While a string of length G has G-k+1 kmers, there may be many fewer *distinct* kmers. For example, in the string "GCATCATCATCATCATCATCAT..." the kmers are: GCA, CAT, ATC, TCA, CAT, ATC, TCA, CAT, ATC, TCA, CAT, ... As such there are only 4 disinct kmers (GCA, CAT, ATC, TCA). Of these GCA occurs once and the others occur many times.

- If your computer runs out of RAM, you can use a portion of chromosome22 (e.g the first 20Mbp or smaller region). Just make to to mark which portion of the chromosome you are using. Also make sure that this region is not just N characters.

#### Questions:

- Question 3.1. How many As, Cs, Gs, Ts and Ns are found in the entire chromosome? If needed convert lowercase letters to uppercase, and any other character can be converted to N.

- Question 3.2. In the language of your choice, tally the frequency of 19-mers in the chromosome, and output the kmer frequency spectrum upto 1000 e.g. how many kmers occur 1 time, how many occur 2 times, how many occur 3 times, etc. For this, convert lowercase letters to uppercase, and any character that is not ACG or T can be converted to A (especially N characters). We recommend you use a dictionary (or hash table) to tally the frequencies using this pseudocode. In your writeup, show the kmer frequency spectrum for 1 to 20, e.g. how many kmers occur 1 time, how many occur 2, ..., how many occur 20 times. This can be done with the unix command `head`:

```
## initialize kmer length
k=19

## read genome, convert to upper canse and convert non-DNA to 'A'
genome_string = read_from_file("chr22.fa")

## dictionary that maps a kmer (like GAT) to a frequency (like 3)
kmer_frequency = initialize_dictionary()

## now scan the genome, extract kmers, and tally up their frequencies
for(i = 0; i < length(genome_string) - k + 1; i++)
{
  kmer = substring(genome_string, i, k)  
  kmer_frequency[kmer] = kmer_frequency[kmer] + 1
}

## now tally the frequencies in a dictionary that maps kmer frequency to count
## also determine the maximum kmer frequency

tally = initialize_dictionary()
all_kmers = kmer_frequency.keys()
max_frequency = 0

for (i = 0; i < length(all_kmers); i++)
{
  kmer = all_kmers[i]
  freq = kmer_frequency[kmer]
  tally[freq] = tally[freq] + 1
  
  if (freq > max_frequency)
  {
    max_frequency = freq
  }
}

## now print in sorted order
for (i = 1; i <= max_frequency; i++)
{
  if (tally.has_key(i))
  {
    freq = tally[i]
    print_to_file(i, freq)
  }
}
```

- Question 3.3. Using the output from 3.2, plot the kmer frequency spectrum: x-axis is the kmer frequency, and the y-axis is the number of kmers that occur x times. Make sure to plot both the x and y-axis in log space.

- Question 3.4. a) What percent of the genome is unique, e.g. what percent of the kmers occur 1 time. b) What percent of the genome is repetitive (occurs more than 1 time). c) What percent occurs more than 1000 times? 

  - Note: For this analysis, you should separately consider all of the kmers in the genome, e.g. the denominator will be G-k+1. When computing the unique percentage, use the number of unique kmers as the numerator. When computing repetitive percentages, make sure to separately count each instance of a repetitve kmer. For example the string "GCATCATCAT" has kmers: GCA, CAT, ATC, TCA, CAT, ATC, TCA, CAT. Of these 1/8 (12.5%) are unique and 7/8 (87.5%) are repetitive



### Question 4: Why Genomics? [10 pts]

- Question 4.1. Use ChatGPT (or your favorite LLM) to write an essay on why you are interested in genomics. Make sure to ask for references. Make sure to include both your prompt(s) and the output from the LLM

- Question 4.2. Comment on the output from the LLM - does it make logical sense, does it include any phrases you would not have written, do the citated papers exist and support the claims from the LLM?

- Question 4.3 Post your final prompt, the LLM's response, and which LLM you used to piazza. Include the link to the post in your submission


### Hints

- We highly recommend you use [Jupyter notebooks](https://jupyter.org/) that you can then "print" to a PDF. If you have issues printing to PDF, export to HTML, and then print to PDF.
- Many of the questions can be addressed with standard command line [tools](http://lh3lh3.users.sourceforge.net/biounix.shtml) such as `grep`, `wc`, `awk`, `sort`, `fold`, etc
- You may wish to try out [`datamash`](https://www.gnu.org/software/datamash/)
- You may find [`samtools`](http://www.htslib.org/) and especially `samtools faidx` helpful for indexing the fasta files
- Plotting can be done in any language; R or Python are recommended; Excel is okay but ugly :-P
- Be sure to clearly mark each question and subquestion in the PDF and then highlight each question in GradeScope
- If your laptop runs out of RAM for question 3, you can consider the first 1Mbp or 10Mbp of the genome. Just be sure to mark what part of the genome you considered




### Packaging

The solutions to the above questions should be submitted as a single PDF document that includes your name, email address, and 
all relevant figures (as needed). Make sure to clearly label each of the subproblems and give the exact commands and/or code snippets you used for solving the question. Submit your solutions by uploading the PDF to [GradeScope](https://www.gradescope.com/courses/839343), and remember to select where in your submission each question/subquestion is. The Entry Code is: Z3J8YV. 

If you submit after this time, you will start to use up your late days. Remember, you are only allowed 96 hours (4 days) for the entire semester!



