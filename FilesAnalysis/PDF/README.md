# PDF Analysis

## PDF Files

### Introduction

A PDF file is a **7-bit ASCII** file, except for certain elements that may have binary content. A PDF file starts with a header containing the magic number and the version of the format such as %PDF-1.7
The PDF language is based on the PostScript language which is a programming language, but PDF
is a subset of PostScript, without the features that make it a programming language.
It is important to note that the PDF language is a case-sensitive language.

PDFs are composed of the header, body, cross reference table and the trailer

#### Header

- header: Specify the PDF Version
  > %PDF-1.7

  &rarr; If not present, not valid
  &rarr; If different than 1.7, this may be suspicious as the standard has been published on November 2006

#### Body

Body: contains a series of objects.
Each object starts with **Object Number** **Revision Number** obj which defines the define a numbered top-level object.
Objects may be labeled so that they can be referred by other objects.
A labeled object is called an indirect object.
  &rarr; Here below 1 0 obj describe indirect object number 1 revion 0

- There is 8 types of objects
  - Boolean Values:
  - Integers and real numbers:
  - Strings
  - Names
  - Dictionaries
  - Streams: A sequence of bytes which correspond to embedded data, which can be compressed.
  - the null object.
  > 1 0 obj <br>
  > ...<br>
  > endobj<br>
  > 2 0 obj<br>
  > ...<br>
  > endobj<br>
  > ...<br>

Here below are risky tags/objects to look for:

- **/OpenAction** and **/AA** specify the script or action to run automatically.
- **/JavaScript** and **/JS** specify JavaScript to run.
- **/GoTo** changes the view to a specified destination within the PDF or in another PDF file.
- **/GoToE**
- **/GoToR** 
- **/Launch** can launch a program or open a document.
- **/URI** accesses a resource by its URL.
- **/SubmitForm** and **/GoToR** can send data to URL.
- **/RichMedia** can be used to embed Flash in a PDF.
- **/ObjStm** can hide objects inside an Object Stream.

#### Cross reference table

Specifies the position of the objects
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

#### Trailer

- Trailer Specifies information about where the document starts
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

Please note that when analysing **<<** and **>>** could be part of the payload and not *breaking* the *schema*

#### PDFiD

[PDFiD](https://blog.didierstevens.com/programs/pdf-tools/) scans a PDF file to look for certain PDF keywords may be link to suspicious behavior.
Keywords includes /Encrypt, /JS, /JavaScript ..

The goal of this tool is to:

- Have a quick idea if the PDF needs further analysis.
- Disarm the PDF, as PDF are case sensitive. By replacing /RichMedia to /RIchMEdiA for instance, this will allow you to open the PDF with your favorite PDF reader, within executing the embeded Flash in a PDF

```bash
pdfid.py suspicious.pdf.analysis
pdfid.py --disarm suspicious.pdf suspicious.pdf.analysis
```

- **/Page**: Gives an indication of the number of pages in the PDF document. Most malicious PDF document have only one page.
&rarr; If there is **only 1 page*, it's common to be suspicious
- **/Encrypt**: Indicates that the PDF document has DRM or needs a password to be read.
- **/ObjStm**: Counts the number of object streams.
- **/JS** and **/JavaScript**: Indicates that the PDF document contains JavaScript.
&rarr; Most malicious PDF documents contain JavaScript.
- **/AA** and **/OpenAction**: Indicates an automatic action to be performed when the page/document is viewed.
&rarr; Most malicious PDF documents with JavaScript have automatic action(s) to launch the JavaScript without user interaction.
- **/JBIG2Decode**: Indicates if the PDF document uses JBIG2 compression.
- **/RichMedia**: Indicates if the PDF document is for embedded Flash.
- **/ObjStm**: Can hide objects inside an Object Stream.

*Note that PDFiD has been implemented within VirusTotal, but sometimes, it is requires **TO NOT** submit files towards online security scanning solutions*

#### pdfparser

[pdfparser](https://blog.didierstevens.com/programs/pdf-tools/) is a Python program I develop to analyze PDF files.

pdf-parser reads the PDF file sequentially from start to end.
This way, it can uncover indirect objects or other PDF structures that are hidden inside the file (i.e.
not referenced in the logical structure).

```bash
pdf-parser.py --raw -O malware1.pdf.vir                # output raw data
pdf-parser.py --search OpenAction -O malware1.pdf.vir  # 
pdf-parser.py --search URI -O malware1.pdf.vir         # Search for URI indirect objects 
pdf-parser.py --reference 31 -O malware1.pdf.vir
pdf-parser.py --object 34 -O malware1.pdf.vir
pdf-parser.py --object 34 -O --filter malware1.pdf.vir
pdf-parser.py --object 34 -O --filter --raw malware1.pdf.vir
pdf-parser.py -a -O malware1.pdf
```

Options:

- **--search**:
- **--reference**:
- **--raw**:
- **-O**: Extracts objects from /ObjStm streams and handles them like normal objects.
- **-k**: 
- **-o**: Select object number °

##### OpenAction

**/OpenAction** is usually followed by **/S** which specify the type of action

In the [PDF32000_2008.pdf](https://www.adobe.com/content/dam/acom/en/devnet/pdf/pdfs/PDF32000_2008.pdf), there is a list of possible values for action types

Let's take the **/Launch** action, a *launch action* which can either:

- Launch an application
- Open a document
- Print a document

A launch action action has the following parameter:

- **/F (Required)**: The file name of the application to be launched or the document to be opened or printed.
- **/D** (Optional): The default directory in standard DOS syntax.
- **/O** (Optional): The operation to perform, either *open* or *print*
- **/P** (Optional): Parameter passed to **/F**

You may also see **/Win**, **/Mac** or **/Unix** to specify an O.S. specific action.

### pdftotext

You may need to search for some content, for example, there could be URL that are written as text and not within an URI object.
In order to do that, I do the following

```bash
pdftotext malware1.pdf.vir malware1.txt.vir
grep 'http\|www' malware1.txt.vir
```

#### Hex Editor

Thanks to:

- @Didier
  - https://blog.didierstevens.com/2008/04/09/quickpost-about-the-physical-and-logical-structure-of-pdf-files/
  - https://blog.didierstevens.com/2010/09/26/free-malicious-pdf-analysis-e-book/
- @0xdf
  - https://0xdf.gitlab.io/2018/09/12/malware-analysis-yourexploitpdf.html

Resources to do:
- https://github.com/zbetcheckin/PDF_analysis/blob/master/README.md#pdf-format-page_facing_up