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

- VCF files were then converted to PLINK bfile (PLINK v1.9)
.. code-block:: console

   ./plink --vcf chr*_splitmultiallelicsnps.vcf.gz --out chr
    21_splitmultiallelicsnps

- Apply filtering: 
    - Filtering criteria: 
        - SNPs with tags `<CNV>` and `<INS>` are removed
        - Unique ID is defined as `chr:pos:allele1:allele2` where alleles are alphabetically sorted
        - Duplicated SNPs (defined as identical unique ID) are removed
    - Steps: 
        - Modify the output `.bim` files so that it contains the unique ID in order to use the flag `--extract`
        .. code-block:: console

            mv chr*_splitmultiallelicsnps.bim chr*_splitmultiallelicsnps.bim.orig

            python add_uniqueID_bim.py -b chr*_splitmultiallelicsnps.bim.orig -o chr*_splitmultiallelicsnps.bim

            python create_include_snps.py --bim chr*_splitmultiallelicsnps.bim.orig --out chr*_kept_snps.txt

            ./plink --bfile chr*_splitmultiallelicsnps --extract chr*_kept_snps.txt --maf 0.00000000001 --out chr*_splitmultiallelicsnps_filtered --make-bed
        
        - **NOTES**: instead of using a very low value for maf threshold, we could compute frequency using plink and then update the file `chr*_kept_snps.txt`.

        - #TODO: 
            - add links to scripts
            - update frequency filtering

- Create the `{pop}.chr*.rsID.gz`
#TODO

- Create the `{pop}.chr*.frq.gz`
.. code-block:: console

   ./plink -bfile chr*_splitmultiallelicsnps_filtered --freq --out chr*_splitmultiallelicsnps_filtered_maf

- Create the `{pop}.chr*.ld.gz`
.. code-block:: console

   ./plink -bfile chr*_splitmultiallelicsnps_filtered --r2 --ld-window 99999 --ld-window-r2 0.05 --out chr*_splitmultiallelicsnps_ld




dbSNPs
------------

- FUMA version 1.7.0 uses dbSNPs version 146