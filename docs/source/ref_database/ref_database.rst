Generating FUMA database
=====

A number of pre-processed files are required to run FUMA. In this section, we will describe how to generate these data. 

.. _dbSNPs:

Reference panel
---------------
To define independent significant SNPs, lead SNPs, and genomic risk loci, FUMA uses reference panels. 

1. 1000 Genome Phase 3
++++++++++++++++++++++

- Genotype data for chromosome 1-22 and X were downloaded from https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ using wget. Example:
.. code-block:: console

   wget https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr*.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz


- Multi-allelic SNPs were first split into separate columns using vcfmulti2oneallele from JVARKIT (http://lindenb.github.io/jvarkit/)
    
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


- Create the {pop}.chr*.rsID.gz

    - #TODO


- Create the {pop}.chr*.frq.gz
.. code-block:: console

   ./plink -bfile chr*_splitmultiallelicsnps_filtered --freq --out chr*_splitmultiallelicsnps_filtered_maf


- Create the {pop}.chr*.ld.gz
.. code-block:: console

   ./plink -bfile chr*_splitmultiallelicsnps_filtered --r2 --ld-window 99999 --ld-window-r2 0.05 --out chr*_splitmultiallelicsnps_ld


.. note::
    Reference panel **ALL** contains the most number of SNPs. To avoid missing SNPs from FUMA annotations, reference panel **ALL** might be preferred. However, the LD is not population specific and need caution for the definition of independent significant SNPs and lead SNPs.


- Download links
.. list-table:: Number of samples and SNPs in the reference panels
   :widths: 25 25 25 25
   :header-rows: 1

   * - Population
     - Sample size
     - Number of SNPs
     - Download link
   * - ALL
     - 2,504
     - 84,853,668
     - 
   * - AFR
     - 661 
     - 84,853,668
     - 
   * - AMR
     - 347
     - 29,501,504
     - 
   * - EAS
     - 504
     - 24,507,348
     - 
   * - EUR
     - 503 
     - 25,063,419
     - 
   * - SAS
     - 489
     - 27,691,316
     -      

2. UK Biobank release 2b
++++++++++++++++++++++++

Genotype data was obtained under application ID 16406. The reference panel is based on genotype data released in May 2018 (including SNPs imputed UK10K/1000G). Two reference panels were created; white British and European subjects. For white British, 10,000 unrelated individuals were randomly selected. For European, each individuals were first assigned to one of the 5 1000G populations based on the minimum Mahalanobis distance. Then randomly selected 10,000 unrelated EUR individuals were used.
SNPs were filtered on INFO score > 0.9. MAF and pairwise LD were computed by PLINK (--r2 --ld-window 99999 --ld-window-r2 0.05) and SNPs with MAF=0 were excluded.
In both reference panels, 16,972,700 SNPs are available.
    

dbSNPs
------------

- FUMA version 1.7.0 uses dbSNPs version 146

QTLs
------------

Since FUMA version x.y.x, a different set of QTL datasets were processed for the QLT Analysis module in FUMA. This section is therefore divided into 2 parts, the first part was documented in the original `Tutorial` section in FUMA to maintain legacy while the second part is meant to document the updated data since FUMA version x.y.z

1. QTLs datasets from FUMA version x.y.z and earlier
++++++++++++++++++++++++++++++++++++++++++++++++++++

2. QTLs datasets from FUMA version x.y.z and beyond
+++++++++++++++++++++++++++++++++++++++++++++++++++
- For a complete documention of all the processed QLTs datasets that have been processed, please check https://github.com/tanyaphung/fuma_qtls
- Summary table: 

.. list-table:: QTL datasets that have been processed for the QTL Analysis module
   :widths: 25 25 25
   :header-rows: 1

   * - QTL Type
     - Source
     - Available Tissues
   * - eQTL
     - GTEx-v10
     - Adipose Subcutaneous, Adipose Visceral Omentum, Adrenal Gland, Artery Aorta, Artery Coronary, Artery Tibial, Bladder, Brain Amygdala, Brain Anterior cingulate cortex BA24, Brain Caudate basal ganglia, Brain Cerebellar Hemisphere, Brain Cerebellum, Brain Cortex, Brain Frontal Cortex BA9, Brain Hippocampus, Brain Hypothalamus, Brain Nucleus accumbens basal ganglia, Brain Putamen basal ganglia, Brain Spinal cord cervical c-1, Brain Substantia nigra, Breast Mammary Tissue, Cells Cultured fibroblasts, Cells EBV-transformed lymphocytes, Colon Sigmoid, Colon Transverse, Esophagus Gastroesophageal Junction, Esophagus Mucosa, Esophagus Muscularis, Heart Atrial Appendage, Heart Left Ventricle, Kidney Cortex, Liver, Lung, Minor Salivary Gland, Muscle Skeletal, Nerve Tibial, Ovary, Pancreas, Pituitary, Prostate, Skin Not Sun Exposed Suprapubic, Skin Sun Exposed Lower leg, Small Intestine Terminal Ileum, Spleen, Stomach, Testis, Thyroid, Uterus, Vagina, Whole Blood


