---
layout: default
---

## JPEG EXIF Tag Viewer
### My First Assignment in C  
I completed this project as part of my courseload for Pitt's Introduction to System Software (CS 0449) class.  The assignment 
was to create a C utility to extract the metadata from a JPEG file. 
  
There were several outcomes from this project:
  * An increased understanding of the theory behind developing in the C Language
  * Experience working with the JPEG image format at the byte-level
  * Application of C to solve a problem and accomplish a defined task

### Example Output

```
$ gcc -o exifview exifview.c
$ ./exifview img2.jpg

Manufacturer:   Canon
Model:          Canon PowerShot SX100 IS
Exposure Time:  1/60 second
F-stop:         f/8.0
ISO:            ISO 100
Date Taken:     2008:02:20 18:52:03
Focal Length:   60  mm
Width:          360 pixels
Height:         240 pixels

```

After compilation through `gcc` the utility can be run with `./exifview image_argument.jpg`

You can view my code for this project [here](EXIF-Viewer-Code.html "EXIF Viewer Code")

All-in-all, this project was not horribly difficult; however, I did gain a better understanding of the C language.  The main challenges were in figuring out the exact layout of the TIFF tags and correctly gathering the EXIF information as a whole while using a programming language that was new to me.  These challenges were relatively easy to overcome through testing and reading documentation provided by my instructor.  
