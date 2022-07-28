
#             Standard McDonald–Kreitman test

## Reference:
   1. McDonald, J. H. Kreitman (1991).
   2. Begun et al 2007.
   3. Nucleic Acids Res. 2008, W157-62, 10.1093/nar/gkn337.


Usage:  
`cmktest`  
`   -f, --fasta      input MSA including outgroup species and ingroup populations.  
    -i, --ingrp      ingroup species (e.g. 'dmel').  
    -o, --outgrp     outgroup species (e.g. 'dsim,dyak').  
    -c, --cutoff     optional, default 0.00000001.  
    -v, --verbose    optional, print polymorphism of each site.  
    -h, --help       optional, print this info.`  

Steps to do MK test:

1. `vcf2msa.py` to generate a MSA of the target gene(s)

    Usage: `python vcf2msa.py <vcf> <fasta> <outdir> <species_name>`

    1.a Note for fasta input:
     - fasta headers should be CDS coordinates splitted by";"
     - i.e., `chrom(-):start1:end1;chrom(-):start2:end2`
     - add a "-" to chromosome if it is in reverse strand

     - one example: 

    `>13:20208087:20208101;13:20216255:20216410;13:20220583:20221431`
    `ATG...TAA`


    1.b Note for species name:
     - species name will be used as headers of output MSA

    e.g.,
    `>dmel_0`
    `>dmel_1`
    `>dmel_2`
    `...`

    where `dmel_0` is CDS of the reference genome



2. use prank to realign MSA to the outgroup CDS

    2.a add outgroup CDS to the output MSA, header should be

        `>outgroup_species_name_0`
        e.g.,
        `>dsim_0`

    2.b after editing use prank to align the new MSA

        `prank -F -codon -d=<new_MSA_file> -o=<any_output_name>`

3. MK test using the aligned MSA
    3.a make sure that headers in this MSA are correct
        an example:
        `>dsim_0`
        `ATGCTTTAA`
        `>dmel_0`
        `ATGTTTTAA`
        `>dmel_1`
        `ATGTCTTAA`
        `>dmel_2`
        `ATGCTTTAA`

    3.b MK test
        `cmktest -f <fasta> -i <ingroup_name> -o <outgroup_name> -c <allele frequency cutoff to do MK test, usually 0.05>`

        an example:
        `cmktest -f FBgn0032916.fasta -i dmel -o dsim -c 0.05`

        for details, use `cmktest -h`        

4. Enjoy!
