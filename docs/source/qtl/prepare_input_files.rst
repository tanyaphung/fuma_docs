Prepare Input Files
===================

.. _prepare_input_qtl:

GWAS summary statistics for a genetic locus of interest
+++++++++++++++++++++++++++++++++++++++++++++++++++++++

Overview
^^^^^^^^

- To perform a QTLs analysis on FUMA, a GWAS summary statistics for a genetic locus of interest is required. 
- The file has to have the following columns in this specific order: CHR, POS, REF, ALT, N, BETA, P, MAF. 
- Delimiter is a tab character.
- The file is not zipped.
- The file size needs to be less than 10Mb. Because this file is subsetted for a single genomic risk locus, this file should be relatively small. 
- Tips: write a script in R or Python to
    - Subset the full GWAS summary statistics using the start and end coordinates that represent a genetic locus of interest
    - Select the 8 columns: CHR, POS, REF, ALT, N, BETA, P, MAF
- Example code: 

.. code-block:: python
    
    outfile = open("locus.input", "w")
    header = ["CHR", "POS", "REF", "ALT", "N", "BETA", "P", "MAF"]
    print("\t".join(header), file=outfile)

    locus_chrom = {chrom} #replace with the chromosome from the genetic locus of interest
    start = {start} #replace with the start coordinates from the genetic locus of interest
    end = {end} #replace with the end coordinates from the genetic locus of interest

    with open({input}, "r") as f: #replace with the path to the GWAS summary statistics
        for line in f:
            if line.startswith("variant_id"): #replace with the correct value to skip header line if needed
                continue  # skip header line
            items = line.rstrip("\n").split("\t") #replace with the correct delimiter
            chrom = items[1] #replace with the correct index
            pos = items[2] #replace with the correct index
            if chrom == locus_chrom and int(pos) >= start and int(pos) <= end:
                alt = items[4] #replace with the correct index
                ref = items[3] #replace with the correct index
                n_samples = items[7] #replace with the correct index
                beta = items[6] #replace with the correct index
                p = items[8] #replace with the correct index
                maf = items[5] #replace with the correct index
                print("\t".join([chrom, pos, ref, alt, n_samples, beta, p, maf]), file=outfile)
    outfile.close()

.. tip:: 
    Use a programming language such as R or Python to prepare your input file

- Example input file: 

.. code-block:: bash

    CHR     POS     REF     ALT     N       BETA    P       MAF
    8       25243213        G       A       716517  0.0621  5.534e-07       0.6976
    8       25243984        G       A       716517  -0.0175 0.7148  0.9827
    8       25244245        G       A       716517  -0.0391 0.5636  0.9888
    8       25244270        G       C       716517  0.0257  0.03354 0.3088
    8       25244326        C       A       716517  0.0256  0.03399 0.3090

