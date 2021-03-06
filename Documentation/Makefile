# where to get our style sheet from
#STYLESHEETARCHIVE = http://docbook.sourceforge.net/snapshot/docbook-xsl-snapshot.zip
STYLESHEETARCHIVE = http://downloads.sourceforge.net/project/docbook/docbook-xsl/1.78.1/docbook-xsl-1.78.1.zip
# local stylesheet dir as it takes ages over a network connection
STYLESHEETDIR = docbook-xsl-1.78.1/xhtml/

# stylesheet to use
STYLESHEET = docbook.xsl 

#  args for xsltproc
XSLARGS = --stringparam  funcsynopsis.style "ansi" \
	--stringparam  toc.section.depth "3" \
  --stringparam generate.toc "book toc,title,figure,table,example,equation" \
	--stringparam html.stylesheet ofxStyle.css

# .h files we are scanning for specially formated property definitions
# We do this so that the documentation in the source agrees with the 
# documentation in the XML, always.
PROPERTY_HEADER_FILES = ../include/ofxParam.h \
	../include/ofxImageEffect.h \
	../include/ofxInteract.h \
	../include/ofxCore.h \
	../include/ofxKeySyms.h \
	../include/ofxParametricParam.h \
	../include/ofxOpenGLRender.h

# files generate from the C comments in the actual headers and used as reference
PROPERY_SUITE_REFERENCE_FILES = \
	ofxPropertySuiteReference.xml \
	ofxMemorySuiteReference.xml \
	ofxParametersSuiteReference.xml \
	ofxThreadingSuiteReference.xml \
	ofxImageEffectSuiteReference.xml \
	ofxProgressSuiteReference.xml \
	ofxInteractSuiteReference.xml \
	ofxTimeLineSuiteReference.xml \
	ofxMessageSuiteV1Reference.xml \
	ofxMessageSuiteV2Reference.xml \
	ofxParametricParamReference.xml \
	ofxOpenGLRenderSuiteReference.xml

all : chunks ofxProgrammingGuide.html ofxProgrammingReference.html

nochunks : ofxProgrammingGuide.html ofxProgrammingReference.html

chunks :
	rm -f *.html
	make STYLESHEET=chunk.xsl chunkreference chunkguide

chunkguide : ofxProgrammingGuide.html
	rm -rf Guide
	mkdir -p Guide
	mv *.html Guide

chunkreference : ofxProgrammingReference.html
	rm -rf Reference
	mkdir -p Reference
	mv *.html Reference

.PHONY: docbook-xsl
docbook-xsl :
	wget --output-document=docbook-xsl-download.zip '$(STYLESHEETARCHIVE)'; \
	unzip -q -o docbook-xsl-download.zip; \
	rm -f docbook-xsl-download.zip

# Common srcs
COMMON_SRCS = header.xml \
	ofxPackaging.xml

# programming guide, all in one
GUIDE_SRCS = header.xml ofxProgrammingGuide.xml 

ofxProgrammingGuide.html : $(GUIDE_SRCS) docbook-xsl
	cat  header.xml ofxProgrammingGuide.xml > tmp.xml
	-xsltproc -o $@ $(XSLARGS) $(STYLESHEETDIR)/$(STYLESHEET) tmp.xml
	rm -f tmp.xml

# programming reference, all in one 
REF_SRCS = ofxProgrammingReference.xml \
	$(COMMON_SRCS) \
	ofxPropertiesReference.xml \
	ofxPropertiesByObject.xml \
	$(PROPERY_SUITE_REFERENCE_FILES) \
	imageEffectActionsChapter.xml \
	interactActionsChapter.xml \
	clipPreferencesChapter.xml \
	ofxParameterChapter.xml 

ofxProgrammingReference.html : $(REF_SRCS) docbook-xsl
	cat  header.xml $(<:.html=.xml) > tmp.xml
	-xsltproc -o $@ $(XSLARGS) $(STYLESHEETDIR)/$(STYLESHEET) tmp.xml
	rm -f tmp.xml

# generate property definitions from the property headers
ofxPropertiesReference.xml : extractPropertyDefinitions.perl $(PROPERTY_HEADER_FILES)
	mkdir -p props
	./extractPropertyDefinitions.perl
	cat props/* > ofxPropertiesReference.xml
	rm -rf props

# Extract references on the property suite from the .h file
ofxPropertySuiteReference.xml : ../include/ofxProperty.h
	./getSuiteReference.perl OfxPropertySuiteV1 $^ > $@

# Extract references on the memory suite from the .h file
ofxMemorySuiteReference.xml : ../include/ofxMemory.h
	./getSuiteReference.perl OfxMemorySuiteV1 $^ > $@

# Extract references on the parameters suite from the .h file
ofxParametersSuiteReference.xml : ../include/ofxParam.h
	./getSuiteReference.perl OfxParameterSuiteV1 $^ > $@

# Extract references on the parameters suite from the .h file
ofxThreadingSuiteReference.xml : ../include/ofxMultiThread.h
	./getSuiteReference.perl OfxMultiThreadSuiteV1 $^ > $@

# Extract references on the parameters suite from the .h file
ofxInteractSuiteReference.xml : ../include/ofxInteract.h
	./getSuiteReference.perl OfxInteractSuiteV1 $^ > $@

# Extract references on the parameters suite from the .h file
ofxImageEffectSuiteReference.xml : ../include/ofxImageEffect.h
	./getSuiteReference.perl OfxImageEffectSuiteV1 $^ > $@

# Extract references on the progress suite
ofxProgressSuiteReference.xml : ../include/ofxProgress.h
	./getSuiteReference.perl OfxProgressSuiteV1 $^ > $@

# Extract references on the progress suite
ofxTimeLineSuiteReference.xml : ../include/ofxTimeLine.h
	./getSuiteReference.perl OfxTimeLineSuiteV1 $^ > $@

# Extract references on the progress suite
ofxMessageSuiteV1Reference.xml : ../include/ofxMessage.h
	./getSuiteReference.perl OfxMessageSuiteV1 $^ > $@

ofxMessageSuiteV2Reference.xml : ../include/ofxMessage.h
	./getSuiteReference.perl OfxMessageSuiteV2 $^ > $@

ofxParametricParamReference.xml : ../include/ofxParametricParam.h
	./getSuiteReference.perl OfxParametricParameterSuiteV1 $^ > $@

ofxOpenGLRenderSuiteReference.xml : ../include/ofxOpenGLRender.h
	./getSuiteReference.perl OfxImageEffectOpenGLRenderSuiteV1 $^ > $@

clean :
	rm -rf *.html props $(PROPERY_SUITE_REFERENCE_FILES) ofxPropertiesReference.xml Reference Guide docbook-xsl
