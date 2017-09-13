# SIO Nonmodel mRNASeq Workshop (2017) 

These are the schedule and classroom materials for the
[2017 RNAseq workshop at SIO](http://sio-rnaseq.readthedocs.io/en/latest/),
which will run from Oct 10-11, 2017.

This workshop runs under a [Code of Conduct](code-of-conduct.html). Please
respect it and be excellent to each other!

If you're not comfortable working on the command line, please work through some of this [command-line bootcamp](http://rik.smith-unna.com/command_line_bootcamp/) before the workshop.

**Schedule and Location**:  

9am-5pm both days, with 1hr break for lunch?

All sessions are in **Eckhart?**, unless otherwise noted.


## Workshop materials


### Wednesday, Day 1: Introduction, QC, Read Quantification

* pre-workshop: please ensure that you have a terminal emulator installed on your laptop.

* 9am: Introductions & RNA-Seq uses & pitfalls
* Hands-on:
   * [Booting a cloud computer from Jetstream](jetstream/boot.html)
   * [Quality trimming your reads](quality-trimming.html)

* Lecture: [Quantification](_static/quantification_slides_Patro_subset.pdf) 
*  Hands-on: [Quantification with Salmon](quantification.html)
   
Other useful lessons   
* Using ssh to log in (instead of the web shell):
     * [Log in using a password](jetstream/ssh_changepassword.html)
     * *OR* [Add ssh keys to your instance](jetstream/login.html)

### Thursday, Day 2: Differential Expression, downstream assessment, and Reproducibility
   
* Lecture: [Differential Expression](_static/Jane_differential_expression.pdf)
* Hands-on: [Differential Expression with DESeq2](DE.html) 

* Discussion: RNA-Seq Study design
  
*  GitHub tutorials:  
     * [Lisa's tutorial](LC-github.html)
     * [tutorial from ANGUS 2017](github.html)


Potentially interesting:
* [Reference independent analyses with k-mers; sourmash.](kmers-and-sourmash.html)
   


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



*format copied from ctb dibsi 2017 materials*
