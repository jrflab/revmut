.. image:: https://zenodo.org/badge/14279/inodb/revmut.svg
   :target: https://zenodo.org/badge/latestdoi/14279/inodb/revmut
.. image:: https://travis-ci.org/inodb/revmut.svg?branch=master 
  :target: https://travis-ci.org/inodb/revmut
REVertant MUTation find & verify (REVMUT)
=========================================
REVMUT can help to **find** and **verify** putative revertant mutations (PRMs) in DNA sequence data. Common workflow is:

1. **Find** PRMs (deletions of given mutation, indels that restore the reading frame)
2. **Annotatote** PRMs with Oncotator to get transcript change of putative
   revertant mutations in HGVS format 
3. **Verify** if one of the transcript change in HGVS format is revertant by looking
   how the length of the protein changes

There are scripts to do 1 and 3. Steps 2 might be added at a later stage in
development.

.. image:: img/revmut_overview.png

Installation
------------
::

    pip install revmut

Find
----
The finding module takes a mutation and finds
PRMs that:

- Delete the entire given mutation
- Restore the reading frame in case the given mutation (GM) is an indel. The criterium is::
  
    length(PRM) +/- length(GM) % 3 == 0
  
Run with::

  revmut-find tests/test_data/human_g1k_v37_chr17.fa \
              tests/test_data/germline_mutations/T1_test_mutation.tsv \
              tests/test_data/T1.bam \
              tests/test_data/N1.bam > tests/test_data/output/T1_test.tsv
  
View input/output files:

- `tests/test_data/germline_mutations/T1_test_mutation.tsv <tests/test_data/germline_mutations/T1_test_mutation.tsv>`_
- `tests/test_data/output/T1_test.tsv <tests/test_data/output/T1_test.tsv>`_


Annotate
--------
Annotation of the PRMs is currently done semi-manually with `Oncotator webserice <http://www.broadinstitute.org/oncotator/>`_. Perhaps at a later stage in development this will be done automatically. Missing is a VCF to Oncotator format converter.

Verify
------
Applies a given mutation in cDNA format to a transcript followed by the cDNA change of the PRM as predicted by Oncotator. Output gives a prediction of how the protein changes.

Run with::

  revmut-verify tests/test_data/to_be_reverted_mutations.txt \
                tests/test_data/oncotator.ins.txt \
                tests/test_data/BRCA_transcripts.fa > tests/test_data/oncotator.ins.maf.out.tsv
  
View input/output files:
  
- `tests/test_data/to_be_reverted_mutations.txt <tests/test_data/to_be_reverted_mutations.txt>`_
- `tests/test_data/oncotator.ins.txt <tests/test_data/oncotator.ins.txt>`_
- `tests/test_data/output/oncotator.ins.maf.out.tsv <tests/test_data/output/oncotator.ins.maf.out.tsv>`_

For an example that includes only reversion mutations try these BRCA1 mutations
from the paper  **Diverse BRCA1 and BRCA2 Reversion Mutations in Circulating
Cell-Free DNA of Therapy-Resistant Breast or Ovarian Cancer.**::

  revmut-verify tests/test_data/paper/brca1/to_be_reverted_mutations.txt \
                tests/test_data/paper/brca1/oncotator.ins.txt \
                tests/test_data/BRCA_transcripts.fa > tests/test_data/paper/brca1/output/oncotator.ins.maf.out.tsv

View input/output files:
  
- `tests/test_data/paper/brca1/to_be_reverted_mutations.txt <tests/test_data/paper/brca1/to_be_reverted_mutations.txt>`_
- `tests/test_data/paper/brca1/oncotator.ins.txt <tests/test_data/paper/brca1/oncotator.ins.txt>`_
- `tests/test_data/paper/brca1/output/oncotator.ins.maf.out.tsv <tests/test_data/paper/brca1/output/oncotator.ins.maf.out.tsv>`_

A description of the columns in the output format::

	mut = germline mutation
	revmut = potential revertant mutation
	revmut_pos_adj = the adjusted revertant mutations position after the germline mutation has been applied
	transcript = the transcript to apply the cdna change to
	normal_protein_length = the length of the protein when you translate the transcript in amino acids
	mut_protein_length = how long the protein is after applying the germline mut cdna change and translating
	revmut_protein_length = how long the protein is after applying the germline mut cdna change followed by the potential revertant mutation cdna change and translating
	mut_p_distance = number of amino acids that differ between original protein and germline mutated protein
	revmut_p_distance = number of amino acids that differ between original protein and germline + revertant mutated protein 


Developers
----------
Tests
~~~~~
In root dir run::

    nosetests
