================
About these docs
================

These docs are literally my digitized master studies and Aalto University.
I make notes about courses I take. Much content is brought from somewhere
else such as blogs, Stackoverflow, Wikipedia and etc. I try to be strict
with citation as much as possible. You may fork it or do whatever you may
want to do.

The structure of this docs is forked from Aalto University CS-IT documentation
page, http://scicomp.aalto.fi. I work there.

Contributing
~~~~~~~~~~~~

This documentation is Open Source (CC-BY 4.0), and we welcome
contributions from the Aalto community.  The project is run on Github
(https://github.com/AaltoScienceIT/triton-docs).

To contribute, you can always use the normal Github contribution
mechanims: make a pull request or comments.  If you are at Aalto, you
can also get direct write access.  Make a github issue, then contact
us in person/by email for us to confirm.

**The worst contribution is one that isn't made.** Don't worry about
making things perfect: since this is in version control, we track all
changes and will just fix anything that's not perfect.  This is also
true for formatting errors - if you can't do ReStructudedText
perfectly, just do your best (and pretend it's markdown because all
the basics are similar).

Contributing gives consent to use content under the licenses (CC-BY
4.0 or CC0 for examples).


Requirements and building
~~~~~~~~~~~~~~~~~~~~~~~~~

The only software needed is Sphinx: Debian package
``python-sphinx``, : PyPI: ``python-sphinx``.  It is already installed
on Aalto workstations.

To build the docs, run ``make html``.

HTML output is in ``_build/html/index.html``, and other output formats
are available as well.


Editing
~~~~~~~

Look at examples and copy.  To add sections, add a new page in a
subfolder.  Link it from the main Table of Contents (``toctree``) in
``index.rst`` to have the document appear and be cross-referenced.

You can see a complete example from UiT: `source
<https://github.com/uit-no/hpc-doc>`_ and `compiled HTML
<http://hpc.uit.no/en/latest/>`_.



ReStructured text
~~~~~~~~~~~~~~~~~

ReStructured Text is similar to markdown for basics, but has a more
strictly defined syntax and more higher level structure.  This
allows more semantic markup, more power to compile into different
formats (since there isn't embedded HTML), and advanced things like
indexing, permanent references, etc.

Restructured text `home <http://docutils.sourceforge.net/rst.html>`_
and `quick reference
<http://docutils.sourceforge.net/docs/user/rst/quickref.html>`_.

Note: Literal inline text uses `````` instead of a single ````` (second
works but gives warning).

A very quick guide is below.

----

Header style::
   
   \=====
   Title
   \=====

   Heading1
   \=======

   Heading2
   \#######

   Heading3
   \^^^^^^^
   
   Heading4
   \*******
   
   Heading5
   \+++++++

----

``Inline code/monospace``, *emphasis*, **strong emphasis**

::

   ``Inline code/monospace``, *emphasis*, **strong emphasis**

----

::
   
   Block quote
   Block quote


::

   ::

     Block quote
     Block quote

----

Block quotes can also start with paragraph ending in double colon,
like this::

  Block quote

::

   Block quotes can also start with paragraph ending in double colon,
   like this::

       Block quote

----

Inline `link <http://python.org>`_, or
anonymous__, or
separate_, or
`different text <separate_>`_ links.
Trailing underscores indicate links.

__ http://python.org

.. _separate: http://python.org

::

    Inline `link <http://python.org>`_, or
    anonymous__, or
    separate_, or
    `different text <separate_>`_ links.
    Trailing underscores indicate links.

    __ http://python.org

    .. _separate: http://python.org

----

Linking to the web.  If possible use a permanent reference (next
section), but you can also refer to specific files by name.  Note,
that for internal links there are no trailing underscores::

  :doc:`../tut/interactive.rst`  (recommended)
  `../tut/interactive.rst`       (short, no warning if link breaks)

  With different text:
  :doc:`Text <../tut/interactive.rst>`  (recommended)
  `Text <../tut/interactive.rst>`       (short, no warning if link breaks)


----

Internal links.  `Permanent references across files <http://www.sphinx-doc.org/en/stable/markup/inline.html#role-ref>`_

Label things this way (note only one colon)::

  .. _label-name:

Reference them this way::

  :ref:`label-name`     (recommended)
  `label-name`          (short, no warning if link breaks)
  `Text <label-name>`   (short, no warning if link breaks)

----

Admonitions: attention, caution, danger, error, hint, important, note, tip, warning, etc.

.. note::

   This is a note

.. warning::

   This is a warning

.. seealso:: 

  This is a simple **seealso** note.

::

  .. note::

    This is a note

  .. warning::

    This is a warning

  .. seealso:: 

    This is a simple **seealso** note.


---

.. topic:: Topic Title

    Subsequent indented lines comprise
    the body of the topic, and are
    interpreted as body elements.

.. sidebar:: Sidebar Title
   :subtitle: Optional Sidebar Subtitle

   Subsequent indented lines comprise
   the body of the sidebar, and are
   interpreted as body elements.