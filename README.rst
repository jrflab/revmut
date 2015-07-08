REVertant MUTation finder (REVMUT)
==================================

You have (a) some transcript mutations in HGVS format that change the reading
frame. You want to see if (b) some other transcript mutations in HGVS format
revert the mutations in (a).  This script applies the mutations in (a) followed
by the mutations in (b) and shows how the resulting protein changes in length. 

Current workflow of finding revertant mutations is:

1. Find putative revertant mutations (SNVs at the same location as original
   SNV, indels that restore the reading frame)
2. Run those mutations through Oncotator to get transcript change of putative
   revertant mutations in HGVS format 
3. Check if one of the transcript change in HGVS format is revertant by looking
   how the length of the protein changes

This script helps you to do 3. Steps 1 and 2 might be added at a later stage in
development.

Installation
------------
::

    pip install revmut

Run
---