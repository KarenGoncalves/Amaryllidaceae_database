# Blast a sequence against Amaryllidaceae database

To blast against the database, you need to use the following script:

```
blastn -query ${mySequence}\
 -db ${database}\
 -task blastn\
 -max_hsps 8\
 -outfmt '10'\
 > ${myResult}
```

Where:
- `${mySequence}` is the path to your fasta file
- `${database}` is the path to the database, it can be either:
  - The unfiltered database (containing the complete output of the assemblies): `${HOME}/projects/def-desgagne/amaryllidaceaeData/blast_dbs/wholeAmaryllidaceae`
  - The filtered database (containing only high confidence transcripts): `${HOME}/projects/def-desgagne/amaryllidaceaeData/blast_dbs/amaryllidaceae`
- max_hsps indicates the number of alignments to keep for each query sequence. Here I put 8, but feel free to change it.
- `oufmt` is the output format (check other options for output format **_[here](https://www.ncbi.nlm.nih.gov/books/NBK279684/table/appendices.T.options_common_to_all_blast/)_**):
  - Format 10 is just a table (with `,` separating the columns) containing the information of each alignment

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
 -max_hsps 8\
 -outfmt '10'\
 > ${HOME}/blastn_result
 ```



