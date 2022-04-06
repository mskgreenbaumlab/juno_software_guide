## Tracer
Purpose:  Reconstruction of T cell receptor sequences from single-cell RNA-seq data.
Location: /work/greenbaum/software/tracer
Usage:

1. Modify tracer.conf based on your needs. I have specified dependencies executable based on their location on juno server.

2. Copy the /work/greenbaum/software/tracer/run_tracer.sh script to your work directory. only modify the tracer commands (tracer includes different modes: assemble, summarise, build, test)

3. Execute run_tracer.sh with singularity image using following commands:

module load singularity/3.7.1
singularity exec --bind /work/greenbaum /work/greenbaum/software/images/tracer_latest.sif /bin/sh /work/greenbaum/software/tracer/run_tracer.sh

Results:

For each cell, an /<output_directory>/<cell_name> directory will be created. This will contain the following subdirectories.

<output_directory>/<cell_name>/aligned_reads
This contains the output from Bowtie2 with the sequences of the reads that aligned to the synthetic genomes.

<output_directory>/<cell_name>/Trinity_output
Contains fasta files for each locus where contigs could be assembled. Also two text files that log successful and unsuccessful assemblies.

<output_directory>/<cell_name>/IgBLAST_output
Files with the output from IgBLAST for the contigs from each locus.

<output_directory>/<cell_name>/unfiltered_TCR_seqs
Files describing the TCR sequences that were assembled prior to filtering by expression if necessary.

unfiltered_TCRs.txt : text file containing TCR details. Begins with count of productive/total rearrangements detected for each locus. Then details of each detected recombinant.
<cell_name>_TCRseqs.fa : fasta file containing full-length, reconstructed TCR sequences.
<cell_name>.pkl : Python pickle file containing the internal representation of the cell and its recombinants as used by TraCeR. This is used in the summarisation steps.
<output_directory>/<cell_name>/expression_quantification
Contains Kallisto/Salmon output with expression quantification of the entire transcriptome including the reconstructed TCRs. When option --small_index is used, this directory contains only the output of the quantification with the small index (built from reconstructed TCRs and only a subset of the base transcriptome; see above).

<output_directory>/<cell_name>/filtered_TCR_seqs
Contains the same files as the unfiltered directory above but these recombinants have been filtered so that only the two most highly expressed from each locus are retained. This resolves biologically implausible situtations where more than two recombinants are detected for a locus. This directory contains the final output with high-confidence TCR assignments.






