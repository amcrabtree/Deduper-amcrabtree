# Deduper

This script will be able to remove reads in SAM file which are PCR duplicates. In the SAM file, these are defined as reads with:

* same chromosome ("RNAME")
* same position ("POS"), corrected for soft clipping ("S" in CIGAR string)
* same strand (encoded in "FLAG", bit 16)
* same unique molecular index; "UMI" or "randomer" ("QNAME")

### Scripts

`amcrabtree_deduper.py` - main script for removing PCR duplicates

`Bioinfo.py` - python module providing required functions for main script

### Reports

`Deduper_report.Rmd` - R Markdown from which the summary report is generated

`Deduper_report.pdf` - Summary report of script performance

### Test Files

Located in `./data`

`STL96.txt` - file containing list of UMIs found in `test_soft.sam`

`counts.csv` - file containing information for chromosome counts, resulting from large test file (not test_soft.sam)

`test_soft.sam` - small SAM-formatted file containing a small list of reads to use for testing

### Images

Located in `./img`

Contains images used in the R markdown report, generated from FastQC. 

### Archived files

Located in `./archive`

Contains pseudocode and other documents relevant earlier in the script-writing process