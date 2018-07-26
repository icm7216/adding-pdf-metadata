# Compare file sizes with pdftk and pdfmark

About file size after adding metadata to PDF. Compare file sizes with pdftk and Ghostscript.

## Test sample Source

*   Calendar_2018.xlsx (Created Excel 2013)
*   Calendar_2018.pdf  (Export pdf quality = xlQualityStandard)

## Test environment

```
>ver
Microsoft Windows [Version 10.0.17134.191]

>pdftk -version
pdftk 2.02 a Handy Tool for Manipulating PDF Documents
Copyright (c) 2003-13 Steward and Lee, LLC - Please Visit: www.pdftk.com
This is free software; see the source code for copying conditions. There is
NO warranty, not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

>rungs -v
GPL Ghostscript 9.23 (2018-03-21)
Copyright (C) 2018 Artifex Software, Inc.  All rights reserved.
```

# adding PDF metadata using pdftk

pdftk.txt
```
InfoBegin
InfoKey: Title
InfoValue: Hello pdftk
InfoBegin
InfoKey: Author
InfoValue: PDF Author
InfoBegin
InfoKey: Subject
InfoValue: Calendar_2018
```

command
```
>pdftk Calendar_2018.pdf update_info_utf8 pdftk.txt output output_pdftk.pdf
```

With  compress option
```
>pdftk Calendar_2018.pdf update_info_utf8 pdftk.txt output output_pdftk.pdf compress
```
Note: The compression option is not good. That file size increased slightly.


# adding PDF metadata using Ghostscript

pdfmark.txt
```
[ /Title (Hello pdfmark with Ghostscript)
  /Author (PDF Author)
  /Subject (Calendar_2018)
  /DOCINFO pdfmark
```

command
```
rungs -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -dCompatibilityLevel=1.5 -dPDFSETTINGS=/default -dFastWebView=true -sOutputFile=output_pdfmark.pdf Calendar_2018.pdf pdfmark.txt
```
Note: "rungs" is Ghostscript command name of TeX Live 2018. Usually "gswin32c" or "gswin64c" are applicable.


# Results

| file name                |  size  |    Results|
|:------------------------|--------|--------------------|
|Calendar_2018.pdf        | 311,093| Original File size|
|output_pdftk.pdf         | 411,331| Increased file size|
|output_pdftk_compress.pdf| 413,487| Increased file size|
|output_pdfmark.pdf       | 169,487| Reduced file size|


In adding metadata, it was better to use Ghostscript Pdfmark.
Good for shrinking file size.
