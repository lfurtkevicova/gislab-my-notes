.. |tip| image:: img/tip.png
   :width: 2.5em
.. |att| image:: img/attention.png
   :width: 2.5em
.. |todo| image:: img/todo.png
   :width: 1.5em
.. |see| image:: img/see.png
   :width: 1.5em
.. |note| image:: img/note.png
   :width: 1.5em
.. |important| image:: img/important.png
   :width: 1.5em
.. |danger| image:: img/danger.png
   :width: 1.5em

.. _conventions:

**************************************
Conventions used in this documentation
**************************************

There are many different organizational and typographical features throughout 
this documentation designed to help you get the most out of the material.
You will find number of styles of text that distinguish 
between different kinds of information. Here are some types of headings, 
examples of typographical conventions, styles, and an explanation of their 
meaning.

.. rubric:: This is style of paragraph heading e.g. Types of Headings:

For style of chapter names, please :ref:`see <conventions>` chapter name above,
others are shown below.

========
Sections
========

-----------
Subsections
-----------

^^^^^^^^^^^^^^
Subsubsections
^^^^^^^^^^^^^^

""""""""""
Paragraphs
""""""""""

#####
Parts
#####

.. rubric:: Styles of text:

``Code text`` represents code, commands, options, switches, variables, 
attributes, keys, functions, types, classes, namespaces, methods, modules, 
properties, parameters, values, objects, events, event handlers, tags, macros, 
the contents of files, or the output from commands. More comprehensive
parts are written in blocks as follows: 

.. code::

	<code block>

.. code-block:: python
   :emphasize-lines: 3

   some python code
   if re.match(r'^\d{3}-\d{4}$', test_string):
   some python code highlighted
   some python code 

.. code-block:: sh
   :linenos:

   some numbered shell script code
   if ! [ $MAX_NO -ge 5 -a $MAX_NO -le 9 ] ; then
   some numbered shell script code

.. code-block:: sql
  
   some sql statement
   ORDER BY Name ASC;
   some sql statement

*Italic* indicates new terms, URLs, email addresses, filenames, file extensions, 
pathnames, directories, and Unix utilities.

**Bold** shows commands or other text indicates that we wish to draw your 
attention to a particular part of a code block, the relevant lines or items.

`Plain text` indicates menu titles, menu options, menu buttons, and keyboard 
accelerators.

Other roles like :superscript:`superscript` and :subscript:`subscript` text.

.. raw:: html

   <font color="silver"> Useful terms </font> 

are represented with grey color. 

Commands are written as :command:`Some command`, guilabel as 
:guilabel:`Guilabel`, direction through a menu is displayed as 
:menuselection:`First step --> Second step`, name of file is represented by 
:file:`file.svg`. For usage of footnotes, see [#name]_, external hyperlinks are 
represented as `GIS.lab web page <http://web.gislab.io/>`_, for reference to 
some picture, see :num:`#some-figure-t`, 
:num:`#some-figure-s`, :num:`#some-figure-m` and :num:`#some-figure-l`, 
for reference to some part of page, 
see :ref:`Conventions <conventions>`, to include files as part of the build process e.g. :download:`GIS.lab logo <https://github.com/gislab-npo/gislab-web/blob/master/clients/src/web/styles/map/image_logo.svg>` is used.

.. rubric:: Short paragraphs:

.. tip:: |tip| This signifies a tip, suggestion, or general useful note.

.. attention:: |att| This style indicates a warning or caution.

.. note:: |note| This is note.

.. important:: |important| This represents something important.

.. danger:: |danger| This style indicates a warning or caution.

.. seealso:: |see| This note leads the user to another material that is on the similar level of scope.

.. note is displayed only if ``todo_include_todos`` in ``conf.py`` is set as ``True``.

.. todo:: |todo| This signifies some issue to be done next time.

.. sidebar:: Some Sidebar 

   :code:`vagrant up`

.. rubric:: Lists and Quote-like blocks:

#. numbered list 
  #. nested numbered list

* bulleted list 

  * nested bulleted list

.. rubric: Sidebars:

.. rubric:: Figures:

.. _some-figure-t:

.. figure:: img/login_text_logo.svg
   :align: center
   :width: 150

   GIS.lab unit tiny.

.. _some-figure-s:

.. figure:: img/login_text_logo.svg
   :align: center
   :width: 250

   GIS.lab unit small.

.. _some-figure-m:

.. figure:: img/login_text_logo.svg
   :align: center
   :width: 450

   GIS.lab unit middle.

.. _some-figure-l:

.. figure:: img/login_text_logo.svg
   :align: center
   :width: 750

   GIS.lab unit large.

.. rubric:: Tables:

+---------------------------------------+----------------+
| Contributors to GIS.lab documentation |    Country     |
+=======================================+================+
|          Ludmila Furtkevicova         |    Slovakia    |
+---------------------------------------+----------------+
|               Ivan Mincik             |    Slovakia    |
+---------------------------------------+----------------+
|               Martin Landa            | Czech Republic |
+---------------------------------------+----------------+
|                   ...                 |       ...      |
+---------------------------------------+----------------+

.. csv-table:: Table with GIS.lab contributors.
   :header: "Contributors to GIS.lab documentation", "Country"
   :widths: 20, 10

   "Ludmila Furtkevicova", "Slovakia"
   "Ivan Mincik", "Slovakia"
   "Martin Landa", "Czech republic"
   "...", "..."

.. rubric:: Columns:

.. hlist::
    :columns: 3

    * A
    * B
    * C
    * D 
    * E
    * F
    * G
    * H
    * I
    * J
    * K
    * L 

.. rubric:: Footnotes:

.. [#name] Some footnote.


