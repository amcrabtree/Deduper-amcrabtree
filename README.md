# Deduper

This script will be able to remove reads in SAM file which are PCR duplicates. In the SAM file, these are defined as reads with:

* same chromosome ("RNAME")
* same position ("POS"), corrected for soft clipping ("S" in CIGAR string)
* same strand (encoded in "FLAG", bit 16)
* same unique molecular index; "UMI" or "randomer" ("QNAME")

### Files

`amcrabtree_deduper.py` - main script for removing PCR duplicates

`Bioinfo.py` - python module providing required functions for main script

### Reports

`Deduper_report.Rmd` - R Markdown from which the summary report is generated

`Deduper_report.pdf` - Summary report of script performance