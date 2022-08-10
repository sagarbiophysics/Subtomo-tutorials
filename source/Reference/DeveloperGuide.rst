Developer Guide
===============

.. note::
    This page describes information for developers and code contributors.
    General users do not need information here.


How to compile this documentation
---------------------------------

To compile this documentation into HTML, type::

    make html

For PDF, perform::

    make latex
    cd build/latex
    pdflatex MultishotTomo.tex # you might have to install several style files

If your system has ``latexmk``, this can be done in one line::

    make latexpdf

If above commands complain about missing Python packages, try::

    python3 -m pip install --upgrade sphinx sphinxcontrib-bibtex

Coding styles in reStructuredText (rST)
---------------------------------------

For Å, α, β in the main text, just type as is using Unicode. Don't use TeX commands.

To make ``git diff`` easier to understand, each sentense should use a new line.

::

    A sentence.
    Next sentence.

    A new paragraph.
    ``Keyword``, *emphasis by italic* and **emphasis by bold** are marked like this.
    You can cite a paper :cite:`Khavnekar2022.04.10.487763`.
    A link to `rST documentation <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_.
    Note the underline.
    You can use LaTeX math :math:`\sum x_i^2`.

    We have custom tags for :button:`buttons`, :joblist:`joblists`, :jobtype:`jobtypes` and :guitab:`guitabs`.
    Custom tags will be updates as the documentation evolves. 

    This paragram is a bad example. Two sentences are in one line.

    ::

        A block quote for commands is an indented block following two
        colons and an empty line. Please use FOUR SPACES, not tabs for indentation.

    -   When using a list, be careful about indentation.
        The second sentence must be aligned.
    -   The second item in the list.

    :Field list: suggested value

        (Explanation sentence 1.
        Note the indentation of this line!
        Also note the following blank line.)

    :Another option without a value: \

        (Please put `\` at the end of the line to the field that should be left empty.)

The above example is rendered as:

A sentence.
Next sentence.

A new paragraph.
``Keyword``, *emphasis by italic* and **emphasis by bold** are marked like this.
You can cite a paper :cite:`Khavnekar2022.04.10.487763`.
A link to `rST documentation <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_.
Note the underline.
You can use LaTeX math :math:`\sum x_i^2`.

We have custom tags for :button:`buttons`, :joblist:`joblists`, :jobtype:`jobtypes` and :guitab:`guitabs`.
Custom tags will be updates as the documentation evolves. 

This paragram is a bad example. Two sentences are in one line.

::

    A block quote for commands is an indented block following two
    colons and an empty line. Please use FOUR SPACES, not tabs for indentation.

-   When using a list, be careful about indentation.
    The second sentence must be aligned.
-   The second item in the list.

:Field list: suggested value

    (Explanation sentence 1.
    Note the indentation of this line!
    Also note the blank lines.)

:Another option without a value: \

    (Please put ``\`` at the end of the line to the field that should be left empty.)