Guide for developers

How relinux is setup
====================
Relinux is split up into utilities and modules. Utilities are libraries that the modules use, and modules are
the parts that make up the relinux toolkit (OSWeaver being the main one).

Tree and basic function of each file
====================================

	src
	├── __init__.py
	└── relinux
	    ├── config.py             Application configuration
	    ├── configutils.py        Utilities for parsing relinux.conf
	    ├── fsutil.py             Utilities to manage the system
	    ├── gui.py                Everything GUI-related
	    ├── __init__.py
	    ├── localization.py       Designed to ease usage of gettext, but might be deprecated soon
	    ├── logger.py             Logging utilities
	    ├── __main__.py           Runs relinux
	    ├── modules
	    │   ├── __init__.py
	    │   └── osweaver          OSWeaver Module
	    │       ├── __init__.py   Module information
	    │       ├── isoutil.py    Generates the ISO structure
	    │       ├── squashfs.py   Generates the SquashFS
	    │       └── tempsys.py    Generates the temporary system (that SquashFS will use)
	    ├── pwdmanip.py           Utilities for parsing /etc/passwd, /etc/g{roup|shadow}, and /etc/shadow
	    ├── test                  Temporary file to use awk/sed with :P
	    ├── threadmanager.py      Manages threads
	    └── versionsort.py        Contains a version sorting class (but will be moved to configutils)

Creating a module
=================
Modules are relatively easy to make. A basic module structure looks like this:

	modulename
	└── __init__.py

The `__init__.py` file does all the work in the module. Here is an example of one:

	'''
	Random Module
	@author: Anonymous
	'''
	
	relinuxmodule = True
	
	def run(adict):
		# Do something here

Note the relinuxmodule variable. Without that variable set to true, relinux will not detect the module.
The run function is called with a dictionary as soon as the module is loaded. The dictionary contains
pointers to objects that relinux uses (see next section for an example of one). 

Making a GUI
============
As mentioned previously, the run function contains a dictionary that contains pointers to objects.
One of these is the GUI object, which allows you to change the GUI in any way you like, for example:

	def run(adict):
		gui = adict["gui"]
		pagenum = gui.wizard.add_tab()
		gui.mypage = tkinter.Label(gui.wizard.page_container(pagenum), text=_("My Page"))
		gui.wizard.add_page_body(pagenum, _("Page"), gui.mypage)

This would create a page with the title of "Page", and the text inside would be "My Page"