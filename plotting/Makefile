PROGRAM       = DrawingClasses
version       = EEC
CXX           = g++
CXXFLAGS      = -O -Wall -fPIC -D$(version) # Add -g for debugger
LDFLAGS       = -O
#############################################
# -bind_at_load helps to remove linker error
############################################
CXXFLAGS += $(shell root-config --cflags)
LDFLAGS  = $(shell root-config --libs)

CXXFLAGS  += $(INCS)

HDRSDICT = SplitCanvas.h
HDRS	+= $(HDRSDICT) AlgorithmLibrary.h EECHistogramManager.h EECCard.h EECDrawer.h EECComparingDrawer.h EECBackgroundScale.h EECSignalToBackgroundUnfoldingScale.h EECUnfoldConfiguration.h SystematicUncertaintyOrganizer.h HybridModelHistogramManager.h HolguinHistogramManager.h CoLBTHistogramManager.h JewelHistogramManager.h


SRCS = $(HDRS:.h=.cxx) nanoDict.cc
OBJS = $(HDRS:.h=.o) nanoDict.o

all:            $(PROGRAM)

$(PROGRAM) :    $(OBJS)
		@echo "Linking $(PROGRAM) ..."
		$(CXX) -shared -lEG -L$(PWD) $(CXXFLAGS) $(OBJS) $(LDFLAGS) -o $(PROGRAM).so
		@echo "done"

%.cxx:

%: %.cxx
#  commands to execute (built-in):
	$(LINK.cc) $^ $(CXXFLAGS) $(LOADLIBES) $(LDLIBS) -o $@

%.o: %.cxx %.h
#  commands to execute (built-in):
	$(COMPILE.cc) $(OUTPUT_OPTION) $<

nanoDict.o: nanoDict.cc $(HDRSDICT)
	$(COMPILE.cc) $(OUTPUT_OPTION) $<

clean:
		rm -f $(OBJS) $(PROGRAM).o $(PROGRAM).so nanoDict.cc nanoDict_rdict.pcm $(PROGRAM)_rdict.pcm

cl:  clean $(PROGRAM)

# Dictionary is needed for all classes inheriting TObject from root
nanoDict.cc: $(HDRSDICT) LinkDef.h
		@echo "Generating dictionary ..."
		@rm -f nanoDict.cc nanoDict.hh nanoDict.h
		@rootcling -f nanoDict.cc -s $(PROGRAM).so -c -D$(version) $(HDRSDICT) LinkDef.h
