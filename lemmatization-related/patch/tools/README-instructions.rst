Instructions for generating lemmatized versions of PPCHE2 release version of parsed files
=========================================================================================

==================
Download the patch
==================

Download the patch from github.  You will need to download the entire
ppche2-2025 repository.  The patch is in lemmatization-related/patch.

======
Backup
======

Before proceeding, make a copy of the release version.  You can think of
the copy as your backup and work on the original release files.  Or you
can work on the copy, treating the original release files as your
backup.  It doesn't matter, as long as you make a copy.

Come back when you've made your copy.

====================
Prepare parsed files
====================

The text in the parsed files has a different legal status than the
annotation.  For this reason, the script that patches the parsed files
is based on a version of those files that puts text and annotation on
separate lines.

On your end, this means that you need to put the parsed release files in
this same "verticalized" format, using the prep-files script that you
have just downloaded.  The following instructions assume the directory
structure in the release.::

  # modify path to data as needed
  cd /your/path/to/PPCHE2/data                                                                        
                                                                                                     
  # modify path to script as needed
  # if using tcsh shell, replace "prep-files" with "prep-files-for-tcsh"                             
  /your/path/to/patch-dir/tools/prep-files PPCEME/parsed/helsinki/*.psd                               
  /your/path/to/patch-dir/tools/prep-files PPCEME/parsed/penn1/*.psd                                  
  /your/path/to/patch-dir/tools/prep-files PPCEME/parsed/penn2/*.psd                                  
  /your/path/to/patch-dir/tools/prep-files PPCMBE2/parsed/*.psd                                       
                                                                                                     
  # sanity checks - assuming you're in /your/path/to/PPCHE2/data
  ls PPCEME/parsed/helsinki/*.psd.vert | wc -l  # should yield 146
  ls PPCEME/parsed/penn1/*.psd.vert | wc -l     # should yield 151                                        
  ls PPCEME/parsed/penn2/*.psd.vert | wc -l     # should yield 148                                        
  ls PPCMBE2/parsed/*.psd.vert | wc -l          # should yield 278                                        

===============
Apply the patch
===============

* Note how the structure of the lemmatization-related/data directory
corresponds to the directory structure of the release.  In particular,
where the release directory has "parsed", the patch directory has
"patching".

* Put the verticalized files that you just created in the previous step
(the input for the patching process) in the appropriate subdirectories
of the patching directory, whether by copying (as shown here) or moving
those files.::

  cd your/path/to/patch-dir/data                                                                     
                                                                                                     
  cp your/path/to/PPCHE2/data/PPCEME/parsed/helsinki/*.psd.vert PPCEME/patching/helsinki/.           
                                                                                                     
  cp your/path/to/PPCHE2/data/PPCEME/parsed/penn1/*.psd.vert PPCEME/patching/penn1/.                 
                                                                                                     
  cp your/path/to/PPCHE2/data/PPCEME/parsed/penn2/*.psd.vert PPCEME/patching/penn2/.                 
                                                                                                     
  cp your/path/to/PPCHE2/data/PPCMBE2/parsed/*.psd.vert PPCMBE2/patching/.                           

* Apply the patch with patch-lemmas.  If you're nervous, replacing the
"patch" with "patch -C" in the script allows you to do a dry run.  For
clarity, patch-lemmas changes the ".psd.vert" extension on the patched
file to ".lem".::

  cd your/path/to/patch-dir/data                                                                     
                                                                                                     
  # if using tcsh shell, replace "patch-lemmas" with "patch-lemmas-for-tcsh"                         
  your/path/to/patch-dir/tools/patch-lemmas PPCEME/patching/helsinki/*.psd.vert                      
  your/path/to/patch-dir/tools/patch-lemmas PPCEME/patching/penn1/*.psd.vert                         
  your/path/to/patch-dir/tools/patch-lemmas PPCEME/patching/penn2/*.psd.vert                         
  your/path/to/patch-dir/tools/patch-lemmas PPCMBE2/patching                                         
                                                                                                     
  # sanity check - the diffs at the end should turn up empty                                         
                                                                                                     
  cd your/path/to/patch-dir/data                                                                     
                                                                                                     
  cd PPCEME/patching/helsinki                                                                        
  lls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > your/path/to/patch-dir/tools/MY-helsinki   
                                                                                                     
  cd ../penn1                                                                                        
  # or equivalently: cd your/path/to/patch-dir/data/PPCEME/patching/penn1                            
  lls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > your/path/to/patch-dir/tools/MY-penn1      
                                                                                                     
  cd ../penn2                                                                                        
  # or equivalently: cd your/path/to/patch-dir/data/PCEME/patching/penn2                             
  lls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > your/path/to/patch-dir/tools/MY-penn2      
                                                                                                     
  cd ../../PPCMBE2/patching                                                                          
  # or equivalently: cd your/path/to/patch-dir/dataPPCMBE2/patching                                  
  lls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > your/path/to/patch-dir/tools/MY-mbe2       
                                                                                                     
  cd your/path/to/patch-dir/tools                                                                    
  diff REF-helsinki MY-helsinki                                                                         
  diff REF-penn1 MY-penn1                                                                               
  diff REF-penn2 MY-penn2                                                                               
  diff REF-mbe2 MY-mbe2                                                                                 

==========
Mopping up
==========

For additional clarity, rename the "patching" directories.::

  cd your/path/to/patch-dir/data                                                                     
                                                                                                     
  mv PPCEME/patching PPCEME/lemmatized                                                               
  mv PPCMBE2/patching PPCMBE2/lemmatized                                                             

If you wish, you can move these directories to make them sister
directories of the parsed and pos-tagged files in your copy of the
release.

Finally, running the lemmatized files through a reformatting query
(https://www.ling.upenn.edu/~beatrice/corpus-ling/CS-users-guide/command-file.html#reformat)
in CorpusSearch (or any CorpusSearch query, for that matter) will
reformat them to their standard format.

