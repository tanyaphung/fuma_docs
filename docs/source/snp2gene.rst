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
- Otherwise, the input GWAS sumstat has to have these columns (more information in the :ref:`mandatory_columns` section):
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


.. _mandatory_columns:
Mandatory columns
^^^^^^^^^^^^^^^^^
- The input GWAS sumstat must include:
   - a column for the p values, a column for the chromosome, and a column for the position OR
   - a column for the p values and a column for the rsID
- The chromosome column can be a string such as "chr1" or just an integer such as "1". When "chr" is attached, this will be removed in output files. 
- Chromosome X can be encoded as chrX, X, chr23, or 23
   - when chrX or X is used, it will be updated to 23.
- Position has to be integer (and not in scientific notation)

Allele columns
^^^^^^^^^^^^^^
- Alleles are not mandatory but if only one allele is provided, that is considered to be the effect allele. 
- When two alleles are provided, the effect allele will be defined depending on column name. 
- If alleles are not provided, they will be extracted from the dbSNP build 146 and minor alleles will be assumed to be the effect alleles. 
- Effect and non-effect alleles are not distinguished during annotations, but used for alignment with eQTLs. 
- Whenever alleles are provided, they are matched with dbSNP build 146 if extraction of rsID, chromosome or position is necessary.
- Alleles are case insensitive.

Headers
^^^^^^^
- A header is mandatory
- Users have an option to specify the column names of the input GWAS sumstat: 

.. image:: images/user_input_colnames.png
   :width: 400


- When users do not specify the column names of the input GWAS sumstat, FUMA automatically detects the columns based on the following headers (case insensitive):
   - If a column has names **snp**, **snpid**, or **markername**, it will be detected as column for rsID
   - If a column has names **chr**, **chromosome**, or **chrom**, it will be detected as column for chromosome
   - If a column has names **bp**, **pos**, or **position**, it will be detected as column for position (basepair)
   - If a column has names **A1**, **effect_allele**, **allele1**, or **alleleB**: it will be detected as the effect allele 
   - If a column has names **A2**, **non_effect_allele**, **allele2**, or **alleleA**: it will be detected as non-effect-allele
   - If a column has names **P**, **pvalue**, **p-value**, **p_value**, or **pval**: it will be detected as p values
   - If a column has names **OR**, it will be detected as odds ratio
   - If a column has names **Beta** or **be**, it will be detected as beta
   - If a column has names **SE**, it will be detected as standard errors

- Note that any columns with the name listed above but with different element need to be avoided. For example, when the column name is "SNP" but the actual element is an id such as "chr:position" rather than rsID will cause an error.
- Extra columns will be ignored.
- Rows that start with "#" will be ignored.
- Column "N" is described in the :ref:`parameters` section.
- Be careful with the alleles header in which A1 is defined as effect allele by default. Please specify both effect and non-effect allele column to avoid mislabeling.
   - If wrong labels are provided for alleles, it does not affect any annotation and prioritization results. It does however affect eQTLs results (alignment of risk increasing allele of GWAS and tested allele of eQTLs). Be aware of that when you interpret results.

Delimiter
^^^^^^^^^
Delimiter can be any of white space including single space, multiple space and tab. Because of this, each element including column names must not include any space.

Tips
^^^^
1. Make sure that there is a header (column name) in the input GWAS sumstat.

   - The header should not start with a comment character (#) because any lines that starts with # will be ignored.
   - The number of column names need to be equal to the number of columns in your input file.

      - if the input file has a row index, this means that there is one fewer column name in the header as compared to when the actual data starts.
      - to fix this, one needs to save the data specifying `index=False` (the syntax depends on the specific language)
2. rsID if exists has to be in rsID format and not chr:pos:a1:a2 format
3. Use gzip software to compress with .gz extension or ZIP software with .zip extension. Make sure you haven't renamed the file manually. Use the proper compression software instead.
4. The chromosome has to be numbers between 1 and 23 or X. Any variant with a chromosome name that deviates from 1 to 23 or X may cause failure. 
5. Position values have to be integer (not in scientific notation)
6. If your file contains chromosome and position, these have to be in hg19 coordinates.
7. Make sure that there is no missing data for the columns that are mandatory such as p-values
8. If you specify the name of chromosome, position, etc… during submission, make sure that these names exist in your input file
9. Make sure that the delimiter is consistent. In addition, Delimiter can be any of white space including single space, multiple space and tab. Because of this, each element including column names must not include any space
10. Check your file to make sure that there is no quotation around each value. It should be for example 1 instead of "1". This is usually caused when you save a file in R. To avoid this, one needs to set `quote=F` when saving a file in R. 

2. Pre-defined lead SNPs
++++++++++++++++++++++++
This is an optional input file.
This option would be useful when
1. You have lead SNPs of interest but they do not reach significant P-value threshold.
2. You are only interested in specific lead SNPs and do not want to identify additional lead SNPs which are independent. In this case, you also have to UNCHECK option of Identify additional independent lead SNPs.

If you want to specify lead SNPs, input file should have the following 3 columns:
   - rsID : rsID of the lead SNPs
   - chr : chromosome
   - pos : genomic position (hg19)

.. note::
      The order of columns has to be exactly the same as shown above but header could be anything (the first row is ignored). Extra columns will be ignored.

3. Pre-defined genomic region
+++++++++++++++++++++++++++++
This is an optional input file. This option would be useful when you have already done some follow-up analyses of your GWAS and are interested in specific genomic regions. When pre-defined genomic region is provided, regardless of parameters, only lead SNPs and SNPs in LD with them within provided regions will be reported in outputs.

If you want to analyze only specific genomic regions, the input file should have the following 3 columns:
   - chr : chromosome
   - start : start position of the genomic region of interest (hg19)
   - end : end position of the genomic region of interest (hg19)

.. note::
      The order of columns has to be exactly the same as shown above but header could be anything (the first row is ignored). Extra columns will be ignored.

.. _parameters:
Parameters
----------
Annotation and prioritization depends on several settings, which can be adjusted if desired. The default settings will result in performing naive positional mapping which maps all independent lead SNPs and SNPs in LD to genes up to 10kb apart. It does not include eQTL mapping by default, and it also does not filter on specific functional consequences of SNPs. If for example you are interested in prioritizing genes only when they are indicated by an eQTL that is in LD with a significant lead SNP, or by exonic SNPs, then you need to adjust the parameter settings.

