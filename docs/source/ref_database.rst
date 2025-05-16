Generating reference database
=====

.. _dbSNPs:

Reference panel
---------------
To define independent significant SNPs, lead SNPs, and genomic risk loci, FUMA uses reference panels. 
1. 1000 Genome Phase 3
- Genotype data for chromosome 1-22 and X were downloaded from https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ using wget. Example:
.. code-block:: console

   wget https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr*.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz

- Multi-allelic SNPs were first split into separate columns using `vcfmulti2oneallele` from JVARKIT (http://lindenb.github.io/jvarkit/)
    - Download the pre-compiled jar archive for JVARKIT from: https://uncloud.univ-nantes.fr/index.php/s/4sL77oWR2BFzSBH
    - Command to split Multi-allelic SNPs: 
    .. code-block:: console

        java -jar jvarkit.jar vcfmulti2oneallele ALL.chr*.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz > chr*_splitmultiallelicsnps.vcf.gz





dbSNPs
------------

- FUMA version 1.7.0 uses dbSNPs version 146