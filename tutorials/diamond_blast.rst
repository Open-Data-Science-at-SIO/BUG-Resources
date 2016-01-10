Blasting with DIAMOND 
===================================

:author: N Tessa Pierce, ntpierce@ucsd.edu
:date: Nov 3, 2015

For a discussion of the diamond algorithm & when to use diamond, see the pdf from Sheila Podell's talk `here. <https://github.com/SIO-BUG/BUG-Resources/blob/master/presentations/diamond_talk_110915.pdf>`__

tl;dr: Diamond is a very fast BLAST+ alternative, but only works on protein databases (=only blastx, blastp, no blastn). Use it when you have large datasets or want to map to large databases (e.g. NR, uniref90, etc). It's easy to install and use. The `paper <http://dx.doi.org/10.1038/nmeth.3176>`__ (+ sheila's testing) suggests it's comparable to blast+ in terms of accuracy and sensitivity.


- `Website <http://github.com/bbuchfink/diamond>`__
- `Nature Methods paper <http://dx.doi.org/10.1038/nmeth.3176>`__


Disclaimer: For tutorials, we typically assume you're working on a linux machine and 
you're installing into the $HOME directory. If you're installing to a different
directory, just replace $HOME with the path to that directory. Wherever possible,
we download pre-compiled linux binaries. If you're working on a different machine
or the binary installation doesn't work, please see the software website for 
instructions to install the software from source code.



Install DIAMOND
-----------------

Download the linux binary for diamond and unzip it::
      
   cd $HOME     # go to your home directory
   curl -O -L http://github.com/bbuchfink/diamond/releases/download/v0.7.9/diamond-linux64.tar.gz
   tar zxf diamond-linux64.tar.gz
      
Put Diamond in your path::
   
   export PATH=$PATH:$HOME/diamond
   echo 'export PATH=$PATH:$HOME/diamond' >> ~/.bashrc
   

**On Mac** (with homebrew)::

    brew install homebrew/science/diamond
    
    
*If you're not using homebrew, see the diamond `documentation <https://github.com/bbuchfink/diamond/#compiling-from-source>`_ for installing diamond from source. Good documentation; should be fairly easy.


Using DIAMOND
-----------------

See the diamond help info `online <https://github.com/bbuchfink/diamond/>`__ or via command-line::

    diamond -h
    

Make a working directory::

   cd $HOME
   mkdir diamond_testing
   cd diamond_testing

Download the database you'd like to map to. As an example, let's download the SWISS-PROT database::
   
   curl -O -L ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
   gunzip uniprot_sprot.fasta.gz

Index the database::

   diamond makedb --in  uniprot_sprot.fasta -d  uniprot_sprot

You should now see ``uniprot_sprot.dmnd`` in the folder.


Download a test fasta (or use your own)::

   # Let's get the transrate test data
   curl -O -L https://bintray.com/artifact/download/blahah/generic/example_data.tar.gz
   tar zxf example_data.tar.gz


With a nucleotide fasta, use ``diamond blastx``; with a protein fasta, use ``diamond blastp``


Run Diamond::

   diamond  blastx -d uniprot_sprot -q ./example_data/transcripts.fa -a matches -p 4


Results
--------

To see the results, we need to convert the binary output from diamond ``matches.daa`` to 
a human-readable format. Diamond provies a tool ``view`` for this purpose.

Convert Diamond output to blast tab-separated output format::

   diamond view -a matches.daa -o matches.m8


You should see 12 columns in the tabular output ``matches.m8``. If you've used command-line blast+ 
in the past, these columns are equivalent to the columns printed by blast+ output format 6.

   - query id
   - subject id
   - % identity
   - alignment length
   - mismatches
   - gap opens
   - query start
   - query end
   - subject start
   - subject end
   - e-value
   - bit score
   
Note that any tab-separated or comma-separated file can be opened in excel/google sheets/your favorite spreadsheet program.
