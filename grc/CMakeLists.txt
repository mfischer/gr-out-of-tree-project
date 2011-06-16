FILE(GLOB GRC_SRCS ${CMAKE_CURRENT_SOURCE_DIR}/*.xml)

INSTALL(FILES ${GRC_SRCS} DESTINATION ${CMAKE_INSTALL_PREFIX}/share/gnuradio/grc/blocks)                                                                                               
