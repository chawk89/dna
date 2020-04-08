# dna
ABSTRACT:
Stanford graduate project to generate and explore polygenic scores for DNA files.

CONTRIBUTIONS:

Colby Hawker (Marketing S&O Program Manager, Visiting Student @ Stanford) - primary developer of the backage and author of clustering and effect size functionality, contributor to polygentic score generatation and plotting functions, and developer of the core R package components and documentation; co-athor of report summary.

Alex Kern (3rd year PhD, Genetics @ Stanford) - primary author of polyGenerate and several of the functions relating to polygentic scores and manhattan plots; co-author of the description and challenges report summary.


DESCRIPTION:
This is a package to help users generate polygenic scores for specific traits from DNA samples. Users can upload lists of SNP’s correlated with specific traits, or use the ‘Big 5’ personality traits sample data (researched by 23andMe) provided by the package. Users can also generate sample DNA test sets.

Polygenic scores use associations between traits and single-nucleotide polymorphisms (SNPs) measured in a genome-wide association study to predict the level of the trait. They can be used to stratify risk groups for a disease. However, they can be overinterpreted, and also can be biased by a number of factors such as population stratification, relatedness, the population used to measure the associations, and the method used to choose the SNPs used in the score. 

CHALLENGES: From an functionality standpoint, determining how to generate randomized or permuted DNA samples in the polyGenerate function was challenging. We wanted each sample to capture some amount of realism, and we wanted to maintain the links (i.e. shared highly associated SNPs) between the traits to use for clustering. Initially, we thought to join all of the trait summary score dataframes, but unfortunately, there was not enough overlap, and so we would end up with empty dataframes. In the interest of making the generated DNA samples “realistic,” we originally tried to prune them for linkage disequilibrium (LD), so that none of the SNPs were too close to each other or too highly correlated. However doing this for each trait separately or for all of them together made it difficult to maintain the few SNPs that did overlap in the permutation. Ultimately, we combined the trait dataframes into one after some trimming using rbind and generated the DNA samples by taking a binomial random sampling at the B allele frequency for each SNP for each permutation. However, for overlapping SNPs between datasets this created two different sets of randomized allelic dosages, so we had to just take the first set of randomizations so that each column wouldn’t have two different allelic dosages for the same SNP. Thus, this method created randomized DNA samples with realistic allele frequencies matching the study population, and which maintained the genetic associations between traits as much as possible. 

From an R development perspective, we had the challenge of binding the global variables, as there were many due to package imports and variables introduced by assignment in functions. We used the suggested method to handle global variables. One of the other development challenges was ensuring consistency of documentation and providing the right tags to prevent warnings



ORGANIZATION OF PACKAGE:
This package has four different .r files containing the core functionality:
1. polyScores - allows users to generate a polygenetic score given a `DNA file` and a list of top SNPS associated with each trait.
   Functions: `polyScore`, `polyScore_effects`
2. polyPlots - manhattan plots to visualize effect sizes and signficiance of top SNPs associated with each trait.
   Functions: `manhattan_plot`, `effect_size_plot`
   
3. polyGenerate - generate a list of simulated polygenetic scores modeled for a given set of traits.
   Functions: `polyGenerate` 
   
4. polyCluster - use K-means clustering to segment and visualize groups of observations by polygenic scores.
   Functions: `polyCluser`, `pcaCluster`
   
The package also includes valuable data sets, including:
1. A sample DNA file upload: `dnaFile`
2. GWAS summary statistics with top SNP's assosiated with each of the Big 5 personality traits: `top_SNPs`
3. GWAS summary statistics with significant SNP's assosiated with each of the Big 5 personality traits: `sig_SNPs` 

FUTURE DIRECTIONS:
In the future it would be good to be able to take in full GWAS data sets with each person’s SNPs and phenotype, so that we could leverage more powerful and accurate methods to generate polygenic scores. For instance, it would then be possible to not rely on single SNP associations but use more modern variable selection techniques like LASSO to pick the SNPs going into the model. We could also then train and test different polygenic score models to achieve a better performing one. It would also be good to pull in LD information, perhaps from the 1000 Genomes Project or another sequencing project so that we could remove SNPs in too high LD, which is thought to help the accuracy of these models. 

