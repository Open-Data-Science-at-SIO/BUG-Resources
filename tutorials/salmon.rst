Using Salmon for RNA-Seq quantification 
==============================================

:author: N Tessa Pierce
:date: Jan 8, 2016


Salmon is a fast RNA-Seq read quantification program. "Quantification" methods
differ from the typical read count methods by avoiding full alignment.
Salmon results can be used with edgeR, DESeq2, etc for differential expression analysis.

- `website <http://salmon.readthedocs.org/en/latest>`__
- `preprint <http://biorxiv.org/content/early/2015/06/27/021592>`__


*Disclaimer:* For all tutorials, we assume you're working on a linux machine and 
you're installing into the $HOME directory. If you're installing to a different
directory, just replace $HOME with the path to that directory. Wherever possible,
we download pre-compiled linux binaries. If you're working on a different machine,
or the binary installation doesn't work, please see the software website for 
instructions to install the software from source code. 

*Download tip:* If you're on a secure file system (e.g. SWFSC people), you may not be able to 
download using curl. In that case, do the downloads on your laptop or other machine (either with
these commands or via the tool's website, and use sftp to transfer the files up to your linux machine.


Install Salmon
-----------------

download::

   cd $HOME
   curl -O -L https://github.com/COMBINE-lab/salmon/releases/download/v0.6.0/SalmonBeta-0.6.0_DebianSqueeze.tar.gz 
   tar zxf SalmonBeta-0.6.0_DebianSqueeze.tar.gz

Add to $PATH::

   export PATH=$PATH:$HOME/SalmonBeta-0.6.0_DebianSqueeze/bin
   echo 'export PATH=$PATH:$HOME/SalmonBeta-0.6.0_DebianSqueeze/bin' >> ~/.bashrc
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/SalmonBeta-0.6.0_DebianSqueeze/lib
   echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/SalmonBeta-0.6.0_DebianSqueeze/lib' >> ~/.bashrc


See the help information::

   salmon -h


Run salmon on test data
------------------

Create a working directory::

   mkdir $HOME/salmon
   cd $HOME/salmon

Transrate provides some test data that we can use to test salmon::

   curl -O -L https://bintray.com/artifact/download/blahah/generic/example_data.tar.gz
   tar zxf example_data.tar.gz
   
First, index the FASTA file::

   cd example_data
   salmon index --transcripts transcripts.fa --index transcripts_index

To see help information for the indexing tool::

   salmon index -h


Quantify the paired-end reads::

   salmon quant --index transcripts_index --libType IU -1 left.fq -2 right.fq -o sample_quant
      
Note: ``--libType`` must come *before* the read files
   
To see help information for the quantification tool::
   
   salmon quant --help-reads


Results
----------------------

Look at the log file, which contains the parameters used in the run, plus a % mapping::

   cat sample_quant/logs/salmon_quant.log


Look at the quantification results:: 

   cd sample_quant
   cat quant.sf 


Columns in quant.sf (`from salmon docs <http://salmon.readthedocs.org/en/latest/salmon.html#output>`__):


- **Name**: Name of the target transcript provided in the input transcript database (FASTA file).
- **Length**: Length of the target transcript in nucleotides.
- **Effective Length**: Effective length refers to the number of possible start sites a feature could have generated a fragment of that particular length.
- **TPM**: Salmon’s estimate of the relative abundance of this transcript in units of Transcripts Per Million (TPM). TPM is the recommended relative abundance measure to use for downstream analysis.
- **NumReads**: This is salmon’s estimate of the number of reads mapping to each transcript that was quantified. It is an “estimate” insofar as it is the expected number of reads that have originated from each transcript given the structure of the uniquely mapping and multi-mapping reads and the relative abundance estimates for each transcript.

For a discussion of RNA-Seq expression units, including TPM vs FPKM, see this `blog post <https://haroldpimentel.wordpress.com/2014/05/08/what-the-fpkm-a-review-rna-seq-expression-units/>`__.


Some Practical Considerations for running your own samples
--------------------------------------------------------

*coming soon*

- The libType Parameter

- Using salmon for alignment-based quanification 




Downstream Analyses
-----------------------

*Tutorials for edgeR, deseq2, etc coming soon*

Imp: for these count-based methods, use the "NumReads" column from the quant.sf file.


