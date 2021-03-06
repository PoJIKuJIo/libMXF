#
# $Id: Makefile,v 1.5 2010/07/21 16:29:33 john_f Exp $
#
# Makefile for building archive examples
#
# Copyright (C) 2006  BBC Research, Philip de Nier <philipn@users.sourceforge.net>
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA
# 02110-1301, USA.
#
TOPLEVEL = ../..
include $(TOPLEVEL)/vars.mk


.PHONY: all
all: test_timecode_index write info test

.PHONY: write
write:
	$(MAKE) -C write

.PHONY: info
info:
	$(MAKE) -C info

.PHONY: test
test:
	$(MAKE) -C test


$(LIBMXF_DIR)/libMXF.a:
	$(MAKE) -C $(LIBMXF_DIR)

test_timecode_index: $(LIBMXF_DIR)/libMXF.a test_timecode_index.o timecode_index.o
	$(CC) test_timecode_index.o timecode_index.o -L$(LIBMXF_DIR) -lMXF $(UUIDLIB) -o $@

test_timecode_index.o: test_timecode_index.c archive_types.h
	$(CC) $(CFLAGS) -c test_timecode_index.c

timecode_index.o: timecode_index.c timecode_index.h archive_types.h
	$(CC) $(CFLAGS) -c timecode_index.c


.PHONY: install
install:
	mkdir -p $(MXF_INSTALL_PREFIX)/include/mxf/examples/archive
	cp archive_types.h $(MXF_INSTALL_PREFIX)/include/mxf/examples/archive/
	$(MAKE) -C write $@
	$(MAKE) -C info $@

.PHONY: uninstall
uninstall:
	$(MAKE) -C write $@
	$(MAKE) -C info $@
	rm -Rf $(MXF_INSTALL_PREFIX)/include/mxf/examples/archive

.PHONY: clean
clean:
	@rm -f *.o *~ test_timecode_index
	$(MAKE) -C write $@
	$(MAKE) -C info $@
	$(MAKE) -C test $@

.PHONY: check
check: all
	$(MAKE) -C write $@
	./test_timecode_index

.PHONY: valgrind-check
valgrind-check: all
	$(MAKE) -C write $@
	valgrind ./test_timecode_index

