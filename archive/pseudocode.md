## Defining the Problem

Remove reads in SAM file which are PCR duplicates. In the SAM file, these are defined as reads with:

* same chromosome ("RNAME")
* same position ("POS"), corrected for soft clipping ("S" in CIGAR string)
* same strand (encoded in "FLAG", bit 16)
* same unique molecular index; "UMI" or "randomer" ("QNAME")

## Examples

* `test_soft.sam` - Input SAM file of uniquely mapped reads
* `output.sam` - Expected output SAM file

## High-level Functions

	def dice_head (sam_head: str -> list
		'''converts SAM header to list of components needed for analysis'''
		<insert code here>
		return [UMI, FLAG, RNAME, POS, CIGAR, unmapped, revc]
		
		test example: 
			
			sam_head = "NS500451:154:HWKTMBGXX:1:11101:24260:1121:CTGTTCAC	20	2	76814284	36	71M	*	0	0	TCCACCACAATCTTACCATCCTTCCTCCAGACCACATCGCGTTCTTTGTTCAACTCACAGCTCAAGTACAA	6AEEEEEEAEEAEEEEAAEEEEEEEEEAEEAEEAAEE<EEEEEEEEEAEEEEEEEAAEEAAAEAEEAEAE/	MD:Z:71	NH:i:1	HI:i:1	NM:i:0	SM:i:36	XQ:i:40	X2:i:0	XO:Z:UU"
			
			output: ["CTGTTCAC", 0, 2, 76814284, "71M", 1, 1]

	def to_tru_pos (soft_pos: int, cigar: str, top_strand: bool) -> int:
		'''Converts position with soft clipping (from CIGAR string) to true position'''
		<insert code here>
		return real_pos

		test example:

			soft_pos = 100
			cigar = "11S60M"
			top_strand = 1

			output: 89

	def to_cush_pos (real_pos: int, cigar: str, top_strand: bool) -> int:
		'''converts true position to position with soft clipping (from CIGAR string)'''
		<insert code here>
		return soft_pos

		test example: 

			real_pos = 89
			cigar = "11S60M"
			top_strand = 1

			output: 100

## Algorithm

1. Write intermediary file with POS substitued with real position generated from `tru_pos` function

2. Run file through samtools sort to get a file with the reads ordered from 5' to 3' of reference genome from top to bottom in SAM file

1. Remove unmapped reads, so we could take raw file input if we wanted to (col 2 "FLAG", bit 4)

2. store UMIs (from file) in a set

For each position in sorted SAM file: (run line counter and nucleotide position counter)

3. store tuple of (top_strand_line, bottom_strand_line)

4. run `dice_head` 

5. if real_pos == previous_pos, check strandedness and UMI. if UMI does not match any in set, discard read. if UMI is same as either top_strand_line or bottom_strand_line, chuck read. else, store line. 

6. if real_pos != previous_pos, write top_strand_line and bottom_strand_line to output file

## Test files

input: `/data/test_soft.sam`

output `/data/test_soft_output.sam`
