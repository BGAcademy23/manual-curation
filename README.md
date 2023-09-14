# Introduction to Manual Curation - HiC and JBrowse

This session is part of [**Biodiversity Genomics Academy 2023**](https://BGA23.org)

## Session Leader(s)

Genome Reference Informatics Team, Wellcome Sanger Institute

- Jo Wood
- Joanna Collins
- Dominic Absolon
- Tom Mathers
- Michael Paulini
- Camilla Santos

## Description

By the end of this session you will be able to:

1. Understand the Manual curation pipeline
2. Understand how Jbrowse can be utilised as a tool both to  aid curation and also for post curation analysis
3. Interpret a HiC Map
4. Understand how to curate an assembly using PretextView and associated curation scripts
5. Create an updated/curated assembly fasta

## Prerequisites

1. Understanding the terms genome assembly, reads, contigs
2. Understanding of linux command line basics
3. To have read/watched the rapid curation documentation available at <https://gitlab.com/wtsi-grit/rapid-curation/-/tree/main>
4. To have read through the Interpreting_HiC_maps guide
5. Have access to a 3 button mouse

!!! warning "Please make sure you MEET THE PREREQUISITES and READ THE DESCRIPTION above"

    You will get the most out of this session if you meet the prerequisites above.

    Please also read the description carefully to see if this session is relevant to you.
    
    If you don't meet the prerequisites or change your mind based on the description or are no longer available at the session time, please email tol-training at sanger.ac.uk to cancel your slot so that someone else on the waitlist might attend.


## Instructions

Step 1. Run rapid_split on your decontaminated, pre-curation fasta file to create a tpf

    perl rapid_split.pl -fa <fasta>

Step 2. Launch PretextView to visualise the contact map for the assembly

    ./PretextView &

    # Note that you must have pop-ups enabled in your browser as this opens the software in a new window

Step 3. Resolve the map within PretextView
    
    Ensure any gap that is edtied in the map is reflected in the tpf with a ">" at the start of the "GAP" line.

Step 4. Add metadata information to the assembly

    "Paint" chromosomes using the chromosome paining mode. Keyboard shortcut "s". Also add any metadata tags required for example "X" "Z" "haplotig" etc. using metadata tagging mode (keyboard shortcut "m")

Step 5. Output an AGP file from PretextView

    This is achieved by navigating to the menu in PretextView (keyboard shortcut "u") and clicking "Generate AGP" 

Step 6. Run rapid_prextext2tpf_XL. This takes as input your outputted agp, your edited tpf and the original fasta file. The output of this script is a csv file reflecting the chromosomes, a new tpf file that reflects the structure of what the new curated fasta will look like and a tpf file for an haplotigs that were marked in the curation.

    python rapid_pretext2tpf_XL.py <tpf> <agp>
    
    Output files will appear in the working directory and will be named: 
        chrs.csv, rapid_prtxt_XL.tpf and haps_rapid_prtxt_XL.tpf 

Step 7. Run rapid_join. This is what generates a new fasta file from the curation manipulations

    perl rapid_join.pl -csv chrs.csv -fa <pre-curation fasta> -tpf rapid_prtxt_XL.tpf -out <name for curated fasta> 

    N.B. rapid_join.pl can also be used to output a fasta file of the haplotigs by running the script with the -hap flag and using the haps_rapid_prtxt_XL.tpf file 

Step 8. Normally after this one should re-map the HiC data to the new fasta file and check no errors were made or anything was missed in the first curation effort. We unfortunately do not have time for this during this practical. 
