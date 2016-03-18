Using BUSCO to Assess Transcriptome Quality 
==============================================

:author: N Tessa Pierce
:date: Jan 8, 2016

*in progress*

Why use BUSCO?
-----------------


BUSCO (Benchmarking Universal Single-Copy Orthologs) provides measures for quantitative assessment of genome assembly, gene set, and transcriptome completeness based on evolutionarily informed expectations of gene content from near-universal single-copy orthologs selected from OrthoDB.

For transcriptomes, BUSCO %complete and %fragmented metrics can be used as an estimate of the "completeness" of the assembly and should be reported in publication of the transcriptome. When comparing assemblies made with different paramters, the best assemblies will maximize the %complete/%fragmented metrics without inflating the %duplicated metric. 


- `website <http://busco.ezlab.org>`__
- `paper <http://dx.doi.org/10.1093/bioinformatics/btv351>`__


Disclaimer: For all tutorials, we assume you're working on a linux machine and 
you're installing into the $HOME directory. If you're installing to a different
directory, just replace $HOME with the path to that directory. Wherever possible,
we download pre-compiled linux binaries. If you're working on a different machine,
or the binary installation doesn't work, please see the software website for 
instructions to install the software from source code.



Install BUSCO
-----------------

download::

   cd $HOME
   curl -O -L http://busco.ezlab.org/files/BUSCO_v1.1b1.tar.gz
   tar zxf BUSCO_v1.1b1.tar.gz
   
add BUSCO to $PATH::

   cd BUSCO_v1.1b1/
   chmod +x BUSCO_v1.1b1.py
   export PATH=$PATH:$(pwd)

See the help information::

   python3 BUSCO_v1.1b1.py -h
   
NOTE: BUSCO requires python3. Most linux servers should have this pre-installed.



Run BUSCO on test data
------------------

Create a working directory::

   mkdir $HOME/busco
   cd $HOME/busco
   
Download a busco database::

   curl -LO http://busco.ezlab.org/files/metazoa_buscos.tar.gz
   tar -zxf metazoa_buscos.tar.gz

**ADD: information on BUSCO databases**

Transrate provides some test data that we can use to test BUSCO::

   curl -O -L https://bintray.com/artifact/download/blahah/generic/example_data.tar.gz
   tar zxf example_data.tar.gz

Run BUSCO::

   python3 BUSCO_v1.1b1.py -m trans -in transcripts.fa --cpu 4 -l metazoa -f -o transcripts_metazoaBusco


Results
-------------
   
Look at the short results summary::

   cat run*/short*
   
** ADD: using BUSCO to assess transcriptomes, genomes etc. What is a "good" result, etc




