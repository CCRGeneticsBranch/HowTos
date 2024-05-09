#Formatting bed files for khanlab exome-rnaseq pipeline.

The Target bed files you receive from capture kits usually arrive in different formats. Here are the steps to convert them to a pipeline accepted format. This bed file will be used to calculate Readdepth, coverage and failedexons in the pipeline.

##1. Filter GTF file to extract bedfile of exons.
1. Pipeline uses gtf annotation [gencodev37](ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_37/GRCh37_mapping/gencode.v37lift37.annotation.gtf.gz).
2. Filter to get exons ie., by col 3.
3. Convert to bed format (columns 1-3) plus format strand (replace +/- with f/r), extract transcript ID, gene name, and exon number from annotations, and add header. here is an example output

| chr | start | stop | strand | transcript_ID | Genename | exon_number |
|---|---|---|---|---|---|---|
| chr1 | 11869 | 12227 | f | ENST00000456328.2_1 | DDX11L1 | 1 |
| chr1 | 12613 | 12721 | f | ENST00000456328.2_1 | DDX11L1 | 2 |
| chr1 | 13221 | 14409 | f | ENST00000456328.2_1 | DDX11L1 | 3 |
| chr1 | 12010 | 12057 | f | ENST00000450305.2_1 | DDX11L1 | 1 |
| chr1 | 12179 | 12227 | f | ENST00000450305.2_1 | DDX11L1 | 2 |
| chr1 | 12613 | 12697 | f | ENST00000450305.2_1 | DDX11L1 | 3 |
| chr1 | 12975 | 13052 | f | ENST00000450305.2_1 | DDX11L1 | 4 |


##2. Downlaod HGNC Biomart table - To identify protein-coding genes

1. Download from [HGNC](https://biomart.genenames.org/martform/#!/default/HGNC?datasets=hgnc_gene_mart&attributes=hgnc_gene__hgnc_gene_id_1010%2Chgnc_gene__status_1010%2Chgnc_gene__approved_symbol_1010%2Chgnc_gene__approved_name_1010%2Chgnc_gene__ensembl_gene__ensembl_gene_id_104)table with HGNC ID, status, approved symbol, approved name and Ensembl gene ID
2. Filter to protein-coding transcripts ("NM_") and reduce to columns 8-9 (transcript_ID,RefSeq transcript ID).
Here is an example output.

| transcript_ID | RefSeq transcript ID |
|---|---|
| ENST00000263100.8 | NM_130786.4 |
| ENST00000373997.8 | NM_014576.4 |
| ENST00000318602.12 | NM_000014.6 |
| ENST00000299698.12 | NM_144670.6 |
| ENST00000442999.3 | NM_001080438.1 |

##3. Filter outputs from above two steps by merging on transcript ID

```
#!/usr/bin/env python

import pandas as pd

df1 = pd.read_csv("Filtered_GTF_file_with_exons",sep='\t')
df1['transcript_ID'] = df1['transcript_ID'].str.split('.').str[0]
df1['exon_number'] = df1['exon_number'].round(decimals=0).astype(object)

df2 = pd.read_csv("Filtered_hgnc_biomart_table-transcript-NM",sep='\t')
df2['transcript_ID'] = df2['transcript_ID'].str.split('.').str[0]
df3 = pd.merge(df1,df2, on="transcript_ID",how="inner")
df3['exon_number'] = df3['exon_number'].astype(str)
df3['RefSeq transcript ID'] = df3['RefSeq transcript ID'].str.split('.').str[0]
df3['NM_ID_strand'] = df3['RefSeq transcript ID'] + "_cds_" + df3['strand']

df5 = df3[['chr','start','stop','Genename','exon_number','NM_ID_strand']]
df5['exon_number'] = df5['exon_number'].str.split('.').str[0]
df5.to_csv("gencode-exon-hgnc-biomart-table-khanlab-bed.csv",index=False,encoding='utf-8', sep ='\t')

```

##4. Filter Capture kit target intervals using the output generated in step 3

Extract columns 1-3 from the capture kit bed file.

```
bedtools intersect -a capture_kit_bed_file /
                   -b gencode-exon-hgnc-biomart-table-khanlab-bed.csv /
                   -wa -wb | cut -f1,2,3,7,8,9| bedtools sort |uniq| /
                   -mergeBed -i - -c 4,5,6 -o distinct,collapse,collapse | /
                   awk '{OFS="\t"}{print $1,$2,$3,$4"___"$5,$6}' > khanlab_pipeline_accepted_bed file.
```


This page was created through contributions from [Erica Pehrsson](mailto:erica.pehrsson@nih.gov) and [Vineela Gangalapudi](mailto:vineela.gangalapudi@nih.gov). 