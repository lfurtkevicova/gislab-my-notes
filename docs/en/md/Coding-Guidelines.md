Don't use TABs anymore !!! We are currently in state, when all tabs will be replaced with four spaces.

# Ansible
## Directories layout
* role-name
  * files
    * static  - applications source code, icons, themes, menu files (*.desktop, *.directory)
    * scripts - scripts executed by Ansible tasks (script module)
  * handlers  - handlers in one _main.yml_ file
  * tasks     - tasks in one _main.yml_ file
  * templates - configuration files and other files which does not fit in _files_
  * vars      - variables in one _main.yml_ file

## Temporary directories
Use _GISLAB_PATH_TMP_ for storing temporary files during installation and _GISLAB_PATH_TEST_TMP_ during tests. These directories will be automatically removed when installation/test is finished.

## YAML syntax in tasks
### Multiline style
```
- name: multi line with yaml dict
  file:
    path: /tmp/foo
    state: absent
```

### Folded style
With the yaml 'folded' style, anything in a new line (and indented) below a > is essentially a single string that has been wrapped. New line chars are not preserved unless the line is further indented or is an empty line.
```
- name: using folded style to insert content
  copy:
    dest: /tmp/foo
    content: >
      this is a string
      that will be one
      line in the file
```

### Literal style
```
- name: using literal style to insert content - useful for cert files
  copy:
    dest: /tmp/foo
    content: |
      this is a string
      that will have
      new lines preserved
```

### Vim modeline
```
# vim: set ts=8 sts=2 sw=2 et:
```

## Python
### Vim modeline
```
# vim: set ts=8 sts=4 sw=4 et:
```

## Bash
### Vim modeline
```
# vim: set ts=8 sts=4 sw=4 et:
```