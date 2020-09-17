Clipper
=======

A Global Clipboard utility.


Introduction
------------

The global clipboard provides a way of transferring data between applications. You select the data in one application and cut or copy it onto the clipboard (usually using Ctrl-X and Ctrl-C). You then move to the other application and paste the data from the clipboard (usually Ctrl-V).

Clipper allows you to save the current contents of the global clipboard, either to disc or to another application. You can also drag files from a filer window to Clipper to put that file on the clipboard, and type the clipboard contents to the caret.

The Clipper utility was originally called Clipboard and written by Thomas Leonard. It was converted to 32-bit for use on RISC OS 5 by myself (Steve Fryatt), and was renamed Clipper in 2020 to resolve a clash with a legitimately-allocated application name. I would like to thank Thomas for allowing his code to be placed in the Public Domain; any problems encountered using the updated application and module are likely to be my fault and should be reported first to the address below.


Building
--------

Clipper consists of a collection of un-tokenised BASIC, which must be assembled using the [SFTools build environment](https://github.com/steve-fryatt). It will be necessary to have suitable Linux system with a working installation of the [GCCSDK](http://www.riscos.info/index.php/GCCSDK) to be able to make use of this.

With a suitable build environment set up, making Clipper is a matter of running

	make

from the root folder of the project. This will build everything from source, and assemble a working !Clipper application and its associated files within the build folder. If you have access to this folder from RISC OS (either via HostFS, LanManFS, NFS, Sunfish or similar), it will be possible to run it directly once built.

To clean out all of the build files, use

	make clean

To make a release version and package it into Zip files for distribution, use

	make release

This will clean the project and re-build it all, then create a distribution archive (no source), source archive and RiscPkg package in the folder within which the project folder is located. By default the output of `git describe` is used to version the build, but a specific version can be applied by setting the `VERSION` variable -- for example

	make release VERSION=1.23


Licence
-------

Clipper is licensed under the EUPL, Version 1.2 only (the "Licence"); you may not use this work except in compliance with the Licence.

You may obtain a copy of the Licence at <http://joinup.ec.europa.eu/software/page/eupl>.

Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "**as is**"; basis, **without warranties or conditions of any kind**, either express or implied.

See the Licence for the specific language governing permissions and limitations under the Licence.