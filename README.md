# MyGenome
Analysis for ABT480/CS485G genome assembly

## 1. Analysis of sequence quality
The F1 and R1 sequence datasets were analyzed using FASTQC:
```bash
ssh -Y <user@virtualmachine>
cd MyGenome
fastqc &
```
Load in F1 and R1 datasets into GUI interface.
Take screen shots of output files:

![F1screenshot.png](/data/F1screenshot.png)

## 2. Ran trimmomatic
```bash
java -jar ~/trimmomatic-0.38.jar PE -threads 16 -phred33 -trimlog file.txt \
U249_1.fq.gz U249_2.fq.gz U249_1_paired.fastq U249_1_unpaired.fastq U249_2_paired.fastq U249_2_unpaired.fastq \
ILLUMINACLIP:adaptors.fasta:2:30:10 SLIDINGWINDOW:20:20 MINLEN:100
```

## 3. Count number of forward reads remaining
First, run head to view the headers of each read to find a suitable string for running grep
```bash
head U249_1_paired.fastq
```
Use the beginning of the header for each read to count the number of reads with grep
```bash
grep -c @A00261:902:HGC52DSX7: U249_1_paired.fastq
```

## 4. Genome assembly using Velvet
The genome assembly was done on the University of Kentucky Morgan Compute Cluster. The velvet command was run using the [velvetoptimiser_noclean.sh](/velvetoptimiser_noclean.sh) SLURM script:
```bash
sbatch velvetoptimiser_noclean.sh U249 61 131 10
```
This command submits the script containing the velvet command with arguments for the genome name, starting k-mer value, ending k-mer value, and step size as a SLURM job into the supercomputer job queue
