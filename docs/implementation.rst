###########################
Usage & Code Implementation
###########################

Input Files
===========


.. _clapi:
Command-Line API
================

Again, the command-line API is heavily inspired by the SExtractor and SCAMP ones.

-a            command-line option "a"
-b file       options can have arguments
              and long descriptions
--long        options can be long also
--input=file  long options can also have
              arguments
/V            DOS/VMS-style options too

SExtractor and SCAMP wrapper
============================


Filter Chain
============

Output Files
============

::

  target directory
  │
  └───cats
  │      exposure1.cat
  │      exposure1.head
  │      exposure1.ahead  *optional*
  │      exposure2.cat
  │      exposure2.head
  │      exposure2.ahead  *optional*
  │      ..
  │      full_1.cat
  │      merged_1.cat
  │      scamp.xml
  │      ssos.csv
  │
  └───cutouts
  │      SOURCE1_CATALOG1.fits
  │      SOURCE1_CATALOG2.fits
  │      SOURCE2_CATALOG1.fits
  │      ..
  │
  └───logs
  │      sso_$DATETIME.log
  │
  └───skybot
  │      skybot_query_string1.xml
  │      skybot_query_string2.xml
  │      ..
  │
  └───weights
  │      exposure1_weight.fits  *optional*
  │      expsoure2_weight.fits  *optional*
  │      ..
