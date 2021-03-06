CC	= gcc
CFLAGS	= -g -O   -I/usr/X11R6/include -O0
LDFLAGS	= 
LIBS  =  -lSM -lICE  -L/usr/X11R6/lib -lm  -ltermcap -lX11 
prefix = /usr/local
BINDIR        = $(prefix)/bin
LIBLOC        = $(prefix)/lib/logo
MAKE = /usr/bin/make

# LIBLOC      = `pwd`

OBJS 	= coms.o error.o eval.o files.o graphics.o init.o intern.o \
	  libloc.o lists.o logodata.o main.o math.o mem.o paren.o parse.o \
	  print.o term.o wrksp.o xgraphics.o nographics.o

SRCS	= coms.c error.c eval.c files.c graphics.c init.c intern.c \
	  libloc.c lists.c logodata.c main.c math.c mem.c paren.c parse.c \
	  print.c term.c wrksp.c xgraphics.c nographics.c

HDRS	= globals.h logo.h xgraphics.h

all:	logo logolib/Messages helpfiles helpfiles/HELPCONTENTS logo-mode make-docs

mem.o:	mem.c
	$(CC) $(CFLAGS) -O0 -c mem.c

logo:	$(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) $(OBJS) $(LIBS) -o logo

tags:	$(SRCS)
	ctags --format=1 -N $(SRCS) $(HDRS)
# 	ctags -t $(SRCS) $(HDRS)

libloc.c:
	echo 'char *libloc="'$(LIBLOC)'/logolib";' > libloc.c
	echo 'char *helploc="'$(LIBLOC)'/helpfiles";' >> libloc.c
	echo 'char *cslsloc="'$(LIBLOC)'/csls";' >> libloc.c
	echo 'char *temploc="/tmp";' >> libloc.c
	echo 'char *separator="/";' >> libloc.c

logolib/Messages:	makelib Messages
	chmod +x makelib
	./makelib
	cp -f Messages logolib

helpfiles:
	mkdir helpfiles

helpfiles/HELPCONTENTS:	makehelp usermanual
	./makehelp
	sort helptemp | pr -5 -t -w80 >> helpfiles/HELPCONTENTS
	rm helptemp

makehelp:	makehelp.c
	$(CC) -o makehelp makehelp.c

clean:
	rm -f *.o libloc.c
	cd emacs; $(MAKE) clean

ship:
	rm -f config.h config.cache config.log config.status
	rm -f makefile makehelp logo *.o libloc.c
	cd emacs; $(MAKE) ship
	cd docs; $(MAKE) ship

install: all
	for d in $(BINDIR) $(LIBLOC) $(LIBLOC)/logolib $(LIBLOC)/helpfiles $(LIBLOC)/csls; do [ -d $$d ] || mkdir -p $$d || exit 1; done
	cp logo $(BINDIR)/.
	cp -f logolib/* $(LIBLOC)/logolib/.
	cp -f helpfiles/* $(LIBLOC)/helpfiles/.
	cp -f csls/* $(LIBLOC)/csls/.
	(cd emacs; prefix=$(prefix) LIBLOC=$(LIBLOC) BINDIR=$(BINDIR) $(MAKE) install)
	(cd docs; prefix=$(prefix) LIBLOC=$(LIBLOC) BINDIR=$(BINDIR) $(MAKE) install)
#	prefix=$(prefix); LIBLOC=$(LIBLOC); BINDIR=$(BINDIR); export prefix LIBLOC BINDIR; cd emacs; $(MAKE) install

logo-mode: 
	(cd emacs; prefix=$(prefix) LIBLOC=$(LIBLOC) BINDIR=$(BINDIR) $(MAKE))
#	@prefix=$(prefix); LIBLOC=$(LIBLOC); BINDIR=$(BINDIR); export prefix LIBLOC BINDIR; cd emacs; $(MAKE)

make-docs:
	(cd docs; prefix=$(prefix) LIBLOC=$(LIBLOC) $(MAKE) all)
