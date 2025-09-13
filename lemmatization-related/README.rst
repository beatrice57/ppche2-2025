Lemmatization-related information
=================================

Please report errors or address questions to beatrice DOT santorini AT
gmail DOT com.

===================
Lemmatization patch
===================

As of July 2025, a second release of the Penn Parsed Corpora of
Historical English (PPCHE2) is available from the Linguistic Data
Consortium (LDC) (https://catalog.ldc.upenn.edu/LDC2025T09).  A
lemmatized version of the Early Modern English and Modern British
English subcorpora exists, but is not part of the release.  The "patch"
directory contains scripts with instructions on how to generate the
lemmatized files from the release version of the parsed files.  
The
instructions cover only the patching process.  The lemmatization
guidelines are discussed in a section of the current annotation manual
(https://www.ling.upenn.edu/~beatrice/corpus-ling/annotation-2022/; see
also https://github.com/beatrice57/annotation-guidelines-for-ppche).

Use of the lemmatized versions of the texts in the PPCHE2 is governed by
the same conditions as the LDC release itself, namely by the Penn Parsed
Corpora of Historical English Second Release Agreement.

========================
Aggregate frequency data
========================

The "lemma-frequencies" directory provides aggregate lemma frequencies
for each subcorpus, tabulated and sorted as follows:

* By lemma only
* By lemma, then form as it appears in the text
* By form, then lemma

Those files are distributed under Creative Commons License
Attribution-NonCommercial-ShareAlike 4.0 International CC BY-NC-SA 4.0
(https://creativecommons.org/licenses/by-nc-sa/4.0/).

Conventions
-----------

* The column delimiter is (possibly multiple instances of) space.
* Forms are normalized with regard to case, with uppercase letters
  being replaced by and folded in with their lowercase counterparts.
* Forms are also normalized with regard to splitting indicated by "@",
  which is deleted.  See the annotation guidelines
  (<https://github.com/beatrice57/annotation-guidelines-for-ppche>) for
  details.
* Apostrophes are not modified.
* Entries are sorted alphabetically.  The files may be sorted by
  frequency in a suitable application or by using the Unix command
  "sort" with suitable switches.


