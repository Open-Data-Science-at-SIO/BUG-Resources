Using Transrate to assess transcriptome quality 
==============================================

:author: N Tessa Pierce
:date: Jan 8, 2016

Transrate can help assess the quality of your de-novo transcriptome assembly.
It calculates a number of length and sequence based metrics such as N50 and 
GC content, maps input reads back to contigs and calculates coverage and mapping
percentage, and provides a 'score' per assembly that represents quality. This metric
may help you choose the best assembler and parameters for your species. Transrate also
provides contig-level statistics to allow you to filter out low-scoring contigs.

- `website <http://hibberdlab.com/transrate>`__
- `preprint <http://dx.doi.org/10.1101/021626>`__


Disclaimer: For all tutorials, we assume you're working on a linux machine and 
you're installing into the $HOME directory. If you're installing to a different
directory, just replace $HOME with the path to that directory. FYI, transrate will
work on macs, see the website for instructions.

Install transrate
-----------------

download::

   cd $HOME
   curl -O -L https://bintray.com/artifact/download/blahah/generic/transrate-1.0.1-linux-x86_64.tar.gz 
   tar zxf transrate-1.0.1-linux-x86_64.tar.gz

add to $PATH::

   export PATH=$PATH:$HOME/transrate-1.0.1-linux-x86_64
   echo 'export PATH=$PATH:$HOME/transrate-1.0.1-linux-x86_64' >> ~/.bashrc
   export PATH=$PATH:$HOME/transrate-1.0.1-linux-x86_64/bin
   echo 'export PATH=$PATH:$HOME/transrate-1.0.1-linux-x86_64/bin' >> ~/.bashrc

install dependencies::

   transrate --install-deps ref
   
See the help information::

   transrate -h


Run transrate on test data
------------------

Create a working directory::

   mkdir $HOME/transrate
   cd $HOME/transrate

download transrate test data::

   curl -O -L https://bintray.com/artifact/download/blahah/generic/example_data.tar.gz
   tar zxf example_data.tar.gz
   

Run initial assembly evaluation (contig metrics only)::

   cd example_data
   transrate -a transcripts.fa


The following output should print to your screen::

   [INFO] 2016-01-08 10:32:31 : Contig metrics:
   [INFO] 2016-01-08 10:32:31 : -----------------------------------
   [INFO] 2016-01-08 10:32:31 : n seqs                           15
   [INFO] 2016-01-08 10:32:31 : smallest                        849
   [INFO] 2016-01-08 10:32:31 : largest                        2396
   [INFO] 2016-01-08 10:32:31 : n bases                       28562
   [INFO] 2016-01-08 10:32:31 : mean len                    1904.13
   [INFO] 2016-01-08 10:32:31 : n under 200                       0
   [INFO] 2016-01-08 10:32:31 : n over 1k                        14
   [INFO] 2016-01-08 10:32:31 : n over 10k                        0
   [INFO] 2016-01-08 10:32:31 : n with orf                       15
   [INFO] 2016-01-08 10:32:31 : mean orf percent              46.46
   [INFO] 2016-01-08 10:32:31 : n90                            1612
   [INFO] 2016-01-08 10:32:31 : n70                            1681
   [INFO] 2016-01-08 10:32:31 : n50                            2037
   [INFO] 2016-01-08 10:32:31 : n30                            2288
   [INFO] 2016-01-08 10:32:31 : n10                            2385
   [INFO] 2016-01-08 10:32:31 : gc                             0.53
   [INFO] 2016-01-08 10:32:31 : gc skew                       -0.05
   [INFO] 2016-01-08 10:32:31 : at skew                        0.06
   [INFO] 2016-01-08 10:32:31 : cpg ratio                      1.59
   [INFO] 2016-01-08 10:32:31 : bases n                           0
   [INFO] 2016-01-08 10:32:31 : proportion n                    0.0
   [INFO] 2016-01-08 10:32:31 : linguistic complexity          0.33
   [INFO] 2016-01-08 10:32:31 : Contig metrics done in 0 seconds
   [INFO] 2016-01-08 10:32:31 : No reads provided, skipping read diagnostics
   [INFO] 2016-01-08 10:32:31 : No reference provided, skipping comparative diagnostics


Read about the `contig metrics <http://hibberdlab.com/transrate/metrics.html#contig-metrics>`__.

Using read-based metrics to assess your assembly
---------------------------------------------------

Transrate has read-based metrics to assess how well your reads map to your transcriptome 
and how well the contigs in your transcriptome are supported by the reads.

You need to choose the right input reads for this comparison. Transrate recommends
using cleaned reads (i.e. post-trimming), to eliminate any negative quality assessments 
the might result from including low quality bases. Post-normalization reads work well,
though the normalization would affect the mean coverage stat. Read about `read mapping metrics here <http://hibberdlab.com/transrate/metrics.html#read-mapping-metrics>`__.

**You can use this analysis to decide whether or not you actually *need* to generate a 
de-novo assembly. If you have a close reference, use that as your "assembly" and 
run your cleaned reads against it.**

Run test evaluation using input reads::

   transrate --assembly transcripts.fa --left left.fq --right right.fq 

In addition to contig metrics, you should see::

   [ INFO] 2016-01-08 10:37:49 : Calculating read diagnostics...
   [ INFO] 2016-01-08 10:37:54 : Read mapping metrics:
   [ INFO] 2016-01-08 10:37:54 : -----------------------------------
   [ INFO] 2016-01-08 10:37:54 : fragments                     10000
   [ INFO] 2016-01-08 10:37:54 : fragments mapped              10000
   [ INFO] 2016-01-08 10:37:54 : p fragments mapped              1.0
   [ INFO] 2016-01-08 10:37:54 : good mappings                  9993
   [ INFO] 2016-01-08 10:37:54 : p good mapping                  1.0
   [ INFO] 2016-01-08 10:37:54 : bad mappings                      7
   [ INFO] 2016-01-08 10:37:54 : potential bridges                 0
   [ INFO] 2016-01-08 10:37:54 : bases uncovered                2902
   [ INFO] 2016-01-08 10:37:54 : p bases uncovered               0.1
   [ INFO] 2016-01-08 10:37:54 : contigs uncovbase                12
   [ INFO] 2016-01-08 10:37:54 : p contigs uncovbase             0.8
   [ INFO] 2016-01-08 10:37:54 : contigs uncovered                 2
   [ INFO] 2016-01-08 10:37:54 : p contigs uncovered            0.13
   [ INFO] 2016-01-08 10:37:54 : contigs lowcovered                9
   [ INFO] 2016-01-08 10:37:54 : p contigs lowcovered            0.6
   [ INFO] 2016-01-08 10:37:54 : contigs segmented                 0
   [ INFO] 2016-01-08 10:37:54 : p contigs segmented             0.0
   [ INFO] 2016-01-08 10:37:54 : Read metrics done in 5 seconds
   [ INFO] 2016-01-08 10:37:54 : No reference provided, skipping comparative diagnostics
   [ INFO] 2016-01-08 10:37:54 : TRANSRATE ASSEMBLY SCORE     0.6693
   [ INFO] 2016-01-08 10:37:54 : -----------------------------------
   [ INFO] 2016-01-08 10:37:54 : TRANSRATE OPTIMAL SCORE      0.9117
   [ INFO] 2016-01-08 10:37:54 : TRANSRATE OPTIMAL CUTOFF     0.7652
   [ INFO] 2016-01-08 10:37:54 : good contigs                     14
   [ INFO] 2016-01-08 10:37:54 : p good contigs                 0.93



Comparing your assembly to a reference
-------------------------------------------------

If you have a reference (DNA, RNA or peptide; does not need to be from your species),
you can use transrate to assess how well your transcriptome maps to the reference. 
If you're working with a relatively divergent reference, assessing relative to a 
peptide file may provide the best information. Read about the `comparative metrics here <http://hibberdlab.com/transrate/metrics.html#comparative-metrics>`__.


Transrate doesn't provide an example reference, but for demonstration purposes, 
we can use the 'transcripts.fa' as both assembly and reference. Since the files
are identical, we should see complete coverage.

To run reference assessment::

   transrate --assembly transcripts.fa --reference transcripts.fa 

In addition to contig metrics, you should see::

   [ INFO] 2016-01-08 11:40:21 : Comparative metrics:
   [ INFO] 2016-01-08 11:40:21 : -----------------------------------
   [ INFO] 2016-01-08 11:40:21 : CRBB hits                        15
   [ INFO] 2016-01-08 11:40:21 : n contigs with CRBB              15
   [ INFO] 2016-01-08 11:40:21 : p contigs with CRBB             1.0
   [ INFO] 2016-01-08 11:40:21 : rbh per reference               1.0
   [ INFO] 2016-01-08 11:40:21 : n refs with CRBB                 15
   [ INFO] 2016-01-08 11:40:21 : p refs with CRBB                1.0
   [ INFO] 2016-01-08 11:40:21 : cov25                            15
   [ INFO] 2016-01-08 11:40:21 : p cov25                         1.0
   [ INFO] 2016-01-08 11:40:21 : cov50                            15
   [ INFO] 2016-01-08 11:40:21 : p cov50                         1.0
   [ INFO] 2016-01-08 11:40:21 : cov75                            15
   [ INFO] 2016-01-08 11:40:21 : p cov75                         1.0
   [ INFO] 2016-01-08 11:40:21 : cov85                            15
   [ INFO] 2016-01-08 11:40:21 : p cov85                         1.0
   [ INFO] 2016-01-08 11:40:21 : cov95                            15
   [ INFO] 2016-01-08 11:40:21 : p cov95                         1.0
   [ INFO] 2016-01-08 11:40:21 : reference coverage              1.0
   [ INFO] 2016-01-08 11:40:21 : Comparative metrics done in 1 seconds
   [ INFO] 2016-01-08 11:40:21 : -----------------------------------



Comparing two or more assemblies
---------------------------------------------

If you run transrate on several assemblies, you can go into the csv output and compare the
transrate scores. Alternatively, you can directly compare two assemblies with a single command. 

If you have assembles one.fa and two.fa::

   transrate --assembly one.fa,two.fa

note: make sure there are no spaces between your comma-separated assembly names

Additional Notes
------------------------------------------

You can combine the above analyses into a single command::

   transrate --assembly one.fa,two.fa --left left.fa --right right.fa --reference transcripts.fa
   
You can specify the number of threads to use::

   transrate --assembly transcripts.fasta --threads 8

See more examples of transrate commands with::

   transrate --examples

Please see the transrate `website <http://hibberdlab.com/transrate>`__ for more information on usage & metrics.



