#
# Makefile:
#	wiringPi device - A "wiring" library for the Raspberry Pi
#
#	Copyright (c) 2012-2016 Gordon Henderson
#################################################################################
# This file is part of wiringPi:
#	https://projects.drogon.net/raspberry-pi/wiringpi/
#
#    wiringPi is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Lesser General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    wiringPi is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Lesser General Public License for more details.
#
#    You should have received a copy of the GNU Lesser General Public License
#    along with wiringPi.  If not, see <http://www.gnu.org/licenses/>.
#################################################################################

VERSION=$(shell cat ../VERSION)
DYN_VERS_MAJ=$(word 1,$(subst ., ,$(VERSION)))
DESTDIR?=/usr
PREFIX?=/local

LDCONFIG?=ldconfig

ifneq ($V,1)
Q ?= @
endif

INCLUDE_DIR?=$(DESTDIR)$(PREFIX)/include
LIB_DIR?=$(DESTDIR)$(PREFIX)/lib

BASE_NAME=libwiringPiDev
STATIC=$(BASE_NAME).a
DYNAMIC=$(BASE_NAME).so.$(VERSION)

#DEBUG ?= -g -O0
DEBUG	?= -O2
CC	?= gcc
INCLUDE	?= -I.
DEFS	?= -D_GNU_SOURCE
CFLAGS	?= $(DEBUG) $(DEFS) -Wformat=2 -Wall -Winline $(INCLUDE) -pipe -fPIC

LIBS    =

###############################################################################

SRC	=	ds1302.c maxdetect.c  piNes.c		\
		gertboard.c piFace.c			\
		lcd128x64.c lcd.c			\
		scrollPhat.c				\
		piGlow.c

OBJ	=	$(SRC:.c=.o)

HEADERS	=	ds1302.h gertboard.h  lcd128x64.h  lcd.h  maxdetect.h piFace.h  piGlow.h  piNes.h\
		scrollPhat.h

all:		$(DYNAMIC)

static:		$(STATIC)

$(STATIC):	$(OBJ)
	$Q echo "[Link (Static)]"
	$Q ar rcs $(STATIC) $(OBJ)
	$Q ranlib $(STATIC)
#	@size   $(STATIC)

$(DYNAMIC):	$(OBJ)
	$Q echo "[Link (Dynamic)]"
	$Q $(CC) -shared $(LDFLAGS) -Wl,-soname,$(BASE_NAME).so.$(DYN_VERS_MAJ) -o $(BASE_NAME).so -lpthread $(OBJ)

.c.o:
	$Q echo [Compile] $<
	$Q $(CC) -c $(CFLAGS) -fPIC $< -o $@

.PHONY:	clean
clean:
	$Q echo "[Clean]"
	$Q rm -f $(OBJ) $(OBJ_I2C) *~ core tags Makefile.bak $(BASE_NAME).so* $(BASE_NAME).a

.PHONY:	tags
tags:	$(SRC)
	$Q echo [ctags]
	$Q ctags $(SRC)


.PHONY:	install
install:	$(DYNAMIC)
	$Q echo "[Install Headers]"
	$Q install -m 0755 -d						$(INCLUDE_DIR)
	$Q install -m 0644 $(HEADERS)					$(INCLUDE_DIR)
	$Q echo "[Install Dynamic Lib]"
	$Q install -m 0755 -d						$(LIB_DIR)
	$Q install -m 0755 $(BASE_NAME).so				$(LIB_DIR)/$(DYNAMIC)
	$Q ln -sf $(DYNAMIC)						$(LIB_DIR)/$(BASE_NAME).so
	$Q ln -sf $(DYNAMIC)						$(LIB_DIR)/$(BASE_NAME).so.$(DYN_VERS_MAJ)

.PHONY:	install-static
install-static:	$(STATIC)
	$Q echo "[Install Headers]"
	$Q install -m 0755 -d						$(INCLUDE_DIR)
	$Q install -m 0644 $(HEADERS)					$(INCLUDE_DIR)
	$Q echo "[Install Static Lib]"
	$Q install -m 0755 -d						$(LIB_DIR)
	$Q install -m 0755 $(STATIC)					$(LIB_DIR)

.PHONY:	install-deb
install-deb:	$(DYNAMIC)
	$Q echo "[Install Headers: deb]"
	$Q install -m 0755 -d							~/wiringPi/debian-template/wiringPi/usr/include
	$Q install -m 0644 $(HEADERS)						~/wiringPi/debian-template/wiringPi/usr/include
	$Q echo "[Install Dynamic Lib: deb]"
	install -m 0755 -d							~/wiringPi/debian-template/wiringPi/usr/lib
	install -m 0755 libwiringPiDev.so.$(VERSION)				~/wiringPi/debian-template/wiringPi/usr/lib/libwiringPiDev.so.$(VERSION)
	ln -sf ~/wiringPi/debian-template/wiringPi/usr/lib/libwiringPiDev.so.$(VERSION)	~/wiringPi/debian-template/wiringPi/usr/lib/libwiringPiDev.so

.PHONY:	uninstall
uninstall:
	$Q echo "[UnInstall]"
	$Q cd $(INCLUDE_DIR) && rm -f $(HEADERS)
	$Q rm -f $(LIB_DIR)/$(STATIC)
	$Q rm -f $(LIB_DIR)/$(DYNAMIC)
	$Q rm -f $(LIB_DIR)/$(BASE_NAME).so
	$Q rm -f $(LIB_DIR)/$(BASE_NAME).so.$(DYN_VERS_MAJ)


.PHONY:	depend
depend:
	makedepend -Y $(SRC)

# DO NOT DELETE

ds1302.o: ds1302.h
maxdetect.o: maxdetect.h
piNes.o: piNes.h
gertboard.o: gertboard.h
piFace.o: piFace.h
lcd128x64.o: font.h lcd128x64.h
lcd.o: lcd.h
scrollPhat.o: scrollPhatFont.h scrollPhat.h
piGlow.o: piGlow.h
