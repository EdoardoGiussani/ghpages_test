---
title: Input file
layout: default
parent: Usage
nav_order: 3
permalink: docs/usage/input-file
---

# Input File
FluMut can analyze multiple A(H5N1) Influenza virus sequences simultaneously.
It can handle partial and complete genome sequences of multiple samples.
You must provide a single file containing all the nucleotide sequences in FASTA format (an example can be downloaded [here](TODO)).
Sequences must adhere to the IUPAC code.
FluMut relies on the FASTA header to assign the sequence to a specific segment and sample.
For this reason, the header must contain both a sample ID (consistent among sequences of the same sample) and one of the the following segment names: `PB2`, `PB1`, `PA`, `HA`, `NP`, `NA`, `MP`, `NS`.

By default, the tool expects the sample ID followed by an underscore and the segment name (e.g. `my_sample_PB2`). FluMut-GUI works only with this FASTA header structure.

# Custom FASTA header parsing
If your sequences have a different FASTA header composition, you can use the option `--name-regex`.
This works with Python regular expressions, using a first capture group to find the sample and a second capture group to find the segment.
Here we provide some examples with most common cases:

| FASTA Header                  | Regular Expression                |
| ----------------------------- | --------------------------------- |
| my_sample_PB2                 | `(.+)_(.+)`                       |
| my_sample\|PB2                | `(.+)\|(.+)`                      |
| my_sample-PB2                 | `(.+)-(.+)`                       |
| my_sample-PB2_something_else  | `(.+)-(.+?)_`                     |
| PB2_my_sample                 | `(?P<segment>.+?)_(?P<sample>.+)` |
 
>**_NOTE:_** to find the regular expression that better fits your FASTA header you can try it on [Regex101](https://regex101.com/) selecting `Python` flavor.

>**_NOTE:_** if in your fasta headers the segment label is located before the sample ID you can use [named groups](https://docs.python.org/3/howto/regex.html#non-capturing-and-named-groups) (`sample` and `segment`) as shown in the last row of the table.
