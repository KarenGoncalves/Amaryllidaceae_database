# Blast a nucleotide sequence against nucleotide database

To blast against the database, you need to use the following script:

```
blastn -query ${mySequence}\
 -db ${database}\
 -task blastn\
 -max_hsps 8\
 -outfmt '7'\
 > ${myResult}
```

Where:
- `${mySequence}` is the path to your fasta file
- `${database}` is the path to the database, it can be either:
  - NCBI nucleotide database: `/cvmfs/bio.data.computecanada.ca/content/databases/Core/blast_dbs/2022_03_23/nt`
  - Amaryllidaceae database: `${HOME}/projects/def-desgagne/amaryllidaceaeData/blast_dbs/Amaryllidaceae`
  - Amaryllidoideae database: `${HOME}/projects/def-desgagne/amaryllidaceaeData/blast_dbs/Amaryllidoideae`
- max_hsps indicates the number of alignments to keep for each query sequence. Here I put 8, but feel free to change it.
- evalue indicates the maximum evalue a hit can have to appear in your results. Remember, the higher this value, the higher the number of bad hits you allow
- `oufmt` is the output format (check other options for output format **_[here](https://www.ncbi.nlm.nih.gov/books/NBK279684/table/appendices.T.options_common_to_all_blast/)_**):
  - Format 7 is just a table (with tabs separating the columns) containing the information of each alignment

## The script

After you log in into the server and copy the fasta sequence into your `$HOME` folder. Copy the script below and paste into the server. Note:
- Here my file is called `myFasta.fasta`, so you need to replace the first line below with the actual name of your file.
- Here I am using the filtered database, if you want to use the unfiltered one, replace the path of the database with the one indicated above.

```
mySequence=${HOME}/myFasta.fasta
account=def-desgagne
database=${HOME}/projects/${account}/amaryllidaceaeData/blast_dbs/amaryllidaceae

module load StdEnv/2020 gcc/9.3.0
module load blast+/2.12.0

cd ${SCRATCH}

srun --account=${account}\
blastn -query ${mySequence}\
 -db ${database}\
 -task blastn\ 
 -evalue 0.0001\
 -max_hsps 8\
 -outfmt '7'\
 > ${HOME}/blastn_result
 ```

# Blast a nucleotide or protein sequence against protein database

To blast against the database, you need to use the following script:

```
# If you have a nucleotide sequence, use blastx
blastx -query ${mySequence}\
 -db ${database}\
 -task blastx\
 -max_hsps 8\
 -evalue 0.0001\
 -outfmt '7'\
 > ${myResult}
 
# If you have a protein sequence, use blastp
blastp -query ${mySequence}\
 -db ${database}\
 -task blastp\
 -max_hsps 8\
 -evalue 0.0001\
 -outfmt '7'\
 > ${myResult}
```

Where:
- `${mySequence}` is the path to your fasta file
- `${database}` is the path to the database, it can be either:
  - NCBI non-redundant protein database: `/cvmfs/bio.data.computecanada.ca/content/databases/Core/blast_dbs/2022_03_23/nr`
  - Uniprot/Swissprot peptide database (Release 2022_03): `/project/${ACCOUNT}/uniprot/uniprot_2022_03`
    Replace `${ACCOUNT}` with your sponsor account name (def-laboidp, def-desgagne. If you are from Hugo's lab, talk to me to get the files and create a folder for your team)
- max_hsps indicates the number of alignments to keep for each query sequence. Here I put 8, but feel free to change it.
- evalue indicates the maximum evalue a hit can have to appear in your results. Remember, the higher this value, the higher the number of bad hits you allow
- `oufmt` is the output format (check other options for output format **_[here](https://www.ncbi.nlm.nih.gov/books/NBK279684/table/appendices.T.options_common_to_all_blast/)_**):
  - Format 7 is just a table (with tabs separating the columns) containing the information of each alignment
  
## The script

After you log in into the server and copy the fasta sequence into your `$HOME` folder. Copy the script below and paste into the server. Note:
- Here my file is called `myFasta.fasta`, so you need to replace the first line below with the actual name of your file.
- Here I am using the filtered database, if you want to use the unfiltered one, replace the path of the database with the one indicated above.

```
#########################################################################################
### Replace the text that comes after the "=" in each line by the correct one for you ###
#########################################################################################
mySequence=${HOME}/myFasta.fasta
account=def-desgagne
database=/project/${account}/amaryllidaceaeData/blast_dbs/Amaryllidaceae

########################
##### Run the code #####
########################

module load StdEnv/2020 gcc/9.3.0
module load blast+/2.12.0

cd ${SCRATCH}

######################################################
##### If you have a protein sequence, use blastp #####
######################################################

srun --account=${account}\
blastp -query ${mySequence}\
 -db ${database}\
 -task blastp\
 -max_hsps 8\
 -outfmt '7'\
 > ${HOME}/blastp_result
 
 
#########################################################
##### If you have a nucleotide sequence, use blastx #####
#########################################################
srun --account=${account}\
blastx -query ${mySequence}\
 -db ${database}\
 -task blastx\
 -max_hsps 8\
 -outfmt '7'\
 > ${HOME}/blastx_result
 ```

