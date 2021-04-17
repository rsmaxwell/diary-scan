# diary-scan

----
## what are the JSR diaries?
see [JSR Diaries](https://server.rsmaxwell.co.uk/diaries/)


----
## Introduction

Tools to allow transcripts of diaries to be built into a set of HTML documents.


----
## Description

A diary is composed from a set of html fragments, some of which are generated
and others are manually written

There is usually (but not always) one manually written fragment per day

Image files are stored outside the project directory, the location of the image directory 
is stored in a ${HOME}/diary.json

Example of the ${HOME}/diary.json file
``` json
{
  "root": "/cygdrive/z",
  "path": "/nancy-and-ronald-maxwell/documents/sea-captains-chest",
  "diaries": [
    "diary-1832"
  ],
  "year": 1832
}
```

When building a diary:

  - The templates from the "input/templates" directory are used to 
  generate fragments (e.g. the titles of year and month).

  - The generated fragments are added to the manually written fragments 
  in the generated from the templated defined in the "input/fragments" directory.

  - The fragments are sorted by: "year/month/day/order" and written out 
  to generate the html diary document

A fragment directory contains:
  - "fragment.html" which is the html fragment to be inserted into the diary 
  - "fragment.json" which contain metadata about the fragment

Example of "fragment.json"

``` json
{
  "year": 1828,  
  "month": 6,
  "day": 26,
  "order": "img2849-left",    # A string used to order fragments withing the same day
  "imageFilename": "img2849.jpg",  # The original image of the actual diary
  "diary": "diary-1828-and-1829-and-jan-1830", # The name of the actual diary
  "images": [] # list of images used in this fragment
}
```

A template directory contains:
  - "fragment.html" which is the html fragment to be inserted into the diary 
  - "fragment.json" which contain metadata about the fragment
  - "template.json" which contain metadata about the template

Example of "template.json"

``` json
{
  "classname": "com.rsmaxwell.diaryjson.template.YearHeader"
}
```

The build process uses this class to determine when to use this template 
to generate a new Fragment. This the case of "YearHeader" this is done 
when the the year changes.

The template classname identifies a class implementing the 
"com.rsmaxwell.diaryjson.template" interface

----
## Convert a Word page into a set of Fragments

Open a cygwin window and make this project the current directory

```
cd /cygdrive/c/Users/Richard/git/github.com/rsmaxwell/diary-scan
```

Set the name of the diary.

```
setDiary diary-1835
```

Convert the Word page document into fragments

```
./extractor img2648-right.docx
```

This will point the input to read ${root}/${path}/${dairy}/img2648-right.docx
i.e.

```
/cygdrive/z/nancy-and-ronald-maxwell/documents/sea-captains-chest/diary-1835/metadata/word/img2648-right.docx
```

And output fragments to the directory

```
/cygdrive/z/Users/Richard/git/github.com/rsmaxwell/diary-scan/input/fragments/1835
```

----
## Edit a fragment

Open a cygwin window and make this project the current directory

```
cd /cygdrive/c/Users/Richard/git/github.com/rsmaxwell/diary-scan
```

Make the Fragment standalone by building a wrapper around it called "document.html"

```
./buildFragment 1835/01/05-img2648-right
```

Open "document.html" in Word, edit then "Save as" a .docx
Extract the Fragment from "document.docx" 

```
./wordConverter 1835/01/05-img2648-right
```

Clean the Fragment directory leaving just the "fragment.html" and "fragment.json"

```
./cleanFragment 1835/01/05-img2648-right
```




