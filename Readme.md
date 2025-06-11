# Readme

This is a Latex package loading some basic Latex packages and setting basic options I am using for writing my (mathematical) writing.

Simply put the file ulthiel.sty in a folder together with your Latex document and load this package like any other package:

```latex
\documentclass{article} % or amsart 

\usepackage[OPTIONS]{ulthiel}

\begin{document}

Hello.

\end{document}
```

The basic packages that are loaded (in this order) are: inputenc with utf8, mmap, fontenc, babel, amssumb, mathrsfs, mathtools, url, tikz-cd, microtype, todonotes, amsthm, hyperref, aliascnt, csquotes. 

I had some initial issues with this because the ordering of some of the packages is important. 

I have added a command `\Autoref{}` which for Theorems, Lemmas, etc. automatically includes this prefix, e.g. if `theo` is some label of a theorem environment then `\Autoref{theo}` typesets `Theorem \ref{theo}`. I find this useful if one later on decides that a Lemma is a Proposition etc. This avoids re-editing.

I have added a command to nicely typeset categories: `\cat{mod}{A}{B}` typsets A-B-Bimodules, `\cat{mod}{A}` is left A-modules, and `\cat{mod}{}{B}` is right B-modules.

Bibtex will use the file references.bib but you can add your own files using `\addbibresource{myfile.bib}`. I recommend to use one reference file per document and keep this in the document folder to ensure that everything will compile fine also in the future.

This package compiles fine with the arXiv (add the ulthiel.sty as additional file when uploading the document, also add the .bbl file one gets from Latex; if you follow my workflow instructions below, this file will be in the subfolder .tmp).

The following options can be set as package options:

* bibtex: load bibtex
* apa: sets APA reference style (needs a "recent" TexLive: 2023)
* german: sets babel language to ngerman
* sasserif: selects a sansserif font
* julia: adds code to nicely typeset Julia code environments

## Latex Workflow

As editor I am using the free and open source [VSCodium](https://vscodium.com/), which is basically like Visual Studio Code but without the Microsoft ingredients. Furthermore, I am using the following extensions which can be added with Settings > Extensions:

* LatexWorkshop (by James-Yu) for automatic compiling
* LTeX+ (by ltex-plus) for spell checking

LatexWorkshop uses latexmk for compiling documents, so make sure you have this (it should come with your Latex distribution usually). An additional thing I am doing to avoid having my directory cluttered with the auxiliary Latex files is to compile into the subfolder `.tmp` and then move the resulting pdf back to the main folder. Here is how to do this.

1. Go to Settings and search for "latexmk". Set "Out Dir" to “.tmp”. 

2. Open the User Settings “settings.json” by hitting Ctrl+Shift+P (Cmd+Shift+P on Mac) and typing "Open User Settings".

3. Add the following code into the block `latex-workshop.latex.tools`:
   ```json
   "latex-workshop.latex.tools": [
     {
               "name": "pdf copy",
               "command": "cp",
               "args": ["%OUTDIR%/%DOCFILE%.pdf", "%DIR%/"]
     }
   ]
   
   ```

4. Add `pdf copy` to the `latexmk` block in `latex-workshop.latex.recipes`:
   ```json
   "latex-workshop.latex.recipes": [
   {
               "name": "latexmk",
               "tools": [
                   "latexmk",
                   "pdf copy"
               ]
           },
   ]
   ```

The spell check with LTeX looks for the language set with babel. If you write a document in German, add `\selectlanguage{ngerman}` at the beginning of the document. A manual dictionary can be maintained in the block `ltex.dictionary` in the settings.json. I have added my dictionary to this repository.