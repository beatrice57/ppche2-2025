Instructions for generating lemmatized versions of PPCHE2 release version of parsed files
=========================================================================================

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

  # edit path to release data as needed
  cd /your/path/to/release/data

  # edit path to script as needed
  # if using tcsh shell, replace "prep-files" with "prep-files-for-tcsh"

  /your/path/to/patch/tools/prep-files PPCEME/parsed/helsinki/*.psd

  /your/path/to/patch/tools/prep-files PPCEME/parsed/penn1/*.psd

  /your/path/to/patch/tools/prep-files PPCEME/parsed/penn2/*.psd

  /your/path/to/patch/tools/prep-files PPCMBE2/parsed/*.psd

  # sanity check - assuming you're in /your/path/to/release/data
  ls PPCEME/parsed/helsinki/*.psd.vert | wc -l  # should yield 146
  ls PPCEME/parsed/penn1/*.psd.vert | wc -l     # should yield 151
  ls PPCEME/parsed/penn2/*.psd.vert | wc -l     # should yield 148
  ls PPCMBE2/parsed/*.psd.vert | wc -l          # should yield 278

===============
Apply the patch
===============

* The structure of the lemmatization-related/data directory corresponds
to the directory structure of the release.  In particular, where the
release directory has "parsed", the patch directory has "patching".

* Put the .psd.vert files that you just created (the input to the
patching process) in the appropriate subdirectories of the patch
directory, whether by copying (as shown here) or moving those files.::

  # edit patch to patch and release directories as needed
  cd /your/path/to/patch/data

  cp /your/path/to/release/data/PPCEME/parsed/helsinki/*.psd.vert PPCEME/patching/helsinki/.

  cp /your/path/to/release/data/PPCEME/parsed/penn1/*.psd.vert PPCEME/patching/penn1/.

  cp /your/path/to/release/data/PPCEME/parsed/penn2/*.psd.vert PPCEME/patching/penn2/.

  cp /your/path/to/release/data/PPCMBE2/parsed/*.psd.vert PPCMBE2/patching/.

* Apply the patch with patch-lemmas.  Replacing the "patch" with "patch
-C" in the script allows you to do a dry run.  For clarity, patch-lemmas
changes the ".psd.vert" extension on the patched file to ".lem".::

  # edit path to patch and tools directories as needed
  cd /your/path/to/patch/data

  # if using tcsh shell, replace "patch-lemmas" with "patch-lemmas-for-tcsh"

  cd PPCEME/patching/helsinki
  /your/path/to/patch/tools/patch-lemmas *.psd.vert

  cd ../penn1
  # or equivalently: cd /your/path/to/patch/data/PPCEME/patching/penn1
  /your/path/to/patch/tools/patch-lemmas *.psd.vert

  cd ../penn2
  # or equivalently: cd /your/path/to/patch/data/PPCEME/patching/penn2
  /your/path/to/patch/tools/patch-lemmas *.psd.vert

  cd ../../PPCMBE2/patching
  # or equivalently: cd /your/path/to/patch/dataPPCMBE2/patching
  /your/path/to/patch/tools/patch-lemmas *.psd.vert

  # sanity check - the diff at the end should turn up empty

  cd /your/path/to/patch/data

  cd PPCEME/patching/helsinki
  # or equivalently: cd /your/path/to/patch/data/PPCEME/patching/helsinki
  ls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > /your/path/to/patch/tools/MY-helsinki

  cd ../penn1
  # or equivalently: cd /your/path/to/patch/data/PPCEME/patching/penn1
  ls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > /your/path/to/patch/tools/MY-penn1

  cd ../penn2
  # or equivalently: cd /your/path/to/patch/data/PPCEME/patching/penn2
  ls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > /your/path/to/patch/tools/MY-penn2

  cd ../../PPCMBE2/patching
  # or equivalently: cd /your/path/to/patch/data/PPCMBE2/patching
  ls -l *.lem | tr -s '[:blank:]' ' ' | cut -d ' ' -f 5,9 > /your/path/to/patch/tools/MY-mbe2

  cd /your/path/to/patch/tools
  diff REF-helsinki MY-helsinki
  diff REF-penn1 MY-penn1
  diff REF-penn2 MY-penn2
  diff REF-mbe2 MY-mbe2

===================
Further suggestions
===================

* For additional clarity, you can rename the "patching" directories.::

  cd /your/path/to/patch/data

  mv PPCEME/patching PPCEME/lemmatized
  mv PPCMBE2/patching PPCMBE2/lemmatized

* You can make the directories with the lemmatized files into sister
directories of the parsed and POS-tagged directories in your copy of the
release.

* Running the lemmmatized files through a reformatting query
(https://www.ling.upenn.edu/~beatrice/corpus-ling/CS-users-guide/command-file.html#reformat)
in CorpusSearch (or any CorpusSearch qu
reformat them to their standard format.

