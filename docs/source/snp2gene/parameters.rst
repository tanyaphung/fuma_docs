Parameters
==========

Annotation and prioritization depends on several settings, which can be adjusted if desired. The default settings will result in performing naive positional mapping which maps all independent lead SNPs and SNPs in LD to genes up to 10kb apart. It does not include eQTL mapping by default, and it also does not filter on specific functional consequences of SNPs. If for example you are interested in prioritizing genes only when they are indicated by an eQTL that is in LD with a significant lead SNP, or by exonic SNPs, then you need to adjust the parameter settings.

In this section, every parameter that can be adjusted will be described in detail.

1. Input files
++++++++++++++

.. list-table:: 
   :widths: 20 20 40 10 10
   :header-rows: 1

   * - Parameter
     - Mandatory
     - Description
     - Type
     - Default
   * - GWAS summary statistics
     - Mandatory
     - Input file of GWAS summary statistics. Plain text file or zipped or gzipped files are acceptable. The maximum file size which can be uploaded is 600Mb. In addition to full results of GWAS summary statistics, subset of results can also be used. e.g. If you would like to look up specific SNPs, you can filter out other SNPs. Please refer to the :ref:`gwas_sumstat` section for specific file format.
     - File upload
     - none
   * - Pre-defined lead SNPs
     - Optional 
     - Optional pre-defined lead SNPs. The file should have 3 columns, rsID, chromosome and position.
     - File upload
     - none
   * - Identify additional lead SNPs
     - Optional only when predefined lead SNPs are provided
     - If this option is CHECKED, FUMA will identify additional independent lead SNPs after defining the LD block for pre-defined lead SNPs. Otherwise, only given lead SNPs and SNPs in LD of them will be used for further annotations.
     - Check
     - Checked
   * - Pre-defined genetic region
     - Optional
     - Optional pre-defined genomic regions.
       FUMA only looks at provided regions to identify lead SNPs and SNPs in LD of them. If you are only interested in specific regions, this option will increase the speed of process.
     - File upload
     - none

2. Parameters for lead SNPs and candidate SNPs identification
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. list-table:: 
   :widths: 15 10 30 15 10 20
   :header-rows: 1

   * - Parameter
     - Mandatory
     - Description
     - Type
     - Default
     - Direction
   * - Sample size (N)
     - Mandatory
     - The total number of individuals in the GWAS or the number of individuals per SNP. This is only used for MAGMA to compute the gene-based P-values. For total sample size, input should be an integer. When the input file of GWAS summary statistics contains a column of sample size per SNP, the column name can be provided in the second text box.
       When column name is provided, please make sure that the column only contains integers (no float or scientific notation). If there are any float values, they will be rounded up by FUMA.
     - Integer or text
     - none
     - Does not affect any candidates
   * - Maximum lead SNP P-value (<)
     - Mandatory
     - FUMA identifies lead SNPs with P-value less than or equal to this threshold and independent from each other.
     - numeric
     - 5e-8
     - lower: decrease #lead SNPs
       higher: increase #lead SNPs
   * - Maximum GWAS P-value (<)
     - Mandatory
     - This is the P-value threshold for candidate SNPs in LD of independent significant SNPs. This will be applied only for GWAS-tagged SNPs as SNPs which do not exist in the GWAS input but are extracted from 1000 genomes reference do not have P-value.
     - numeric
     - 0.05
     - higher: increase #candidate SNPs
       lower: decrease #candidate SNPs
   * - r:sup:`2` threshold for independent significant SNPs (≥)
     - Mandatory
     - The minimum r:sup:`2` for defining independent significant SNPs, which is used to determine the borders of the genomic risk loci. SNPs with r:sup:`2` ≥ user defined threshold with any of the detected independent significant SNPs will be included for further annotations and are used fro gene prioritisation.
     - numeric
     - 0.6
     - higher: decrease #candidate SNPs and increase #independent significant SNPs
       lower: increase #candidate SNPs and decrease #independent significant SNPs
   * - 2nd r:sup:`2` threshold for lead SNPs (≥)
     - Mandatory
     - The minimum r:sup:`2` for defining lead SNPs, which is used for the second clumping (clumping of the independent significant SNPs). Note that when this threshold is same as the first r:sup:`2` threshold, lead SNPs are identical to independent significant SNPs.
     - numeric
     - 0.1
     - higher: increase #lead SNPs
       lower: decrease #lead SNPs
   * - Reference panel
     - Mandatory
     - The reference panel to compute r2 and MAF. Five populations from 1000 genomes Phase 3 and 3 versions of UK Biobank are available. See :ref:`ukb_ref` for details.
     - Select
     - 1000G Phase EUR
     - 
   * - Include variants from reference panel
     - Mandatory
     - If Yes, all SNPs in strong LD with any of independent significant SNPs including non-GWAS-tagged SNPs will be included and used for gene mapping.
     - Yes/No
     - Yes
     - 
   * - Minimum MAF (≥)
     - Mandatory
     - The minimum Minor Allele Frequency to be included in annotation and prioritisation. MAF is based the user selected reference panel. This filter also applies to lead SNPs. If there is any pre-defined lead SNPs with MAF less than this threshold, those SNPs will be skipped. When this value is 0 (by default), SNPs with MAF>0 are considered.
     - numeric
     - 0
     - higher: decrease #candidate SNPs
       lower: increase #candidate SNPs
   * - Maximum distance of LD blocks to merge (≤)
     - Mandatory
     - This is the maximum distance between LD blocks of independent significant SNPs to merge into a single genomic locus. When this is set at 0, only physically overlapping LD blocks are merged. Defining genomic loci does not affect identifying which SNPs fulfil selection criteria to be used for annotation and prioritization. It will only result in a different number of reported risk loci, which can be desired when certain loci are partly overlapping or physically very close.
     - numeric
     - 250kb
     - higher: decrease #genomic loci
       lower: increase #genomic loci