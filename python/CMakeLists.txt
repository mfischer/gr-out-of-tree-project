EXECUTE_PROCESS(
    COMMAND ${PYTHON_EXECUTABLE} -c "from distutils import sysconfig; print sysconfig.get_python_lib(prefix='${CMAKE_INSTALL_PREFIX}')"
    OUTPUT_VARIABLE PYTHON_DIR OUTPUT_STRIP_TRAILING_WHITESPACE
)


################################################################################
# Add your tests here (don't put the qa_ ) in front
################################################################################

SET(PYTHON_QA )

################################################################################
# Here we add the tests you added above
################################################################################

FOREACH(testcase ${PYTHON_QA})
ADD_TEST(${testcase}_py
	${CMAKE_CURRENT_SOURCE_DIR}/test_runner.py --test ${CMAKE_CURRENT_SOURCE_DIR}/qa_${testcase}.py --path ${CMAKE_BINARY_DIR}/swig)
ENDFOREACH(testcase ${PYTHON_QA})

################################################################################
# Here we add our python sourcecode that needs to be installed
################################################################################

#TODO add all
SET(OUT_OF_TREE_PY_SRCS
	__init__.py)

INSTALL(FILES ${OUT_OF_TREE_PY_SRCS} DESTINATION ${PYTHON_DIR}/out_of_tree)
