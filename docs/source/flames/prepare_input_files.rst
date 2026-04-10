Prepare Input Files
===================

.. _prepare_input_file:


GWAS summary statistics
++++++++++++++++++++++++++

- To perform gene prioritization analysis with FLAMES on FUMA, a GWAS summary statistics is required. Specific requirements:
    - This is the same summary statistics file that you used for SNP2GENE analysis.
    - The file has to have the following columns in this specific order: CHR, BP, A2, A1, RSID, P, BETA. 
    - The header has to start with "#"
    - The file has to be bgzip

    .. code-block:: bash
        
        bgzip -c {input} > {input}.gz #replace with the path to the GWAS summary statistics

