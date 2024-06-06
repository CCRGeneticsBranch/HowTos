#Overview

[GDC](https://portal.gdc.cancer.gov/) suggests adding viral sequences and decoys to human genome when building a reference. There are about 200 viral sequences listed [here](https://gdc.cancer.gov/system/files/public/file/GRCh83.d1.vd1_virus_decoy.txt). Downloading these fasta sequences one by one can be quite tedious and prone to errors. 

##Download steps

We put together a python script that will download all the viral sequences to a single fasta file.

1. Copy all the lines from [here](https://gdc.cancer.gov/system/files/public/file/GRCh83.d1.vd1_virus_decoy.txt) and save it to a text file.(GRCh83.d1.vd1_virus_decoy.txt)
2. Make sure biopython library is installed in your env. If using biowulf, you can do this in a conda env
```
pip3 install biopython
```
3. Before launching the python script, please add your email id for `Entrez.email`. 

This script takes one input.

```
Usage: gdc_download_viral_seqs.py <virus_decoys_file>
```

This script filters the virus decoy file to extract the accession ids for each viral sequence. Using `Entrez` module these ids are queried to download the fasta sequences that are all saved to the output file `viral_sequences.fasta`.

```
#!/usr/bin/env python3

import csv
import sys
from Bio import Entrez
from Bio import SeqIO

if len(sys.argv) != 2:
    print("Usage: gdc_download_viral_seqs.py <input_file>")
    sys.exit(1)

input_file = sys.argv[1]

with open(input_file, 'r', newline='') as file:
    reader = csv.reader(file, delimiter='\t')
    cleaned_data = [row for row in reader if any(row)]

accession_ids = []

if len(cleaned_data) > 1:
    for row in cleaned_data[1:]:
        if len(row) > 2:
            value = row[2] if row[2] else (row[3] if len(row) > 3 else '')
            accession_ids.append(value)
else:
    print("The file is empty or only contains empty lines.")

# Please add your email id here
Entrez.email = "vineela.gangalapudi@nih.gov"

with open("viral_sequences.fasta", "w") as output_file:
    # Iterate over each accession ID
    for accession_id in accession_ids:
        # Fetch the sequence from NCBI
        handle = Entrez.efetch(db="nucleotide", id=accession_id, rettype="fasta", retmode="text")
        fasta_sequence = handle.read()
        handle.close()

       	if fasta_sequence.strip():
            # Write the fasta sequence to the output file
            output_file.write(fasta_sequence.strip() + "\n")
        else:
            print(f"Sequence not found for accession ID: {accession_id}")

```

If you have any questions or would like to provide valuable additions to this page please email [Vineela Gangalapudi](mailto:vineela.gangalapudi@nih.gov).