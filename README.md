# SeSiMCMC revisited: Stochastic Chaos participates in the IBIS challenge

### Team name
Stochastic Chaos

### Primary Disciplines
A2G-PWM, G2A-PWM -- actually, we tried to identify motifs in everything we could identify one in.

# Summary

SeSiMCMC is a Gibbs sampler for de-novo identification of similar nucleotide substrings in the input. The input is a collection of nucleotide sequences, and we know that many of them contain the binding sites of the TF of interest. So, the substrings are supposed to be the TF binding sites of the TF. The algorythm uses the PWM model to describe the motif. The SeSIMCMC paper was published 20 years ago, and our main goal of participation in the challenge was to answer whether the algorythhms of this class are still usable in the modern world of deep learning models.

### Implementation & Software
SeSiMCMC [doi:10.1093/bioinformatics/bti336] identified motifs in the input sequence data. R, perl and bash were used to prepare and to prepare the results to submission.

# Methods
## Motif search
We searched for motifs in fasta files collections using the SeSiMCMC [https://github.com/favorov/SeSiMCMC] software.

The default search parameters were used, with two adjustments. For SMiLE-seq, we set the probability for an input sequence to be garbage (i.e not to carry the motif) to 0.9 instead of the default 0.1 . For all the runs, we set the number of chains (attempts) to 7 instead of default 3. The time limit was posted to 10000 seconds (the default is 1000). 

We added --ibis command-line switch to the SeSiMCMC software to generate the IBIS challenge formatted output.

The SeSiMCMC was invoked by a bash script. The script was generated by an R script simultaneously with the fasta data preparation.

## Fasta preparation.
To  prepare the fasta-formatted nucleotide sequence collections and to generate the bash script to invoke the motif identification, we wrote R script https://gitlab.com/favorov/stochaos-ibis-challenge/-/blob/main/scripts/prepare_fasta_and_script.R?ref_type=heads. 
To prepare the fasta, the peaks files were truncated to get the best peaks, and then the peaks were mapped to the hg38 genome. The SMILEseq files were subsetted at random and converted to fasta. The PBM files were subsetted so that only the intersection of the best records according to z-score and the best according to the foreground level.

## PWM joining.
The SeSiMCMC --ibis switch prepares the output in ibis format, still when the task fails, an empty PWM file or a text with commented options of SeSIMCMC run appears. To filter this garbage out and to join the valid pwms, we wrote perl script https://gitlab.com/favorov/stochaos-ibis-challenge/-/blob/main/scripts/join_pwms.pl . The script also controlled the format compliance.

## References
[doi:10.1093/bioinformatics/bti336] SeSIiMCMC.
