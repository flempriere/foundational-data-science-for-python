# Chapter 1: Learning Python in a Notebook Environment


- [Notes](#notes)
  - [Running Python Statements](#running-python-statements)
  - [Jupyter Notebooks](#jupyter-notebooks)
    - [Hosting Services](#hosting-services)
    - [LaTeX](#latex)
    - [Google Colab](#google-colab)
      - [Colab Files](#colab-files)
      - [Code Snippets](#code-snippets)
    - [Shell Commands](#shell-commands)
    - [Magic Functions](#magic-functions)
- [Summary](#summary)
- [Questions](#questions)

## Notes

### Running Python Statements

- Python is an interpreted language
  - Can be run by reading a script
  - Or through the interactive REPL
- Typically accessed via, `python` or `python3`
  - To see what version of python you have installed try the following

``` shell
$ python3 --version

Python 3.14.3
```

- Invoking python without passing a script will instantiate the REPL

``` shell
$ python3

Python 3.14.3 (main, Feb  3 2026, 22:52:18) [Clang 21.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("Hello")
Hello
```

- Invoking python and passing a script (typically denoted by the
  extension `.py`) will execute the file

``` shell
$ python3 hello.py
Hello
```

- The traditional development flow is

  1.  Experiment in the REPL
  2.  Write software and transition to a file-based program

- A newer intermediate is to use an *interactive notebook*

  - Useful to be able to have the interactivity and instant response of
    the REPL
  - But, also want to be able to group code and keep the reproducibility
    of a file system

### Jupyter Notebooks

- Jupyter notebooks are a creation of the [IPython
  Project](https://ipython.org/)

> [!NOTE]
>
> **Jupyter**
>
> The name Jupyter is a derived from the languages, **Ju**lia,
> **Py**thon, **R**. The three languages originally supported by the
> project.

- Notebooks are comprised of individual executable cells, cells can be

  1.  Text
      - Written using the [markdown
        language](https://www.markdownguide.org/)

      - Executing a text cell will *render* the markdown

        ``` markdown
          **Bold**

          _italic_

          *also italic*

          ~strikethrough~

          `monospace`

          # Heading

          ## Sub-heading

          ### sub-sub-heading

          [A link](https://colab.research.google.com/notebooks/markdown_guide.ipynb)

          - Unordered list

          1. Ordered list
        ```
  2.  Code Cells
      - Executable cells of python code
      - Can be comprised of multiple lines
      - State is maintained between executions of cells

- Notebooks allow code to be built up *cell* by *cell*

  - Can rerun specific cells rather than the full document
  - Great for iterative prototyping and narrative story-telling

- Cells are typically run either by pressing a play button or the
  shortcut `shift+enter`

#### Hosting Services

- There are a number of online services that host jupyter notebook
  environments

1.  [Project Binder](https://jupyter.org/binder)
    - An online service for sharing and jupyter notebook environments
    - Can be linked to a git repository to source books and dependencies
2.  [Jupyter Labs](https://jupyter.org/try)
    - Project Jupyter’s web-based application for developing jupyter
      notebooks
3.  [Google Colab](https://colab.research.google.com/)
    - A google-hosted notebook environment that uses Google’s cloud
      infrastructure
    - Requires a google account
    - Eliminates the need for a local environment setup
    - Offers a range of other features over a standard notebook
      environment

#### LaTeX

- [LaTeX](https://www.latex-project.org/about/) is a type-setting
  system, especially useful for type-setting mathematical equations and
  diagrams.
- LaTeX can be inserted into Markdown text cells either via
  - Inline $latex$ format using `$ text $`

  - Paragraphed by wrapping the latex block in delimiting opening `$$`
    and closing `$$`

    $$ \begin{align*}
        B' &= -\partial \times E, \\
        E' &= \partial \times B - 4\pi j,\\
     \end{align*}$$

#### Google Colab

- As cited Google Colab offers a range of features above standard
  Jupyter Environments

##### Colab Files

- Colab provides a number of mechanisms for accessing files

1.  Notebooks in your google drive are accessible by default
2.  `sample_data`
    - A series of notebooks provided by Google as examples
3.  Uploaded to Session
    - Files can be uploaded to sessions
    - Files are only available in the session they are uploaded to

- By default documents are saved to google drive

- Alternatively you can,

  1.  Save to Github
      - As tracked files in a repository
      - As a gist
  2.  Download them
      - As a notebook (.ipynb)
      - As a python script (.py)
  3.  Share them with others
      - Use the share button

##### Code Snippets

- Code snippets are insertable code blocks that can be used to
  demonstrate Colab features, includes

  1.  Interactive forms
  2.  Data downloads
  3.  Visualisation
  4.  and more…

#### Shell Commands

- Shell commands can be run from a notebook via the `!` or bang operator
  - Prepend the command with the `!` operator

``` python
!pwd
```

    /home/runner/work/foundational-data-science-for-python/foundational-data-science-for-python/1-learning-python-in-a-notebook-environment/Chapter_01

- Shell output can be captured in Python variables to be used in the
  program

``` python
var = !ls
print(var)
```

    ['Chapter_01.html', 'Chapter_01.ipynb', 'Chapter_01.md', 'Chapter_01.qmd', 'Chapter_01.quarto_ipynb', 'Chapter_01_files', 'Examples']

#### Magic Functions

- Magic functions are special in-builts that adjust how a python program
  runs
- For example, jupyter provides an inbuilt of the python `timeit`
  statement for timing the execution of a cell

``` python
import time
%timeit time.sleep(1)
```

    1 s ± 9.1 μs per loop (mean ± std. dev. of 7 runs, 1 loop each)

- Or to render HTML

``` python
%%html
<marquee style='width: 30%; color: blue;'><b>Whee!</b></marquee>
```

<marquee style='width: 30%; color: blue;'><b>Whee!</b></marquee>

> [!TIP]
>
> **Jupyter Magics**
>
> There are a large number of different jupyter *magic functions* that
> are built-in. They cover a wide variety of use cases from timing a
> cell as seen above, to displaying visualisation artifacts within the
> document as opposed to opening a window. The best way to get started
> is by reading the documentation. Jupyter also provides [a
> notebook](https://nbviewer.org/github/ipython/ipython/blob/1.x/examples/notebooks/Cell%20Magics.ipynb)
> containing some example uses.

## Summary

- Python is an interpreted language

- There are three main methods of running python

  1.  Line by line through the Shell
  2.  As a script
  3.  Using cell-based notebooks

- Notebooks mix text with executable code

  - They are a popular format for sharing explainable documents and
    scientific communication
  - Text is written in markdown
  - By default code cells are Python
    - However, other kernels are available for other languages

- There are several methods for working online with notebooks

  1.  Project Binder
  2.  JupyterLab
  3.  Google Colab

- Google Colab integrated with Google and provides a pre-configured
  python environment containing many common packages

## Questions

1.  What kind of Notebooks are hosted in Google Colab?

    - Jupyter Notebooks
    - Interactive notebooks mixing executable code and formatted text

2.  What cells types are available in Jupyter Notebooks?

    - Text cells using formatted markdown
      - Including LaTeX
    - Executable code cells using Python

3.  How do you mount your Google Drive in Colab?

    - Google drive’s can be mounted from the file sub-menu

4.  What language runs in Jupyter Notebook code cells?

    - By default Python is the language of choice
    - Other languages can also be supported by installing other
      [kernels](https://docs.jupyter.org/en/stable/projects/kernels.html)
    - Mixing multiple languages in the same document can be difficult
      however
