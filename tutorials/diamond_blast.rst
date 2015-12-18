Blasting with DIAMOND 
===================================

Make a new directory for diamond, for example, in your home directory::

   cd
   mkdir diamond
   cd diamond

**On Linux**: download diamond & put in your path::
   
   curl -O http://github.com/bbuchfink/diamond/releases/download/v0.7.9/diamond-linux64.tar.gz
   tar xzf diamond-linux64.tar.gz
   export PATH=$PATH:$HOME/diamond
   echo 'export PATH=$PATH:$HOME/diamond' >> ~/.bashrc
   
**On Mac** (with homebrew)::

    brew install homebrew/science/diamond
    
*If you're not using homebrew, see the diamond `documentation <https://github.com/bbuchfink/diamond/#compiling-from-source>`_ for installing diamond from source

Download the database you'd like to map to. As an example, let's download the SWISS-PROT database::
   
   curl -O ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
   gunzip uniprot_sprot.fasta.gz

Index the database::

   diamond makedb --in  uniprot_sprot.fasta -d  uniprot_sprot

Run Diamond::

   diamond  blastx -d uniprot_sprot -q MY_FASTA.fasta -a matches -t tempDir -p 16

Convert to blast tab-separated output format::

   diamond view -a matches.daa -o matches.m8
