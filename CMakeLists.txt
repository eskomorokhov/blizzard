PROJECT (blizzard)
CMAKE_MINIMUM_REQUIRED (VERSION 2.6)
INCLUDE (CMakeMacro.cmake)

INCLUDE_DIRECTORIES (src/)

USE_PACKAGE (expat expat.h)
USE_PACKAGE (ev ev.h PATH_SUFFIXES libev .) # PATH_SUFFIXES is for stupid CentOS RPM package
USE_PACKAGE (coda coda/coda.h)

AUX_SOURCE_DIRECTORY (src/blizzard SRC_BLIZZARD)
ADD_EXECUTABLE (blizzard ${SRC_BLIZZARD})
TARGET_LINK_LIBRARIES (blizzard ${LIB_coda} ${LIB_expat} ${LIB_ev} pthread)

IF (CMAKE_SYSTEM_NAME STREQUAL Linux)
  TARGET_LINK_LIBRARIES (blizzard dl)
ENDIF ()

INSTALL (FILES src/blizzard/plugin.hpp DESTINATION include/blizzard)
INSTALL (TARGETS blizzard DESTINATION bin)
INSTALL_TEMPLATE (src/blizzard/config.xml.in DESTINATION etc/blizzard)

# Example Module

AUX_SOURCE_DIRECTORY (blzmod_example SRC_BLZMOD_EXAMPLE)
ADD_LIBRARY (blzmod_example SHARED ${SRC_BLZMOD_EXAMPLE})
INSTALL (TARGETS blzmod_example DESTINATION lib)
INSTALL_TEMPLATE (blzmod_example/config.xml.in DESTINATION etc/blzmod_example)
INSTALL_TEMPLATE (blzmod_example/config_module.xml.in DESTINATION etc/blzmod_example)

