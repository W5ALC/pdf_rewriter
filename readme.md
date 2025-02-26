# Background

Depending on the pdf creator/engine used, a `.pdf` file may include
content irrelevant for reading by a human. An example is the inclusion
of complete sets of fonts though only a few glyphs are (or only one is)
used on the "electronic paper". To print the `.pdf` with `ghostscript`
again into a `.pdf` may reduce the file size.

By far, this bash script *does not* claim to be the first one collecting
bits and bolts to address the issue. It rather serves as an aide-memoire
of finds encountered earlier, and to moderate `ghostscript` in Linux
accordingly. Within reason, the snippets were joined as provided; thus,
the credit belongs to those already in the field.

# Intended Use

The most comfortable use is 1) the provision of the executable bit
(`chmod +x pdf_reprint.sh`), and 2) to set an alias in your `.bashrc`
file. Then, its functionality described below is available all across
your system.

-   Else, to reprint the `.pdf` while retaining the color, run either
    one of the following commands

    ``` shell
    bash ./pdf_rewrite.sh --reprint input.pdf
    bash ./pdf_rewrite.sh -r input.pdf
    bash ./pdf_rewrite.sh --colour input.pdf
    bash ./pdf_rewrite.sh --color input.pdf
    bash ./pdf_rewrite.sh -c input.pdf
    ```

    *to replace* the original file `input.pdf` by a new version of
    smaller file size. There will be a short note if the attempt was
    successful; and if so, the improvement compared to the original
    `input.pdf` is reported (percentage). Though you may process the
    file multiple times with this method, savings in file size often
    quickly converge to be insignificant in comparison to the remaining
    file size.

    The credit for the underlying approach and implementation belongs to
    Evan Langlois.[^1]

-   Often, an additional reduction of file size may be obtained by
    reprinting the `.pdf` in gray-scale only. Either one of the
    following commands to the script is equivalent

    ``` shell
    bash ./pdf_rewrite.sh --gray input.pdf
    bash ./pdf_rewrite.sh --grey input.pdf
    bash ./pdf_rewrite.sh -g input.pdf
    ```

    to replace file `input.pdf` by its rewritten form. The credit for
    this approach belongs to user `slm` on the Unix stackexchange.[^2]

-   To process multiple `.pdf`, a for-loop in your shell could follow
    the pattern of

    ``` shell
    for file in *.pdf
    do
        echo "$file"
        bash ./pdf_rewrite.sh -r "$file"
    done
    ```

    This equally provides you a brief progress report, too.

Note, this script's primary aim is to obtain a file of small file size,
e.g., as an attachment of an email while retaining the text easy to read
and – if present – to retain a text layer searchable. Especially the
reprint in half-tones however may render illustrations less
intelligible. It is up to the creators of figures to use easy
discernible markers in diagrams, as well as to use color scales suitable
for the color blind, and safe for this mimicked "photocopying". For the
later, a service like <https://colorbrewer2.org/> may guide your
selection.

Keep a backup of the .pdf to be processed. Though the script may report
problems while processing the data (or even crash, which may destroy the
.pdf), it *is not* a PDF validator such as e.g., veraPDF.[^3]

# Benchmark

Initially written for Linux Xubuntu 18.04.3 LTS and ghostscript
(version 9.26), the archive includes two .pdf files to check the scripts
action e.g., with Debian 12/bookworm (currently *testing*) and
ghostscript (version 10.00.0).

-   File `link2web.pdf` was compiled with pdfLaTeX based on an example
    provided by
    [www.texample.net](http://www.texample.net/tikz/examples/lune-of-hippocrates/).
    This .pdf contains a color figure and link to an external reference.
    Note, the simplification into half-tones (option `-g`) affects the
    document printed, depending on the pdf viewer used, the box around
    the link may remain colored *for the display on screen*.

-   File Kletskov2020.pdf is a contemporay research article[^4]
    including text, colour line art, and photographs which was published
    under a permissive Creative Commons license.

In the table below, *savings* computes the difference of the file size
prior and after the processing with either option, then reports this
change as percentage in respect to the original file size.

|                  |               |              |         |              |         |
|------------------|---------------|--------------|---------|--------------|---------|
| file name        | original size | by flag `-r` | savings | by flag `-g` | savings |
|                  | \[kiB\]       | \[kiB\]      | \[%\]   | \[kiB\]      | \[%\]   |
| link2web.pdf     | 38.0          | 9.8          | 74.2    | 9.8          | 74.2    |
| Kletskov2020.pdf | 3405          | 2288         | 32.8    | 658          | 80.7    |

# Footnotes

[^1]: <https://tex.stackexchange.com/questions/18987/how-to-make-the-pdfs-produced-by-pdflatex-smaller?rq=1>

[^2]: <https://unix.stackexchange.com/questions/93959/how-to-convert-a-color-pdf-to-black-white>

[^3]: <https://openpreservation.org/tools/verapdf/>

[^4]: "Isothiazoles in the Design and Synthesis of Biologically Active
    Substances and Ligands for Metal Complexes", Kletskov, A. V.;
    Bumagin, N. A.; Zubkov, F. I.; Grudini, D. G.; Potkin, V. I.
    *Synthesis*, **2020**, *52*, 159–188, [doi
    10.1055/s-0039-1690688](https://www.thieme-connect.de/products/ejournals/abstract/10.1055/s-0039-1690688).
