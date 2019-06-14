cmake_minimum_required(VERSION 3.4.3)
project(xml2)

set(VERSION 2.9.3)
set(LIBXML_VERSION_NUMBER 20903)

set(SOURCES
    SAX.c entities.c encoding.c error.c parserInternals.c
    parser.c tree.c hash.c list.c xmlIO.c xmlmemory.c uri.c
    valid.c xlink.c HTMLparser.c HTMLtree.c debugXML.c xpath.c
    xpointer.c xinclude.c nanohttp.c nanoftp.c
    catalog.c globals.c threads.c c14n.c xmlstring.c buf.c
    xmlregexp.c xmlschemas.c xmlschemastypes.c xmlunicode.c
    xmlreader.c relaxng.c dict.c SAX2.c
    xmlwriter.c legacy.c chvalid.c pattern.c xmlsave.c
    xmlmodule.c schematron.c xzlib.c
)

foreach(Option  
        WITH_TRIO WITH_THREADS WITH_THREAD_ALLOC WITH_TREE WITH_OUTPUT WITH_PUSH WITH_READER
        WITH_PATTERN WITH_SAX1 WITH_FTP WITH_HTTP WITH_VALID WITH_HTML WITH_LEGACY WITH_C14N
        WITH_CATALOG WITH_DOCB WITH_XPATH WITH_XPTR WITH_XINCLUDE WITH_ICONV WITH_ICU WITH_ISO8859X
        WITH_DEBUG WITH_MEM_DEBUG WITH_RUN_DEBUG WITH_REGEXPS WITH_SCHEMAS WITH_SCHEMATRON 
        WITH_MODULES WITH_ZLIB WITH_LZMA WITH_WRITER)
    option(XML2_${Option} "" OFF)
    if(${XML2_${Option}})
        set(${Option} 1)
    else()
        set(${Option} 0)
    endif()
endforeach()

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/include/libxml/xmlversion.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/include/libxml/xmlversion.h
               @ONLY)

file(GENERATE OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/include/config.h CONTENT "
    #include <stddef.h>
    #include <stdint.h>
    #include <ctype.h>
    #include <unistd.h>
")

add_library(xml2 ${SOURCES})
target_include_directories(xml2 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(xml2 PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/include)
