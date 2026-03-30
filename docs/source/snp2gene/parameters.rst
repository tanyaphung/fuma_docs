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