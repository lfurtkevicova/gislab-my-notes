One of the most important responsibilities of QA team is to produce meaningful bug reports which will help developers to fix a problem.

# Recording a problem
If You have found a problem, please submit it as an Issue to GIS.lab [bug tracker](https://github.com/imincik/gis-lab/issues), assign _bug_ tag and provide as much information as possible about the situation when the problem happened and step-by-step instructions how to reproduce the problem. If some specific data is needed to reproduce a problem, please attach also small sample dataset too. Please report only one problem per one Issue. You can also assign Issue to particular person.

## QGIS
If You want to report application crashes, please provide so called _traceback_ which will help a developer to locate a possible reason of Your problem. Follow these instructions:
* Open a Terminal window (Accessories > Terminal Emulator)
* Start GDB by running command
`gdb /usr/bin/qgis`
* Start application by command
`run`
* Do whatever You need to reproduce Your problem
* If application crashes save approximately last 30 lines of GDB output to text file
* Print backtrace by running command
`bt`
* Press _SPACE_ until You reach end of backtrace output and append it to a text file which You have created before
* Attach text file with GDB output and backtrace to Your bug report

# Forwarding bug reports to upstream projects
## QGIS
**Bug tracker**: http://hub.qgis.org/projects/quantum-gis/issues  
To create bug report OSGeo ID is needed. It is possible to create it by this [form.](https://www2.osgeo.org/cgi-bin/ldap_create_user.py)

**Required details**:
* Operating System: XUbuntu 12.04 32bit
* Software versions: please provide all informations from QGIS > Help > About > About