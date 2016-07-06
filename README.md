# Differential Mutation Analysis

Differential mutation analysis is a framework that uncovers cancer genes by comparing the mutational profiles of genes across cancer genomes with their natural germline variation profiles across healthy individuals.

This is the code for out method for evaluating genes for differential mutation. Our approach is entirely based on somatic mutations and germline variation, without any additional parameters. Briefly, for a cancer type of interest, we first count, for each individual, the number of mutations found in the exons of each gene. Similarly, we use the 1000 Genomes sequencing data to count, for each individual, how many variants appear in each gene. We define a variant as any amino acid that differs from the most common one across the healthy cohort. For each individual, we then rank normalize the mutation or variant counts across genes so that each gene is assigned a score between 0 and 1 that reflects the relative number of mutations or variants that fall within it. Next, for each gene, we aggregate its mutation and variation scores across healthy and cancer cohorts separately, resulting in a set of normalized variation scores as well as a set of normalized mutation scores. We use these sets to build a pair of histograms estimating the density of mutation and variant normalized scores. The first represents the gene’s tendency to be ranked highly amongst all genes with respect to somatic mutation across a cancer genome cohort; the other represents its tendency to be ranked highly with respect to germline variation across a healthy cohort. In order to uncover whether a gene has a mutational profile that is very different between healthy and cancer cohorts, we compute the distance between the two distributions using a modification of the classical Earth Mover’s Distance, which we refer to as a unidirectional Earth Mover’s Distance (uEMD). Finally, we rank all genes by their uEMD scores, considering higher ranking genes to be more likely to be functionally related to a given cancer type, and compute a supporting p-value for each uEMD Score.

The only required input is a single MAF file, a sample MAF file for BRCA is
provided in "Data/BRCA_sample.maf" and can be tested by calling

DifferentialMutationAnalysis("Data/BRCA_sample.maf")

Output is a single two column file with protein names and their uEMD scores
named "uEMDscores.txt"

The code can optionally be run to search for oncogenes or tumor suprressor
genes separately by passing "onco" or "TSG" as options for geneType

DifferentialMutationAnalysis("Data/BRCA_sample.maf", geneType="onco")

Finally, the code can optionally compute supporting p-values for genes. To 
compute p-values for the p highest scoring genes, simply pass a value p

DifferentialMutationAnalysis("Data/BRCA_sample.maf", p=100)

This will generate a separate p-value file ordered by uEMD score called
"pvalues.txt"
