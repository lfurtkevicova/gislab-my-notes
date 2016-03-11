Don't use TABs anymore !!! We are currently in state, when all tabs will
be replaced with four spaces.

Ansible
=======

Directories layout
------------------

-  role-name
-  files

   -  static - applications source code, icons, themes, menu files
      (*.desktop, *.directory)
   -  scripts - scripts executed by Ansible tasks (script module)

-  handlers - handlers in one *main.yml* file
-  tasks - tasks in one *main.yml* file
-  templates - configuration files and other files which does not fit in
   *files*
-  vars - variables in one *main.yml* file

Temporary directories
---------------------

Use *GISLAB*\ PATH\_TMP\_ for storing temporary files during
installation and *GISLAB*\ PATH\_TEST\_TMP\_ during tests. These
directories will be automatically removed when installation/test is
finished.

YAML syntax in tasks
--------------------

Multiline style
~~~~~~~~~~~~~~~

::

    - name: multi line with yaml dict
      file:
        path: /tmp/foo
        state: absent

Folded style
~~~~~~~~~~~~

With the yaml 'folded' style, anything in a new line (and indented)
below a > is essentially a single string that has been wrapped. New line
chars are not preserved unless the line is further indented or is an
empty line.

::

    - name: using folded style to insert content
      copy:
        dest: /tmp/foo
        content: >
          this is a string
          that will be one
          line in the file

Literal style
~~~~~~~~~~~~~

::

    - name: using literal style to insert content - useful for cert files
      copy:
        dest: /tmp/foo
        content: |
          this is a string
          that will have
          new lines preserved

Vim modeline
~~~~~~~~~~~~

::

    # vim: set ts=8 sts=2 sw=2 et:

Python
------

Vim modeline
~~~~~~~~~~~~

::

    # vim: set ts=8 sts=4 sw=4 et:

Bash
----

Vim modeline
~~~~~~~~~~~~

::

    # vim: set ts=8 sts=4 sw=4 et:

