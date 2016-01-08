Using BUSCO to Assess Transcriptome Quality 
==============================================

:author: N Tessa Pierce
:date: Jan 8, 2016

BUSCO .... information 

- `website <http://salmon.readthedocs.org/en/latest>`__
- `paper <http://biorxiv.org/content/early/2015/06/27/021592>`__


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

   python3 ./BUSCO_v1.1b1.py -h
   
NOTE: BUSCO requires python3. Most linux servers should have this pre-installed.

download a busco database::

   curl -LO http://busco.ezlab.org/files/metazoa_buscos.tar.gz
   tar -zxf metazoa_buscos.tar.gz


Run BUSCO on test data
------------------

Create a working directory::

   mkdir $HOME/busco
   cd $HOME/busco

Transrate provides some test data that we can use to test salmon::

   curl -O -L https://bintray.com/artifact/download/blahah/generic/example_data.tar.gz
   tar zxf example_data.tar.gz


Run BUSCO::

   python3 BUSCO_v1.1b1.py -m trans -in transcripts.fa --cpu 4 -l metazoa -f -o transcripts_metazoaBusco


Results
-------------
   
Look at the short results summary::

   cat run*/short*




