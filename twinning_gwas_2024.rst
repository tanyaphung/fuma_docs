Practicals on Twinning GWAS 2024
================================

Overview
--------
In this practicals, we are going to analyze the GWAS from Twinning 2024 to showcase how to use the new modules from FUMA v2.0.0

Download and Format  GWAS
-------------------------

- Download the GWAS summary statistics from https://www.ebi.ac.uk/gwas/studies/GCST90244755
- The downloaded file is: `GCST90244755_buildGRCh37.tsv`

.. tip:: 
    ALWAYS inspect the GWAS summary statistics file and format it before submitting to FUMA

1. Check which build, GRCh37 or GRCh38

- Spot check a few variants on gnomad, the chromosome and position matches with GRCh37. For example: 
    - https://gnomad.broadinstitute.org/variant/10-10000018-A-G?dataset=gnomad_r2_1
    - https://gnomad.broadinstitute.org/variant/10-100003785-T-C?dataset=gnomad_r2_1

2. Check other columns

- p value is found under column name `p_value`

- beta is found under column name `beta`

- Sample sizes: In the paper it says: `We conducted a genome-wide association meta-analysis (GWAMA) of mothers of spontaneous dizygotic (DZ) twins (8265 cases, 264 567 controls) and of independent DZ twin offspring (26 252 cases, 417 433 controls).`
    - So we can do 8265+264567+26252+417433 = 716517. We will use this value for the sample size.

3. Submit a SNP2GENE job
- Apriori I know that the Twinning 2024 GWAS has only a few genomic risk loci. Therefore, I will submit a SNP2GENE job that does both gene mapping and MAGMA. 

- Example of section 1 upload input files
.. image:: twinning_2024_upload_input.png
    :width: 800

- Make sure to put in an integer value for the sample size
.. image:: twinning_2024_samplesize.png
    :width: 800

- Gene mapping: select positional mapping and xQTLs mapping from these 8 tissues: whole blood, breast, ovary, pituitary, testis, uterus, hypothalamus, and vagina. 
    - select eQTLs and sQTLs and all of pQTLs

- Include the MHC region
.. image:: twinning_2024_mhcinclude.png
    :width: 800

- Make sure to check MAGMA in section 6
.. image:: twinning_2024_magmainclude.png
    :width: 800

- Then click on submit. 