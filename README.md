# Investigating-Pancreatic-Adenocarcinoma-with-GEMmaker
This repository contains all the steps I used to build GEMs from publicly available RNAseq data to study pancreatic cancer. 

In order to build my GEMs, I used the Nextflow workflow called GEMmaker: https://github.com/SystemsGenetics/GEMmaker. Before I installed GEMmaker, I first installed Nextflow by entering the folliwng commands in my Linux command line: "curl -s https://get.nextflow.io | bash" and "sudo apt install openjdk-8-jre" (the second command was used to install java).
To install Singularity container software and dependincies, I followed the instructions found here: https://sylabs.io/guides/3.7/user-guide/quick_start.html#quick-installation-steps
Next, I cloned GEMmaker from GitHub with the following command: "git clone https://github.com/SystemsGenetics/GEMmaker.git --branch develop"
In order to start building the GEM, I first needed to index the human genome. To do this, the first step was to download the HG 38 human genome (http://ftp.ensembl.org/pub/release-101/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz) and the GTF file (ftp://ftp.ensembl.org/pub/release-101/gtf/homo_sapiens/Homo_sapiens.GRCh38.101.gtf.gz) and place the two files in the /demo/references directory
To prepare the human genome for FASTQ reading I ran the following command: "singularity exec -B ${PWD} docker://gemmaker/hisat2:2.1.0-1.1 hisat2-build Homo_sapiens.GRCh38.dna.primary_assembly.fa HG38"
The above command created a series of HG38 files in my reference directory.
Next, I edited the SRA_IDs.txt file in the /demo directory and inserted SRA IDs from a publicly available study on pancreatic cancer: https://trace.ncbi.nlm.nih.gov/Traces/sra/?study=SRP313524. The study contained three sequences derived from pancreatic cancer and three sequences derived from normal pancreatic tissue, so I built one GEM by placing the three pancreatic cancer sequences in the SRA_IDs.txt file and another with the three normal pancreatic tissue samples.
After placing the correct SRA IDs in the SRA_ID's.txt file, I then went to the main GEMmaker directory and edited the nextflow.config. I enabled the hisat2 alignment tool and disabled all others. Then, I changed the base name for hisat2 to "HG38"and indicated where the indexed HG38 files and GTF file could be found.
Finally, I then ran the following command to prepare my GEMs: "nextflow run main.nf -profile standard -with-singularity -with-report -with-timeline"

After repeating this with both the pancreatic cancer samples and normal pancreatic tissues samples, I successfully built two GEMs, one for pancreatic cancer and one for normal pancreatic tissue. 

The next goal for this experiment is to merge these GEMs using the GEMprep workflow (https://github.com/SystemsGenetics/GEMprep) and then use the KINC workflow (https://github.com/SystemsGenetics/KINC) to build a gene co-expression network. Unfortunately, I have been encountering some issues when trying to merge my GEMs with GEMprep, so my project has reached a bit of a standstill... TO BE CONTINUED
