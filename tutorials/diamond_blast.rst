Blasting with DIAMOND 
===================================

Make a new directory for diamond::
   
   cd
   mkdir diamond
   cd diamond

On Linux: download diamond::
   
   wget http://github.com/bbuchfink/diamond/releases/download/v0.7.9/diamond-linux64.tar.gz
   tar xzf diamond-linux64.tar.gz

Put diamond in your path::
   
   export PATH=$PATH:$HOME/diamond
   echo 'export PATH=$PATH:$HOME/diamond' >> ~/.bashrc
   
On Mac (with homebrew)::

    brew install homebrew/science/diamond

Download the swissprot database::
   
   wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
   gunzip uniprot_sprot.fasta.gz

Index the database::

   diamond makedb --in  uniprot_sprot.fasta -d  uniprot_sprot

Run Diamond::

   diamond  blastx -d uniprot_sprot -q MY_FASTA.fasta -a matches -t tempDir -p 16

Convert to blast tab-separated output format::

   diamond view -a matches.daa -o matches.m8
