# Copyright 2014, Stephen Fryatt (info@stevefryatt.org.uk)
#
# This file is part of Clipboard:
#
#   http://www.stevefryatt.org.uk/software/
#
# Licensed under the EUPL, Version 1.1 only (the "Licence");
# You may not use this work except in compliance with the
# Licence.
#
# You may obtain a copy of the Licence at:
#
#   http://joinup.ec.europa.eu/software/page/eupl
#
# Unless required by applicable law or agreed to in
# writing, software distributed under the Licence is
# distributed on an "AS IS" basis, WITHOUT WARRANTIES
# OR CONDITIONS OF ANY KIND, either express or implied.
#
# See the Licence for the specific language governing
# permissions and limitations under the Licence.

# This file really needs to be run by GNUMake.
# It is intended for native compilation on Linux (for use in a GCCSDK
# environment) or cross-compilation under the GCCSDK.

# Set VERSION to build using a version number and not an SVN revision.

.PHONY: all clean application documentation release backup

# The build date.

BUILD_DATE := $(shell date "+%d %b %Y")
HELP_DATE := $(shell date "+%-d %B %Y")

# Construct version or revision information.

ifeq ($(VERSION),)
  RELEASE := $(shell svnversion --no-newline)
  VERSION := r$(RELEASE)
  RELEASE := $(subst :,-,$(RELEASE))
  HELP_VERSION := ----
else
  RELEASE := $(subst .,,$(VERSION))
  HELP_VERSION := $(VERSION)
endif

$(info Building with version $(VERSION) ($(RELEASE)) on date $(BUILD_DATE))

# The archive to assemble the release files in.  If $(RELEASE) is set, then the file can be given
# a standard version number suffix.

ZIPFILE := clipboard$(RELEASE).zip
SRCZIPFILE := clipboard$(RELEASE)src.zip
BUZIPFILE := clipboard$(shell date "+%Y%m%d").zip

# Build Tools

MKDIR := mkdir
RM := rm -rf
CP := cp

ZIP := $(GCCSDK_INSTALL_ENV)/bin/zip

LIBPATHS := BASIC:$(SFTOOLS_BASIC)/

MANTOOLS := $(SFTOOLS_BIN)/mantools
BINDHELP := $(SFTOOLS_BIN)/bindhelp
TEXTMERGE := $(SFTOOLS_BIN)/textmerge
MENUGEN := $(SFTOOLS_BIN)/menugen
TOKENIZE := $(SFTOOLS_BIN)/tokenize


# Build Flags

ZIPFLAGS := -x "*/.svn/*" -r -, -9
SRCZIPFLAGS := -x "*/.svn/*" -r -9
BUZIPFLAGS := -x "*/.svn/*" -r -9
BINDHELPFLAGS := -f -r -v
TOKFLAGS := -verbose -crunch EIrW -warn pV -swi -swis $(GCCSDK_INSTALL_CROSSBIN)/../arm-unknown-riscos/include/swis.h -swis $(GCCSDK_INSTALL_ENV)/include/TokSWIs.h


# Set up the various build directories.

SRCDIR := src
MANUAL := manual
OUTDIR := build


# Set up the named target files.

APP := !Clipboard
RUNIMAGE := !RunImage,ffb
README := ReadMe,fff
TEXTHELP := !Help,fff


# Set up the source files.

MANSRC := Source
READMEHDR := Header

SRCS := Clipboard.bbt

# Build everything, but don't package it for release.

all: application documentation


# Build the application and its supporting binary files.

application: $(OUTDIR)/$(APP)/$(RUNIMAGE)


# Build the complete !RunImage from the object files.

SRCS := $(addprefix $(SRCDIR)/, $(SRCS))

$(OUTDIR)/$(APP)/$(RUNIMAGE): $(SRCS)
	$(TOKENIZE) $(TOKFLAGS) $(firstword $(SRCS)) -link -out $(OUTDIR)/$(APP)/$(RUNIMAGE) -path $(LIBPATHS) -define 'build_date$$=$(BUILD_DATE)' -define 'build_version$$=$(VERSION)'

# Build the documentation

documentation: $(OUTDIR)/$(APP)/$(TEXTHELP) $(OUTDIR)/$(README)

$(OUTDIR)/$(APP)/$(TEXTHELP): $(MANUAL)/$(MANSRC)
	$(MANTOOLS) -MTEXT -I$(MANUAL)/$(MANSRC) -O$(OUTDIR)/$(APP)/$(TEXTHELP) -D'version=$(HELP_VERSION)' -D'date=$(HELP_DATE)'

$(OUTDIR)/$(README): $(OUTDIR)/$(APP)/$(TEXTHELP) $(MANUAL)/$(READMEHDR)
	$(TEXTMERGE) $(OUTDIR)/$(README) $(OUTDIR)/$(APP)/$(UKRES)/$(TEXTHELP) $(MANUAL)/$(READMEHDR) 5

# Build the release Zip file.

release: clean all
	$(RM) ../$(ZIPFILE)
	(cd $(OUTDIR) ; $(ZIP) $(ZIPFLAGS) ../../$(ZIPFILE) $(APP) $(README))
	$(RM) ../$(SRCZIPFILE)
	$(ZIP) $(SRCZIPFLAGS) ../$(SRCZIPFILE) $(OUTDIR) $(SRCDIR) $(MANUAL) Makefile


# Build a backup Zip file

backup:
	$(RM) ../$(BUZIPFILE)
	$(ZIP) $(BUZIPFLAGS) ../$(BUZIPFILE) *


# Clean targets

clean:
	$(RM) $(OUTDIR)/$(APP)/$(RUNIMAGE)
	$(RM) $(OUTDIR)/$(APP)/$(TEXTHELP)
	$(RM) $(OUTDIR)/$(README)

