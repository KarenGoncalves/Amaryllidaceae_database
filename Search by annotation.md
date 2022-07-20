# Search by annotation

To search a transcript ID by its annotation you will use in the command line something similar to `Ctrl + F` when you search texts elsewhere. 

The command for this is explained in the [glossary](https://github.com/KarenGoncalves/Amaryllidaceae_database#readme): `grep`.

## Summary of commands to use

### Take a quick peak at the annotation files:
```
head $HOME/projects/def-desgagne/amaryllidaceaeData/annotation_amaryllidaceae.tsv
head $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv
```

### Search for transcripts with specific annotation

Replace **_word_** below with the word you want:

```grep 'word' $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv```

### Filter result to get specific species

|Species|Acronyms|
|:------|:-------|
|_Leucojum aestivum_|`La`|
|_Narcissus papyraceus_|`Np`|
|_Narcissus spp._ King Alfred|`NKa`|

Put the acronym between `^` and `_`:

```grep 'word' $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv | grep '^La_'```

### Save search into file 

```grep 'word' $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv > $HOME/myResult.tsv```

### Downloading the files

Follow the steps at the bottom of the page

## Using `grep` to search the annotation files

The annotation files are stored in the project folder for the account `def-desgagne`:
- Complete annotation file (includes that may not be well supported in the assembly, use with care): `$HOME/projects/def-desgagne/amaryllidaceaeData/annotation_amaryllidaceae.tsv`
- Filtered annotation file (higher confidence sequences): `$HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv`

Files that end in .tsv are basically excell sheets. If you want to know what they look like, just copy the script below and paste on the server:

```
head $HOME/projects/def-desgagne/amaryllidaceaeData/annotation_amaryllidaceae.tsv
head $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv
```

These files contain one line for each transcript, including the gene and transcript IDs, the result of blast searches against different databases, GO term annotations (based on the blast result), etc. If there is no result for the annotation, the slot reserved for it will contain a `.`.

If you want to look for, say, methyltransferases in the filtered annotation file, you must copy the script below and paste on the server:

```
grep --ignore-case 'methyltransferase' $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv
```

This will print to the screen all the transcripts for which the word "methyltransferase" is present in any of the annotation sources.

The gene and transcript IDs all start with two letters that indicate the species from which they come: 

|Species|Acronyms|
|:------|:-------|
|_Leucojum aestivum_|`La`|
|_Narcissus papyraceus_|`Np`|
|_Narcissus spp._ King Alfred|`NKa`|
 
 So, if you are only interested in methyltransferases from, let's say _L. aestivum_, you need to filter the result you got earlier:
 
```
grep --ignore-case 'methyltransferase' $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv | grep '^La_'
```
 
 Now that you were able to get what you wanted, save the result into a file in your home folder:
 
```
grep --ignore-case 'methyltransferase' $HOME/projects/def-desgagne/amaryllidaceaeData/filteredAnnotationAmaryllidaceae.tsv > $HOME/La_methyltransferases.tsv
```
 
 ## Get the transcript or gene IDs and use them to get the sequences
 
 Working with the result of the methyltransferase search ($HOME/La_methyltransferases.tsv) use the code below to get the IDs:
 - Gene IDs: 
 ```
 awk '{print $1}' $HOME/La_methyltransferases.tsv > $HOME/La_methyltransferases_geneIDs.tsv
 ```
 - Transcript IDs: 
 ```
 awk '{print $2}' $HOME/La_methyltransferases.tsv > $HOME/La_methyltransferases_transcriptIDs.tsv
 ```

Awk is another software and we use it to search for the first ($1) or second ($2) column, which contains the gene and transcript IDs respectively. 

With this new file we can get the sequences for the transcripts (if we use the gene IDs, we will get all the transcripts for that gene):

- Gene IDs: 
```
grep -A 1 -f $HOME/La_methyltransferases_geneIDs.tsv $HOME/projects/def-desgagne/amaryllidaceaeData/blast_dbs/amaryllidaceae.fasta > $HOME/La_methyltransferases.fasta
```
- Transcript IDs: 
```
grep -A 1 -f $HOME/La_methyltransferases_transcriptIDs.tsv $HOME/projects/def-desgagne/amaryllidaceaeData/blast_dbs/amaryllidaceae.fasta > $HOME/La_methyltransferases.fasta
```

## Downloading the files from the server

You will need the exact location of your files in the server to be able to copy them. If you saved them to the same place as in the examples above, follow the steps below. If you saved to a different location, you can get the location by looking at the script you used.

1. On MobaxTerm, while connected to the server, paste this code in your command line: `echo $HOME`
2. Copy the result.
3. On the left-side pane, paste the result of the `echo` command on the address bar (you can see I pasted the result and the text is selected in the bar on the image)
![Going to the right folder](https://user-images.githubusercontent.com/22644195/179847971-9d8a3656-f38f-4a69-88e3-15f037ded610.png)
4. Press `Ctrl` and click on all the files you wish to download.
5. Still on the left pane, between the bar that reads "Quick connect" and the address bar, click on the symbol for download (down arrow in blue).
6. On the pop-up window, select the folder in your computer where you want to save your files.
7. Wait for the download to finish before logging out.
