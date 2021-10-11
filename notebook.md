Deduper-amcrabtree
==================

Discovered test file (`test.sam`) does not have soft clipping in CIGAR strings, so I need to add those for test purposes. 

	cat data/test.sam | awk '{print $6}' | grep 'S'


