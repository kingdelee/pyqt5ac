PyQt5 Auto Compiler (pyqt5ac)
=================
pyqt5ac is a Python package for automatically compiling Qt's UI and QRC files into Python files.

[Qt Designer](https://www.qt.io/) is an application apart of Qt's suite of tools that allows one to create a GUI using a drag-and-drop interface. The user interface for a window/widget/dialog is stored in a *.ui* file and any resources such as images or icons are stored in a *.qrc* file.

These two filetypes must be compiled into Python files before they can be used in your Python program using PyQt5. There are a few ways to go about this currently:
1. Manually compile the files using the command line and pyuic5 for *.ui* files and pyrcc for *.qrc* files.
2. Compile the files each time the application is started up by calling pyuic5 and pyrcc5 within your Python script

The downside to the first method is that it can be a tedious endeavor to compile the files, especially when one is faced with a larger project with many of these files that need to be compiled.

Although the second method eliminates the tediousness of compilation, these files are compiled **every** time you run your script, regardless of if anything has been changed. This can cause a hit in performance and take longer to startup your script.

Enter *pyqt5ac*! This command-line interface (CLI) given a set of parameters will search through your files and automatically compile any *.ui* or *.qrc* files. In addition, pyqt5ac can be called from your Python script. In both instances, **ui and resource files are only compiled if they have been updated**.

Installing
=================
Installing pyqt5ac
-------------------------
pyqt5ac is currently available on `PyPi <https://pypi.python.org/pypi/pyqt5ac/>`_. The simplest way to
install alone is using ``pip`` at a command line::

  pip install pyqt5ac

which installs the latest release.  To install the latest code from the repository (usually stable, but may have
undocumented changes or bugs)::

  pip install git+https://github.com/addisonElliott/pyqt5ac.git

For developers, you can clone the pyqt5ac repository and run the ``setup.py`` file. Use the following commands to get
a copy from GitHub and install all dependencies::

  git clone pip install git+https://github.com/addisonElliott/pyqt5ac.git
  cd pyqt5ac
  pip install .

or, for the last line, instead use::

  pip install -e .

to install in 'develop' or 'editable' mode, where changes can be made to the local working code and Python will use
the updated polarTransform code.

Getting Started
===============
All of the options that can be specified to pyqt5ac can also be placed in a configuration file (JSON or YAML). My recommendation is to use a configuration file to allow easy compilation of your software. For testing purposes, I would use the options in the command line interface to make sure everything works and then transcribe that into a configuration file for repeated use.

Configuration Options
---------------------
Whether running via the command line or from a script, the arguments and options that can be given are the same. The valid options are:
* **rccPath** - Path to the resource compiler. Default is pyrcc5 and assumes pyrcc5 is in the PATH.
* **rccOptions** - Additional options to pass to the resource compiler. See the man page of pyrcc5 for more information on options. An example of a valid option would be "-compress 1". Default is to pass no options.
* **uicPath** - Path to the resource compiler. Default is pyuic5 and assumes pyuic5 is in the PATH.
* **uicOptions** - Additional options to pass to the UI compiler. See the man page of pyuic5 for more information on options. An example of a valid option would be '--from-imports'. Default is to pass no options.
* **force** - Specifies whether to force compile all of the files found. The default is false which means only outdated files will be compiled.
* **config** - JSON or YAML configuration file that contains information about these parameters.
* **ioPaths** - This is a 2D list containing information about what source files to compile and where to place the source files. The first column is the source file global expression (meaning you can use wildcards, ** for recursive folder search, ? for options, etc to match filenames) and the second column is the destination file expression. The destination file expression can these 'special' variables that will be replaced with information from the source filename:
    * %%FILENAME%% - Filename of the source file without the extension
    * %%EXT%% - Extension excluding the period of the file (e.g. ui or qrc)
    * %%DIRNAME%% - Directory of the source file

Example Setup
-------------
Give an example file structure here of PATS or something and explain what I did.
TODO Keep going...

Running from Command Line
=================
If pyqt5ac is installed via pip, the command line interface can be called like any Unix based program in the terminal::

    pyqt5ac [OPTIONS] [IOPATHS]...
    
In the interface, the options have slightly different names so reference the help file of the interface for more information. The largest difference is that the ioPaths option is now just a list of space delineated paths where the even items are the source file expression and the odd items are the destination file expression.

The help file of the interface can be run as::

    pyqt5ac --help

Running from Python Script
=================
XXX

Example Configuration Files
=================
YAML
-----------------
```YAML
ioPaths:
  -
    - "D:/Users/addis/Documents/PythonProjects/PATS/gui/*.ui"
    - "%%DIRNAME%%/../generated/%%FILENAME%%_ui.py"
  -
    - "D:/Users/addis/Documents/PythonProjects/PATS/resources/*.qrc"
    - "D:/Users/addis/Documents/PythonProjects/PATS/generated/%%FILENAME_%%EXT%%.py"

rcc: pyrcc5
uic: pyuic5
uic_options: --from-imports
force: True
```

JSON
-----------------
```JSON
{
  "ioPaths": [
    ["D:/Users/addis/Documents/PythonProjects/PATS/gui/*.ui", "%%DIRNAME%%/../generated4/%%FILENAME%%_ui.py"]
  ],
  "rcc": "pyrcc5",
  "rcc_options": "",
  "uic": "pyuic5",
  "uic_options": "--from-imports",
  "force": true
}
```

License
=================
pyqt5ac has an MIT-based `license <https://github.com/addisonElliott/pyqt5ac/blob/master/LICENSE>`_.
