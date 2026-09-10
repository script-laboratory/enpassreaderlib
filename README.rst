===============
enpassreaderlib
===============

A library implementing the decrypting and retrieving secrets from an enpass 6 database.


* Documentation: https://enpassreaderlib.readthedocs.org/en/latest

### Note for Documentation.

pysqlcipher3 needs to compile on your workstation and may fail with python 3.12 or later.
Apply
   [PR-38](https://github.com/rigglemania/pysqlcipher3/pull/38)
in https://github.com/rigglemania/pysqlcipher3, or use forked repository in
 [our repositories](https://github.com/script-laboratory/pysqlcipher3)

Development Workflow
====================

The workflow supports the following steps

 * lint
 * test
 * build
 * document
 * upload
 * graph

These actions are supported out of the box by the corresponding scripts under _CI/scripts directory with sane defaults based on best practices.
Sourcing setup_aliases.ps1 for windows powershell or setup_aliases.sh in bash on Mac or Linux will provide with handy aliases for the shell of all those commands prepended with an underscore.

The bootstrap script creates a .venv directory inside the project directory hosting the virtual environment. It uses pipenv for that.
It is called by all other scripts before they do anything. So one could simple start by calling _lint and that would set up everything before it tried to actually lint the project

Once the code is ready to be delivered the _tag script should be called accepting one of three arguments, patch, minor, major following the semantic versioning scheme.
So for the initial delivery one would call

    $ _tag --minor

which would bump the version of the project to 0.1.0 tag it in git and do a push and also ask for the change and automagically update HISTORY.rst with the version and the change provided.


So the full workflow after git is initialized is:

 * repeat as necessary (of course it could be test - code - lint :) )

   * code
   * lint
   * test
 * commit and push
 * develop more through the code-lint-test cycle
 * tag (with the appropriate argument)
 * build
 * upload (if you want to host your package in pypi)
 * document (of course this could be run at any point)


Important Information
=====================

This template is based on pipenv. In order to be compatible with requirements.txt so the actual created package can be used by any part of the existing python ecosystem some hacks were needed.
So when building a package out of this **do not** simple call

    $ python setup.py sdist bdist_egg

**as this will produce an unusable artifact with files missing.**
Instead use the provided build and upload scripts that create all the necessary files in the artifact.



Project Features
================

See USAGE.rst.

* Can retrieve single entries
* Can iterate over all entries
* Can do fuzzy matching of entries while searching

What Changed from Original
==========================

* Moved array of field types to get from local variable to member variable of class.
* Changed logic to create fields of itemfield in sql.
* Changed logic when storing results from pysqlcipher3 dbapi2 due to changed format.
* Changed logic storing results to dictionary because pysqlcipher3 return values as array.
* Store all fields in entry stores in _row variable so that we can use fields other than login and password.
