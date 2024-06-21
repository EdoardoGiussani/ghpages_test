---
title: Usage
layout: default
---

# Basic Usage
Open the terminal in the same directory where the input file (e.g. `your_fasta.fa`) is located.

You can get the output file in Excel format (user-friendly) running:
```
mutfinder -x excel_output.xlsm your_fasta.fa
```
If you prefer the text outputs (machine-readable format) you shuold run instead:
```
mutfinder -m markers_output.tsv -M mutations_output.tsv -l literature_output.tsv your_fasta.fa
```

# Update marker database
MutFinder uses an SQLite database containing all information needed to perform the analysis.
The database contains the reference sequences, their annotation, the amino acid mutations, and all related marker information.

We will regularly update this database with new published markers.
You should always use latest version of our database and you can do it just by running this command:
```
mutfinder --update
```


# Options
You can modify the behaviour of MutFinder with the following options:
- `-v`/`--version`: print MutFinder version
- `--update`: update the database to the latest version
- `--skip-unmatch-names`: when a FASTA header does not match the pattern of the regular expression Mutfinder skips this sequence and continues the analysis. By default, MutFinder would exit with an error.
- `--skip-unknown-segments`: when the segment name is not present in the database (eg. `>yoursample_P3`) Mutfinder skips the sequence and continues analysis. By default, MutFinder would exit with error.
- `-r`/`--relaxed`: reports all markers where at least one mutation is found. By default MutFinder reports only markers if all mutations composing the markers are found in the input sequence
- `-n`/`--name-regex`: change the regular expression used to parse FASTA headers. By default the regular expression used is (?P<sample>.+)_(?P<segment>.+). More details can be found here.
- `-D`/`--db-file`: use a custom markers database instead of the default one
- `-m`/`--markers-output`: return as an output a text file containing the list of the identified markers (see “Markers output” for details) 
- `-M`/`--mutations-output`: return as an output a text file containing the list of the identified mutations (see “Mutations output” for details) 
- `-l`/`--literature-output`: return as an output a text file containing le list of the literature
- `-x`/`--excel-output`: return as an output an excel file containing all the outputs and a summary table (see “Excel output” for details) 

# Input File Format
Mutfinder can analyze multiple A(H5N1) Influenza virus sequences simultaneously, regardless of their order within the file.
It can handle partial and complete genome sequences and analyze sequences from one or more samples.
You must provide a single file containing all the nucleotide sequences in FASTA format (an example can be downloaded here).
Sequences must adhere to the IUPAC code.
MutFinder relies on the FASTA header (the name of your sequence) to assign the sequence to a specific segment.
For this reason, the header must contain both a sample ID (consistent among segments of the same sample) and the segment name as reported in the database (e.g., for avian influenza, possible segments are `PB2`, `PB1`, `PA`, `HA`, `NP`, `NA`, `MP`, `NS`).

By default, the tool expects the sample ID to precede the segment name separated by an underscore (e.g. `my_sample_PB2`).

If your sequences have a different name composition, you don’t have to rename all your sequences, instead you can use the option --name-regex.
This works with python regular expressions, using a first capture group to find the sample and a second capture group to find the segment.
If in your fasta headers the segment label is located before the sample ID you can use named capture groups (`sample` and `segment`). Here we provide some examples with most common cases:

| FASTA Header              	| Regular Expression                |
| ----------------------------- | --------------------------------- |
| my_sample_PB2             	| `(.+)_(.+)`                   	|
| my_sample\|PB2            	| `(.+)\\|(.+)`                 	|
| my_sample-PB2             	| `(.+)-(.+)`                   	|
| my_sample-PB2_something_else  | `(.+)-(.+?)_`                 	|
| PB2_my_sample             	| `(?P<segment>.+?)_(?P<sample>.+)` |
 
**_NOTE:_** to find the regular expression that better fits your data you can try it on [Regex101](https://regex101.com/) selecting `Python` flavor.
