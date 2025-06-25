SNP2GENE
========

.. _prepare_input_file:

Prepare Input Files
-------------------

1. GWAS summary statistics
++++++++++++++++++++++++++

Overview
^^^^^^^^

- GWAS summary statistics (GWAS sumstat) is a mandatory input of SNP2GENE module. 
- FUMA accepts various types of format. For example, PLINK, SNPTEST, and METAL output formats can be used as is. 
- Otherwise, the input GWAS sumstat has to have these columns (more information in the `Mandatory columns` section):
   - chromosome, position (in hg19), and p values OR
   - rsID and p values
- Every row should contain information for one SNP. 
- An input GWAS sumstat could contain only subset of SNPs (e.g. SNPs of interest for your study to annotate them), but in this case, results of MAGMA will not be relevant anymore.
- Please note that variants that do not exist in the selected reference panel will not be included in any analyses. The 1000G reference panel is provided in the Download page (scroll to the section Reference panel data).
- Prepare input GWAS sumstat in ascii txt.  
- Before submitting, gzip your file: 

.. code-block:: console
   gzip {gwas/sum/stat}

- Below are some more specific information to help with preparing the input GWAS sumstat

Genome Build
^^^^^^^^^^^^
The reference data used by FUMA is on build GRCh37 (hg19).
If your data is build GRCh37, you can upload your file.
If your data is build GRCh38, you can: 
   - use UCSC liftover tool to lift over from GRCh38 to GRCh37
   - if your input GWAS sumstat file contains rsID, you can still submit to FUMA by first make sure that chromosome and position columns are not present. 
      - in this case, FUMA will use the provided rsID to look up the chromosomes and positions using dbSNP




Parameters
----------
Annotation and prioritization depends on several settings, which can be adjusted if desired. The default settings will result in performing naive positional mapping which maps all independent lead SNPs and SNPs in LD to genes up to 10kb apart. It does not include eQTL mapping by default, and it also does not filter on specific functional consequences of SNPs. If for example you are interested in prioritizing genes only when they are indicated by an eQTL that is in LD with a significant lead SNP, or by exonic SNPs, then you need to adjust the parameter settings.

