# FSBthesis-modificirani

FSBthesis-modificirani je LaTeX stil koji je derivat originalnog FSBthesis stila (preuzeto s https://github.com/aerodyn/fsbthesis) te je prilagođen prema <a href="https://www.fsb.unizg.hr/index.php?fsbonline&diplomski_ispiti">službenom predlošku FSB-a</a> (stil Word dokumenta s navedene web stranice). 


## Lista sadržaja
  * [Instalacija paketa i korištenje lokalno](#instalacija-paketa-i-korištenje-lokalno)
  * [Postavke za korištenje s VS Code uređivačem koda](#postavke-za-korištenje-s-vs-code-uređivačem-koda)

<hr>

## Instalacija paketa i korištenje lokalno

Za instalaciju LaTeX-a pratiti upute na <a href="https://www.latex-project.org/get/">link-u</a>. Osim 
instalacije LaTeX-a, potrebno je klonirati ovaj repozitorij naredbom:         

`git clone https://github.com/Ginkbel/fsbthesis-modificirani.git`. 

Kada ste klonirali repozitorij, najlakše je uređivati Vaš rad pomoću datoteke `glavni.tex`. Za 
detaljnije upute i mogućnosti FSBthesis paketa vidjeti <a href="https://github.com/aerodyn/fsbthesis/blob/master/fsbthesis-doc.txt">dokumentaciju FSBthesis stila</a>.

Za korištenje FSBthesis-modificirani stila, potrebno je još konfigurirati VS Code (ili neki drugi TeX editor, no ovdje je samo objašnjeno konfiguriranje za VS Code).

## Postavke za korištenje s VS Code uređivačem koda

Najprije je potrebno instalirati <a href="https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop">LaTeX Workshop</a> ekstenziju za VS Code. Na linku se nalaze detaljne upute za instalaciju i korištenje ekstenzije. 

Za rad s ovim stilom, potrebno je definirati recepte za prevođenje `pdflatex -> bibtex -> pdflatex * 2` i `pdflatex ➞ makeindex ➞ pdflatex * 2`. 

Za dodavanje novih ili uređivanje postojećih recepta potrebno je otvoriti _Command Palette_ naredbom `Ctrl+Shift+P` (ili `Cmd+Shift+P` na _MacOS_) te je potrebno upisati `Preferences: Open User Settings (JSON)` i pritisnuti `Enter`. Nakon izvođenja naredbe, otvara se JSON datoteka `settings.json` gdje se mogu konfigurirati postavke VS Code-a za trenutnog korisnika računala. 

Potrebno je dodati ključu `"latex-workshop.latex.recipes"` vrijednosti (ukoliko je Vaša datoteka prazna, kopirajte kod iz [settings.json datoteka](#settingsjson-datoteka) te preskočite ostatak ovog poglavlja):

```
            "name": "pdflatex -> bibtex -> pdflatex * 2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
        {
            "name": "pdflatex ➞ makeindex ➞ pdflatex * 2",
            "tools": [
              "pdflatex",
              "makeindex",
              "pdflatex",
              "pdflatex"
            ]
        }
```

Kada su definirani recepti u `settings.json` datoteci, moguće je buildati projekt. Najbolja praksa je koristiti recept `pdflatex ➞ makeindex ➞ pdflatex * 2` za prvi build projekta (kada ste obrisali sve datoteke, uključujući i PDF, prethodnog builda) kako bi se napravio popis oznaka i kratica. Kada se izvrši build s ovim receptom, popis oznaka i kratica se nalazi u datoteci `glavni.nls`.

Svaki sljedeći build radi se s receptom `pdflatex -> bibtex -> pdflatex * 2`. Ako imate problema s redoslijedom popisa literature, obrišite sve datoteke koje se naprave tijekom buildanja (osim `glavni.nls` da zadržite popis oznaka) te ponovno napravite build.

Koristan trik za urednost radnog direktorija projekta je definiranje varijable `%OUTDIR%` unutar lokalne `settings.json` datoteke. 

Na kraju datoteke `settings.json` potrebno je dodati definiciju `%OUTDIR%` varijable: `"latex-workshop.latex.outDir": "./build"`.

Zatim je potrebno napraviti promjene vrijednosti u `"latex-workshop.latex.tools"` da se poklapaju sa sljedećim vrijednostima:

```
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-output-directory=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%OUTDIR%/%DOCFILE%",
            ],
            "env": {}
        },
        { 
            "name": "makeindex",
            "command": "makeindex",
            "args": [
                "%OUTDIR%/%DOCFILE%.nlo",
                "-s",
                "nomencl.ist",
                "-o",
                "%OUTDIR%/%DOCFILE%.nls",
            ]
        }
```

Na ovaj način datoteke koje su napravljane tijekom builda spremaju se u poseban direktorij `./build` te je bolja preglednost zbog manjeg broja mapa i datoteka u radnom direktoriju projekta.

## settings.json datoteka

Ukoliko je Vaša `settings.json` datoteka prazna, kopirajte sljedeće linije u Vašu datoteku te spremite:

```
{
    "explorer.confirmDelete": false,
    "latex-workshop.latex.autoBuild.run": "never",
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.latex.recipes": [
        {
            "name": "pdflatex -> bibtex -> pdflatex * 2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        },
        {
            "name": "pdflatex ➞ makeindex ➞ pdflatex * 2",
            "tools": [
              "pdflatex",
              "makeindex",
              "pdflatex",
              "pdflatex"
            ]
        },
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "latexmk (latexmkrc)",
            "tools": [
                "latexmk_rconly"
            ]
        },
        {
            "name": "latexmk (lualatex)",
            "tools": [
                "lualatexmk"
            ]
        },
        {
            "name": "latexmk (xelatex)",
            "tools": [
                "xelatexmk"
            ]
        },
        {
            "name": "Compile Rnw files",
            "tools": [
                "rnw2tex",
                "latexmk"
            ]
        },
        {
            "name": "Compile Jnw files",
            "tools": [
                "jnw2tex",
                "latexmk"
            ]
        },
        {
            "name": "Compile Pnw files",
            "tools": [
                "pnw2tex",
                "latexmk"
            ]
        },
        {
            "name": "tectonic",
            "tools": [
                "tectonic"
            ]
        }
    ],
    "git.confirmSync": false,
    "git.autofetch": true,
    "editor.minimap.enabled": false,
    "[latex]": {
        "editor.tabSize": 2
    },
    "editor.indentSize": "tabSize",
    "latex-workshop.latex.tools": [
        

        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "--shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "lualatexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-lualatex",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "xelatexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-xelatex",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "latexmk_rconly",
            "command": "latexmk",
            "args": [
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "--shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-output-directory=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%OUTDIR%/%DOCFILE%",
            ],
            "env": {}
        },
        {
            "name": "rnw2tex",
            "command": "Rscript",
            "args": [
                "-e",
                "knitr::opts_knit$set(concordance = TRUE); knitr::knit('%DOCFILE_EXT%')"
            ],
            "env": {}
        },
        {
            "name": "jnw2tex",
            "command": "julia",
            "args": [
                "-e",
                "using Weave; weave(\"%DOC_EXT%\", doctype=\"tex\")"
            ],
            "env": {}
        },
        {
            "name": "jnw2texminted",
            "command": "julia",
            "args": [
                "-e",
                "using Weave; weave(\"%DOC_EXT%\", doctype=\"texminted\")"
            ],
            "env": {}
        },
        {
            "name": "pnw2tex",
            "command": "pweave",
            "args": [
                "-f",
                "tex",
                "%DOC_EXT%"
            ],
            "env": {}
        },
        {
            "name": "pnw2texminted",
            "command": "pweave",
            "args": [
                "-f",
                "texminted",
                "%DOC_EXT%"
            ],
            "env": {}
        },
        {
            "name": "tectonic",
            "command": "tectonic",
            "args": [
                "--synctex",
                "--keep-logs",
                "%DOC%.tex"
            ],
            "env": {}
        },
        { 
            "name": "makeindex",
            "command": "makeindex",
            "args": [
                "%OUTDIR%/%DOCFILE%.nlo",
                "-s",
                "nomencl.ist",
                "-o",
                "%OUTDIR%/%DOCFILE%.nls",
            ]
        }
    ],
    "git.enableSmartCommit": true,
    "workbench.settings.applyToAllProfiles": [
    ],
    "latex-workshop.latex.outDir": "./build",
    "explorer.confirmDragAndDrop": false
}
```

Autor: Jurica Vučković
