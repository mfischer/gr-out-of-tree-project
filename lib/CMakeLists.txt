SET(CXX_LDFLAGS ${GNURADIO_CORE_LDFLAGS})
SET(CMAKE_CXX_FLAGS -fPIC)

###############################################################################
# Our C++ sources go here
###############################################################################

SET(CXX_SRCS 
	${CMAKE_CURRENT_SOURCE_DIR}/out_of_tree_dummy.cc)

###############################################################################
# Our QA sources go here
###############################################################################

SET(CXX_QA_SRCS
	${CMAKE_CURRENT_SOURCE_DIR}/qa_out_of_tree.cc)

###############################################################################
# Our CXX unit tests go here
###############################################################################

SET(CXX_QA )

################################################################################
# Here we add the tests you added above
################################################################################

FOREACH(testcase ${CXX_QA})
	ADD_TEST(${testcase}_cxx ${EXECUTABLE_OUTPUT_PATH}/test_runner qa_${testcase})
ENDFOREACH(testcase ${CXX_QA})

###############################################################################
# Here we finally create our targets
###############################################################################

###############################################################################
# Our main library
###############################################################################

ADD_LIBRARY(gnuradio-out_of_tree SHARED ${CXX_SRCS})
TARGET_LINK_LIBRARIES(gnuradio-out_of_tree
		${CXX_LDFLAGS})

ADD_LIBRARY(gnuradio-out_of_tree-static ${CXX_SRCS})
TARGET_LINK_LIBRARIES(gnuradio-out_of_tree-static
		${CXX_LDFLAGS})

# we do this to have the same name for our static library
SET_TARGET_PROPERTIES(gnuradio-out_of_tree-static
		PROPERTIES
		OUTPUT_NAME
		"gnuradio-out_of_tree")

SET_TARGET_PROPERTIES(gnuradio-out_of_tree-static
	PROPERTIES
	PREFIX
	"lib")

###############################################################################
# Our QA library
###############################################################################

ADD_LIBRARY(gnuradio-out_of_tree-qa ${CXX_QA_SRCS})

TARGET_LINK_LIBRARIES(gnuradio-out_of_tree-qa
	gnuradio-out_of_tree
	rt
	pthread)

###############################################################################
# Our testrunner
###############################################################################

ADD_EXECUTABLE(test_runner ${CMAKE_CURRENT_SOURCE_DIR}/test_runner.cc)
TARGET_LINK_LIBRARIES(test_runner
	gnuradio-out_of_tree-static
	gnuradio-out_of_tree-qa
	${CPPUNIT_LIBRARIES}
	${CPPUNIT}
	${CXX_LDFLAGS})

###############################################################################
# Install the stuff
###############################################################################

IF(UNIX AND ("${CMAKE_SIZEOF_VOID_P}" EQUAL "8") AND (EXISTS "${CMAKE_INSTALL_PREFIX}/lib64"))
	LIST(APPEND BOOST_LIBRARYDIR "/usr/lib64") #fedora 64-bit fix
	SET(LIB_POSTFIX 64)
ENDIF(UNIX AND ("${CMAKE_SIZEOF_VOID_P}" EQUAL "8") AND (EXISTS "${CMAKE_INSTALL_PREFIX}/lib64"))


INSTALL(TARGETS gnuradio-out_of_tree gnuradio-out_of_tree-static
	LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib${LIB_POSTFIX}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib${LIB_POSTFIX}
	RUNTIME DESTINATION	${CMAKE_INSTALL_PREFIX}/lib${LIB_POSTFIX})

FILE(GLOB CXX_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/out_of_tree_*.h)
INSTALL(FILES ${CXX_HDRS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/out_of_tree)
