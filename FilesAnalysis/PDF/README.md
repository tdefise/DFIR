# PDF Analysis

## PDF Files

### Introduction

A PDF file is a **7-bit ASCII** file, except for certain elements that may have binary content. A PDF file starts with a header containing the magic number and the version of the format such as %PDF-1.7
The PDF language is based on the PostScript language which is a programming language, but PDF
is a subset of PostScript, without the features that make it a programming language.
It is important to note that the PDF language is a case-sensitive language.


PDFs are composed of:
- header: Specify the PDF Version
  > %PDF-1.7

  -> If not present, not valid
- Body: contains a series of objects.
  - Each object starts with **Object Number** **Revision Number** obj which defines the define a numbered top-level object.
  Objects may be labeled so that they can be referred by other objects.
  A labeled object is called an indirect object.
  -> Here below 1 0 obj describe indirect object number 1 revion 0
  - There is 8 types of objects
    - Boolean Values:
    - Integers and real numbers:
    - Strings
    - Names
    - Dictionaries
    - Streams: A sequence of bytes which correspond to embedded data, which can be compressed.
    - the null object.

<!--- This is in work in progress
  
  The most interesting are **name objects**.
  A name object is an atomc symbil uniquely defined by a sequence of characters.
  A slash (/d) introduces a name object.
  Here are some interesting ones:
  - **/AA** (Within the Root element):
  - **/OpenAction** (Within the Root element):
  - **/URI** (Within the Root element):
  - **/JavaScript** (Within the Name element)
  - **/URLS** (Within the Name element)



ActionType:
/SubmitForm
/GoTo
/GoToE
/GoToR
/URI

/ObjStm


Here below are risky tags:

- /OpenAction and /AA specify the script or action to run automatically.
- /JavaScript and /JS specify JavaScript to run.
- /GoTo changes the view to a specified destination within the PDF or in another PDF file.
- /Launch can launch a program or open a document.
- /URI accesses a resource by its URL.
- /SubmitForm and /GoToR can send data to URL.
- /RichMedia can be used to embed Flash in a PDF.
- /ObjStm can hide objects inside an Object Stream.





/Page gives an indication of the number of pages in the PDF document.
-> It is common to see malware that are only 1 page.

/JS and /JavaScript, and /RichMedia indicate that the PDF document contains JavaScript
respectively Flash.

/AA, /OpenAction and /AcroForm indicate an automatic action to be performed when the
page/document is viewed. This is often used to execute JavaScript without user interaction.
 -->


  > 1 0 obj <br>
  > ...<br>
  > endobj<br>
  > 2 0 obj<br>
  > ...<br>
  > endobj<br>
  > ...<br>
  
- Cross reference table: Specifies the position of the objects
  This is the **object offsets**. The cross-reference table allow to access any given object by its number easily, and fast.
  - It starts with **xref**
  - Then two numbers which indicates the numbers of object it contains starting from a given offset
  > xref<br>
  > 0 6<br>
  > 0000000000 65535 f
  > 0000000010 00000 n<br> 
  > 0000000079 00000 n<br>
  > 0000000173 00000 n<br>
  > 0000000301 00000 n<br>
  > 0000000380 00000 n<br>

- Trailer: Specifies information about where the document starts
  - It contains the following entries:
    - **/Root**: Root element of the tree structure of the objects that are within the body
  > trailer<br>
  > <<<br>
  > /Size 6<br>
  >  /Root 1 0 R<br>
  > \>><br>
  > startxref<br>
  > 492<br>
  > %%EOF<br>


### Analysis

#### PDFiD

[PDFiD]() scans a PDF file to look for certain PDF keywords may be link to suspicious behavior.
Keywords includes /Encrypt, /JS, /JavaScript ..

The goal of this tool is to :
- Have a quick idea if the PDF needs further analysis.
- Disarm the PDF, as PDF are case sensitive. By replacing /RichMedia to /RIchMEdiA for instance, this will allow you to open the PDF with your favorite PDF reader, within executing the embeded Flash in a PDF

```bash
pdfid.py suspicious.pdf.analysis
pdfid.py --disarm suspicious.pdf suspicious.pdf.analysis
```

*Note that PDFiD has been implemented within VirusTotal, but sometimes, it is requires **TO NOT** submit files towards online security scanning solutions*

#### pdfparser

[pdfparser]() is a Python program I develop to analyze PDF files. 

pdf-parser reads the PDF file sequentially from start to end.
This way, it can uncover indirect objects or other PDF structures that are hidden inside the file (i.e.
not referenced in the logical structure).

```bash
pdf-parser.py --search javascript malware1.pdf.vir
pdf-parser.py --reference 31 malware1.pdf.vir
pdf-parser.py --object 34 malware1.pdf.vir
pdf-parser.py --object 34 --filter malware1.pdf.vir
pdf-parser.py --object 34 --filter --raw malware1.pdf.vir
```

#### Hex Editor



Thanks to:

- @Didier
  - https://blog.didierstevens.com/2008/04/09/quickpost-about-the-physical-and-logical-structure-of-pdf-files/
  - https://blog.didierstevens.com/2010/09/26/free-malicious-pdf-analysis-e-book/

Resources to do:
- https://github.com/zbetcheckin/PDF_analysis/blob/master/README.md#pdf-format-page_facing_up