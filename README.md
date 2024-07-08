# myIEEEtran-bst

Updated IEEEtran BibTeX style file including URL accessed dates for references, adapted for KTH Royal Institute of Technology master thesis 2024.

## Description

This repository contains the `myIEEEtran.bst` file, an adaptation of the IEEEtran BibTeX style file. It includes support for DOI, ISSN, ISBN, and now URL accessed dates, as required by the IEEE referencing style for online documents. This is particularly useful for master's theses at KTH Royal Institute of Technology in 2024 and other similar IEEEtran BibTeX files.

## Installation

To use this updated BibTeX style file in your LaTeX documents, follow these steps:

1. Download the `myIEEEtran.bst` file from this repository.
2. Place the file in your LaTeX project directory.


## Adding Accessed Date to IEEEtran BibTeX Style your self

The IEEE referencing style requires showing the date of access for online documents. This adaptation incorporates changes made by Michael Tyson to the Harvard BibTeX style and applies them to the IEEE style, based on instructions from [Owen Stephens](https://www.owenstephens.co.uk/blog/2010/03/28/add_accessed_date_to_ieeetran_bibtex_style.html).

### Instructions

To modify the IEEEtran BibTeX style file to include accessed dates:

1. **Locate the `IEEEtran.bst` File:**
   The location depends on your installation, but it is typically found in the `bibtex/bst/IEEEtran/` directory.

2. **Modify the `ENTRY FIELDS`:**
   - Find the section starting with:
     ```latex
     %%%%%%%%%%%%%%%%%%
     %% ENTRY FIELDS %%
     %%%%%%%%%%%%%%%%%%
     ```
   - Look for `url` and add a new attribute `urldate` beneath it.
     ```latex
     ENTRY
       { address
         assignee
         author
         ...
         url
         urldate
         ...
       }
     ```

3. **Update the `format.url` Function:**
   - Locate the `format.url` function:
     ```latex
     FUNCTION {format.url}
     { url empty$
         { "" }
         { this.to.prev.status
           this.status.std
           cap.yes 'status.cap :=
           name.url.prefix " " *
           "\url{" * url * "}" *
           punct.no 'this.status.punct :=
           punct.period 'prev.status.punct :=
           space.normal 'this.status.space :=
           space.normal 'prev.status.space :=
           quote.no 'this.status.quote :=
         }
       if$
     }
     ```
   - Modify it to include `urldate`:
     ```latex
     FUNCTION {format.url}
     { url empty$
         { "" }
         { this.to.prev.status
           this.status.std
           cap.yes 'status.cap :=
           name.url.prefix " " *
           "\url{" * url * "}" *
           punct.no 'this.status.punct :=
           punct.period 'prev.status.punct :=
           space.normal 'this.status.space :=
           space.normal 'prev.status.space :=
           quote.no 'this.status.quote :=
           urldate empty$
             { "there is url but no urldate in " cite$ * warning$ }
             { " [Accessed: " * urldate * "]" * }
           if$
         }
       if$
     }
     ```

### Testing the Changes

To verify the modifications, use the following example files:

1. **Create `example.tex`:**
   ```latex
   \documentclass{article}
   \begin{document}
   Hello world! \cite{wadler1992comprehending}
   \bibliographystyle{myIEEEtran}
   \bibliography{example}
   \end{document}
   ```

2. **Create `example.bib`:**
   ```bibtex
   @article{wadler1992comprehending,
     title={Comprehending monads},
     author={Wadler, P.},
     journal={Mathematical Structures in Computer Science},
     volume={2},
     number={04},
     pages={461--493},
     year={1992},
     publisher={Cambridge Univ Press},
     url={http://dx.doi.org/10.1017/S0960129500001560},
     urldate={04/12/2012}
   }
   ```

3. **Compile the Document:**
   ```bash
   pdflatex example.tex
   bibtex example
   pdflatex example.tex
   pdflatex example.tex
   ```

To see the new warning, delete the `urldate` line from the `.bib` file and recompile.



Keywords: latex, bib, URL, accessed date, IEEEtran, myIEEEtran, BibTeX, LaTeX, URL Accessed Date, Master's Thesis, KTH Royal Institute of Technology,2024.
