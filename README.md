# Lociq_shell

## Background

In short, the Lociq program is a MLST generator for closed, circular sequences that uses both sequence and structural components.  The program was designed with plasmid biology in mind as plasmid molecules are prone to selective pressures and recombination events that affect both plasmid sequence and plasmid structure.

This repository contains sample data and instructions for formatting sequences for use in the program..

## Setup directory and retrieve files

First, create a subdirectory for the sample files.  The example in the Lociq repository assumes the folder will be created as a subdirectory of the Lociq parent directory, though this is adjustable:
```
mkdir db
cd db
```

Next, download the sample data files
```
wget https://github.com/LBHarrison/Lociq_sample/blob/main/plsdb.names.bz2
wget https://github.com/LBHarrison/Lociq_sample/blob/main/combinedplasmid.csv
wget https://github.com/LBHarrison/Lociq_sample/blob/main/annotations.tar.bz2
```

## Preparation of Sample Set Annotation files

Annotation files are in the GFF3 format that contains the full *.fasta sequence.

Sample annotation files are contained in the compressed annotations.tar.bz2 file.

Annotation files may be extracted with the following:
```
tar xjvf annotations.tar.bz2
```

## Reference database

A reference database is necessary to address the potential for bias in the initial dataset.  The user may reference any database that is appropriate for their purpose.
The following may be run to acquire such a database, using PLSDB as an example:

Download the sequence files:
```
wget https://ccb-microbe.cs.uni-saarland.de/plsdb/plasmids/download/plsdb.fna.bz2
```

Extract the files
```
bzip2 -d plsdb.fna.bz2
```

Simplify the fasta headers by removing everything after the first space:
```
awk -F '[ ]' '{print $1}' plsdb.fna > plsdb2.fna
```

Create a BLAST database that allows queries based on accession ID:
```
makeblastdb -dbtype nucl -hash_index -parse_seqids -in plsdb2.fna -out plsdb
```

Once you have confirmed the database has been created, remove sequence files to free up space:
```
rm plsdb.fna
rm plsdb2.fna
```

## Metadata Files

Finally, both the GFF files and the reference files require a plasmid-type metadata file.
The metadata file is a binary matrix organized as:
- plasmid type a row headers
- sequence ID as the column header
- row/col intersection as "1" when the plasmid ID belongs to the corresponding row plasmid type
- row/col intersection as "0" when the plasmid ID does not belong to the corresponding row plasmid type

metadata files corresponding to the sample data are found here:
- GFF annotation metadata file:  combinedplasmid.csv
- PLSDB v2021_06_23 plasmid metadata file: plsdb.names.bz2

Perform the following to decompress the plasmid-type metadata file for the reference database to create the plsdb.names metadata file:
```
bunzip2 plsdb.names.bz2
```

This should complete the sample dataset retrieval and preparation step.

Please remember to return to the Lociq parent directory.
