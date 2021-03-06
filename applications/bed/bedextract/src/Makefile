MAIN	                = ../../../..
MAINAPPDIR              = ../..
CC                      = gcc
CXX                     = g++
HEAD                    = $(MAIN)/interfaces/general-headers
LIB1                    = $(MAIN)/interfaces/src/data/measurement
LIB2                    = $(MAIN)/interfaces/src/utility
LIB3                    = $(MAIN)/interfaces/src/data/starch
THISDIR	                = ${shell pwd}
PARTY3                  = ${THISDIR}/$(MAIN)/third-party
OBJDIR                  = objects
LIBJANSSON              = libjansson.a
LIBBZIP2                = libbz2.a
LIBZLIB                 = libz.a
LOCALJANSSONDIR         = ${PARTY3}/jansson
LOCALJANSSONLIBDIR      = ${LOCALJANSSONDIR}/lib
LOCALJANSSONINCDIR      = ${LOCALJANSSONDIR}/include
LOCALJANSSONLIBPATH     = ${LOCALJANSSONLIBDIR}/${LIBJANSSON}
LOCALBZIP2DIR           = ${PARTY3}/bzip2
LOCALBZIP2LIBDIR        = ${LOCALBZIP2DIR}
LOCALBZIP2LIBPATH       = ${LOCALBZIP2LIBDIR}/${LIBBZIP2}
LOCALBZIP2INCDIR        = ${LOCALBZIP2DIR}
LOCALZLIBDIR            = ${PARTY3}/zlib
LOCALZLIBLIBDIR         = ${LOCALZLIBDIR}
LOCALZLIBLIBPATH        = ${LOCALZLIBLIBDIR}/${LIBZLIB}
LOCALZLIBINCDIR         = ${LOCALZLIBDIR}
BOOSTLIBDIR             = $(PARTY3)/boost
INCLUDES                = -iquote$(HEAD) -I${BOOSTLIBDIR}
NONSTATICFLAGS          = -Wall -pedantic -O3 ${INCLUDES} -std=c++11

ifeq ($(wildcard .forcestatic),)
	SFLAGS = -I$(HEAD)
else
	SFLAGS = -static -s -I$(HEAD)
endif

FLAGS                   = $(SFLAGS) $(NONSTATICFLAGS) $(OBJDIR)/NaN.o $(OBJDIR)/Formats.o $(OBJDIR)/starchConstants.o $(OBJDIR)/starchFileHelpers.o $(OBJDIR)/starchHelpers.o $(OBJDIR)/starchMetadataHelpers.o $(OBJDIR)/unstarchHelpers.o $(OBJDIR)/starchSha1Digest.o $(OBJDIR)/starchBase64Coding.o -L${LOCALJANSSONLIBDIR} -L${LOCALBZIP2LIBDIR} -L${LOCALZLIBLIBDIR} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
DFLAGS                  = $(SFLAGS) -g -O0 -std=c++11 -Wall -fno-inline -pedantic $(OBJDIR)/NaN.o $(OBJDIR)/Formats.o $(OBJDIR)/starchConstants.o $(OBJDIR)/starchFileHelpers.o $(OBJDIR)/starchHelpers.o $(OBJDIR)/starchMetadataHelpers.o $(OBJDIR)/unstarchHelpers.o $(OBJDIR)/starchSha1Digest.o $(OBJDIR)/starchBase64Coding.o -L${LOCALJANSSONLIBDIR} -L${LOCALBZIP2LIBDIR} -L${LOCALZLIBLIBDIR} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
GPROFFLAGS              = $(SFLAGS) -O -std=c++11 -Wall -pedantic -pg $(OBJDIR)/NaN.o $(OBJDIR)/Formats.o $(OBJDIR)/starchConstants.o $(OBJDIR)/starchFileHelpers.o $(OBJDIR)/starchHelpers.o $(OBJDIR)/starchMetadataHelpers.o $(OBJDIR)/unstarchHelpers.o $(OBJDIR)/starchSha1Digest.o $(OBJDIR)/starchBase64Coding.o -L${LOCALJANSSONLIBDIR} -L${LOCALBZIP2LIBDIR} -L${LOCALZLIBLIBDIR} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
SOURCE1                 = ExtractRows.cpp
BIN                     = ../bin
PROG                    = bedextract

.cpp.o:; $(CXX) -c $(FLAGS) $<

run: dependencies
	mkdir -p $(BIN) && $(CXX) -o $(BIN)/$(PROG) $(FLAGS) ${LOCALJANSSONLIBPATH} ${LOCALBZIP2LIBPATH} ${LOCALZLIBLIBPATH} $(SOURCE1)

gprof: dependencies
	mkdir -p $(BIN) && $(CXX) -o $(BIN)/gprof.$(PROG) $(GPROFFLAGS) ${LOCALJANSSONLIBPATH} ${LOCALBZIP2LIBPATH} ${LOCALZLIBLIBPATH} $(SOURCE1)

nonstatic: dependencies
	mkdir -p $(BIN) && $(CXX) -o $(BIN)/nonstatic.$(PROG) $(NONSTATICFLAGS) $(SOURCE1) $(OBJDIR)/NaN.o $(OBJDIR)/Formats.o

jansson:
	test -s ${LOCALJANSSONLIBPATH} || { cd ${MAIN} && make jansson_support_c && cd ${LOCALJANSSONDIR} && ${LOCALJANSSONDIR}/configure --prefix=${LOCALJANSSONDIR} && make && make install && cd ${THISDIR}; }

bzip2:
	test -s ${LOCALBZIP2LIBPATH} || { cd ${MAIN} && make bzip2_support_c && cd ${LOCALBZIP2DIR} && make libbz2.a && cd ${THISDIR}; }

zlib:
	test -s ${LOCALZLIBLIBPATH} || { cd ${MAIN} && make zlib_support_c && cd ${LOCALZLIBDIR} && ${LOCALZLIBDIR}/configure --static && make && cd ${THISDIR}; }

boost:
	test -d ${BOOSTLIBDIR} || { cd ${MAIN} && make boost_support_c && cd ${THISDIR}; }

dependencies: jansson bzip2 zlib boost
ifneq ($(wildcard .forcestatic),)
	rm -rf .forcestatic
endif
	mkdir -p $(OBJDIR)
	$(CXX) -c $(NONSTATICFLAGS) $(LIB1)/NaN.cpp -o $(OBJDIR)/NaN.o -iquote${HEAD}
	$(CXX) -c $(NONSTATICFLAGS) $(LIB2)/Formats.cpp -o $(OBJDIR)/Formats.o -iquote${HEAD}
	$(CXX) -c $(NONSTATICFLAGS) $(LIB3)/starchConstants.c -o $(OBJDIR)/starchConstants.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
	$(CXX) -c $(NONSTATICFLAGS) $(LIB3)/starchFileHelpers.c -o $(OBJDIR)/starchFileHelpers.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
	$(CXX) -c $(NONSTATICFLAGS) $(LIB3)/starchHelpers.c -o $(OBJDIR)/starchHelpers.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
	$(CXX) -c $(NONSTATICFLAGS) $(LIB3)/starchMetadataHelpers.c -o $(OBJDIR)/starchMetadataHelpers.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
	$(CXX) -c $(NONSTATICFLAGS) $(LIB3)/unstarchHelpers.c -o $(OBJDIR)/unstarchHelpers.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
	$(CXX) -c ${NONSTATICFLAGS} ${LIB3}/starchSha1Digest.c -o  ${OBJDIR}/starchSha1Digest.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}
	$(CXX) -c ${NONSTATICFLAGS} ${LIB3}/starchBase64Coding.c -o  ${OBJDIR}/starchBase64Coding.o -iquote${HEAD} -I${LOCALJANSSONINCDIR} -I${LOCALBZIP2INCDIR} -I${LOCALZLIBINCDIR}

debug: dependencies
	mkdir -p $(BIN) && $(CXX) -o $(BIN)/debug.$(PROG) $(DFLAGS) ${LOCALJANSSONLIBPATH} ${LOCALBZIP2LIBPATH} ${LOCALZLIBLIBPATH} $(SOURCE1)

clean:
	rm -rf $(OBJDIR) $(BIN)
