# SIO Nonmodel mRNASeq Workshop (2017) 

These are the schedule and classroom materials for the
[2017 RNAseq workshop at SIO](http://sio-rnaseq.readthedocs.io/en/latest/),
which will run from Oct 11-12, 2017. This will be followed by an Oct 13th lecture from instructor
Lisa Komoroske as part of the Marine Biology Seminar Series.

This workshop runs under a [Code of Conduct](code-of-conduct.html). Please
respect it and be excellent to each other!

If you're not comfortable working on the command line, please work through some of this [command-line bootcamp](http://rik.smith-unna.com/command_line_bootcamp/) before the workshop.

**Schedule and Location**:  

All sessions are in the Eckhart Sea Cave, unless otherwise noted. The workshop runs from 9am-5pm, with a 1 hour break for lunch.


## Workshop materials


### Wednesday, Day 1: Introduction, QC, Assembly, and Quantification

   * Introductions, RNA-Seq uses & pitfalls
   * [Booting a cloud computer from Jetstream](jetstream/boot.html)
   * [Quality trimming your reads](quality-trimming.html)

Lunch Break

   * [De novo RNAseq assembly](_static/assembly-trinity.html)
   * Discussion: RNA-Seq study design 
   * [Quantification with Salmon](quantification.html)  *(might be pushed to thurs morning)*


### Thursday, Day 2: Differential Expression, Downstream Analyses, Alternate RNA-Seq pipelines & resources
   
   * [Differential Expression with DESeq2](DE.html) 
   * Discussion & examples: downstream analyses

Lunch Break

   * Discussion: Modifications & Considerations for microbial transcriptomics & metatranscriptomics
    


#### Useful Further Resources:  
 Note that these are taken from the 2017 [ANGUS](http://angus.readthedocs.io/en/2017/index.html) workshop

* [Introduction to automation](introduction-to-automation.html)
* [Jupyter Notebook, R and Python for data science.](jupyter-notebook-demo/Jupyter-Notebook-Notes.html)
* [Review and explore: Command line UNIX, and R/RStudio](command-line-and-rstudio.html)
* [Pathway Analysis](pathway_analysis.html) 
* [RMarkdown](rmarkdown_rnaseq.html)
* [where do I find the data? NCBI, ENSEMBL, ENA; how to get FASTQ out of NCBI.](database_resources.html)

*  [Adrienne Roeder](http://roeder.wicmb.cornell.edu/), Cornell - [Reaching biological conclusions from RNA-seq: the good, the bad, and the ugly](https://osf.io/qz3m6/)
*  [Michael I Love](https://mikelove.github.io/), UNC Chapel Hill - ["Statistics and bias correction in RNAseq differential expression analysis"](https://osf.io/gbjhn/)
*  [Robert Patro](http://www.robpatro.com/redesign/), Stony Brook University - ["Don't count on it: Pragmatic and theoretical concerns and best practices for mapping and quantifying RNA-seq data"](https://osf.io/bv85u/)
*  C. Titus Brown, UC Davis - ["Effectively infinite: next steps in Data Intensive Biology."](https://osf.io/pbmeh/)
* [Assessing & assembling nanopore data](analyzing_nanopore_data.html) (Lisa Cohen and Jon Badalamenti)

Related/useful lessons:
* Lecture: [Assembly evaluation](_static/Jane_assembly_stats.pdf)
* Hands-on: [Assembly evaluation](evaluation.html)
* Lecture: [Quantification](_static/quantification_slides_Patro_subset.pdf) 
* Lecture: [Differential Expression](_static/Jane_differential_expression.pdf)

* Using ssh to log in (instead of the web shell):
     * [Log in using a password](jetstream/ssh_changepassword.html)
     * *OR* [Add ssh keys to your instance](jetstream/login.html)

Potentially interesting:
* Lecture: [Annotation](_static/Jane_transcriptome_annotation.pdf)
* Hands-on: [Transcriptome annotation](dammit_annotation.html)
*  GitHub tutorials:  
     * [Lisa's tutorial](LC-github.html)
     * [tutorial from ANGUS 2017](github.html)
* [Reference independent analyses with k-mers; sourmash.](kmers-and-sourmash.html)

