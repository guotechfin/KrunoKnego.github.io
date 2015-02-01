---
layout: post
title: "Extraction and compression with TAR"
category: 
date: " 2015-01-25 13:00:31"
tags: [linux, tar]
---

## Introduction

TAR stands for tape archive. In linux/unix systems it's available as a command that can handle extraction and
compression of files and directories. TAR joins multiple files into one file. The compression or decompression for that
matter occurs only when you use either z or j flag. You can use only one type of compression.
If you decide to go with z flag then you'll be using GZIP and with j flag you'd be using BZIP2.
TAR compressed files with z flag typically have extension <code>.tar.gz</code> or it's short form <code>.tgz</code>.
Here you have complete list of short and long TAR suffixes:

| Short | Long    |
|:-----:|:-------:|
|  .tgz | .tar.gz |
|  .tbz | .tar.bz2|
|  .tz  | .tar.Z  |
|  .tlz | .tar.lz |
|  .txz | .tar.xz |

## Compression

If you wish to compress a file you'd do it like this:

    tar cvzf compressed_dir.tar.gz dir/

This command will compress contents of dir folder into compressed_dir.tar.gz file.
To clarify the meaning of flags look at the table below.


|  Flag |                 Meaning
|:-----:|:----------------------------------------:|
|   c   | Create new archive                       |
|   v   | Verbose output                           |
|   f   | Filename of the archive                  |
|   z   | Run through GZIP first                   |

## Extraction

Extracting files is fairly simple as well:

    tar xvzf compressed_dir.tar.gz

If you happen to encounter files with <code>.tar.bz2</code> extension you'd handle their decompression like this:

    tar xvjf compressed_dir.tar.bz2


|  Flag |                 Meaning
|:-----:|:----------------------------------------:|
|   x   | Create new archive                       |
|   v   | Verbose output                           |
|   f   | Filename of the archive                  |
|   z   | Run through GZIP first                   |
|   j   | Extract bzip2 files                      |

## Conclusion

TAR is only one way of extracting/compressing files in linux/unix systems. If you prefer using ZIP then I suggest
looking into zip command.


## Read More

* [tar wiki](http://en.wikipedia.org/wiki/Tar_%28computing%29)
* [tar man page](http://unixhelp.ed.ac.uk/CGI/man-cgi?tar)
