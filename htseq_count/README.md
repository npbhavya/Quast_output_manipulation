# Htseq count manipulation
Take multiple htseq count results and add them to one tsv table

## Pre-requisites 
Run htseq counts command and generate a counts output file. This is an example htseq command \
`htseq-count -f bam -r pos -s no -m intersection-nonempty -t CDS -i Parent <bam file> <gff file> > <output counts file>`

## Dependecies 
**Python 3** \
Python packages 
- argparse
- numpy
- pandas
- collection 

## Usage 
`python counts_table.py --help` \
usage: counts_table.py [-h] [-d DIRECTORY] [-g GFF] [-o OUTPUT]

Take multiple output files and consolidate to one file

optional arguments:
  -h, --help    show this help message and exit \
  -d DIRECTORY  Enter a directory with htseq count files \
  -g GFF        Enter the refernece gff file \
  -t type       Enter the type of the feature from the gff file \
  -o OUTPUT     Enter the output file name

## Example command 
`python counts_table.py -d Counts/ -g reference.gff -t type -o counts_output`

## Input files
This code takes the htseq counts from htseq. To setup the input files, first write the htseq counts into one directory.
1) Run htseq first for all the samples, \
`htseq-count -f bam -r pos -s no -m intersection-nonempty -t CDS -i Parent <bam file> <gff file> > <output counts file>`

2) write all the counts to the new directory, but make sure to name the counts files to sample-name.tsv, here are the commands to do this 

3) Require the reference gff file that was used in the command. has to be gff formatm not GTF.

## Downstream 
Run the bash command to generate a tsv file \
`sed -e "s/\[//g;s/\]//g;s/'//g;s|\t|,|g;s|,|\t|g " counts_output >counts_output.tsv`

## Assumptions

### Assumptions in the code about the gff file
The code grabs the ID=... and Name=... from the gff last column, assuming the last column for the "type" entry is in the order 

  *TTHERM_140 testing CDS 1 20000 1 + . ID=cds;Name=TTHERM_00898140;Note="kinase domain protein"*

If the last column's order changes the code needs to be changes on lines below in the gfaparser function. 

    id=split_fields[0].strip("ID=")
    name=split_fields[1].strip("Name=")

