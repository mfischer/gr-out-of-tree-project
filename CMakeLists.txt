PROJECT(GR-OUT_OF_TREE)
ENABLE_LANGUAGE(CXX)

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

IF(UNIX AND ("${CMAKE_SIZEOF_VOID_P}" EQUAL "8"))
	LIST(APPEND BOOST_LIBRARYDIR "/usr/lib64") #fedora 64-bit fix
	SET(LIB_POSTFIX 64)
ENDIF(UNIX AND ("${CMAKE_SIZEOF_VOID_P}" EQUAL "8"))

LIST(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMake)

###############################################################################
# Search for the required packages
###############################################################################

FIND_PACKAGE(Boost REQUIRED)
FIND_PACKAGE(CppUnit REQUIRED)

INCLUDE(FindPkgConfig)
PKG_CHECK_MODULES(GNURADIO_CORE REQUIRED gnuradio-core)

INCLUDE_DIRECTORIES(${GNURADIO_CORE_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(SYSTEM ${CMAKE_SOURCE_DIR}/lib ${CMAKE_BINARY_DIR}/lib)

SUBDIRS(lib swig python grc apps doc)

CONFIGURE_FILE(${CMAKE_SOURCE_DIR}/gnuradio-out_of_tree.pc.in ${CMAKE_BINARY_DIR}/gnuradio-out_of_tree.pc @ONLY)

ENABLE_TESTING()

INSTALL(FILES ${CMAKE_BINARY_DIR}/gnuradio-out_of_tree.pc DESTINATION ${CMAKE_INSTALL_PREFIX}/lib${LIB_POSTFIX}/pkgconfig)
