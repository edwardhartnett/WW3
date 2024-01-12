# This is the CMake file for building the docs directory of WW3
#
# Ed Hartnett 1/12/24

IF(ENABLE_DOCS)

  # Set these so they can be used in the Doxyfile.
  SET(abs_top_srcdir "${CMAKE_SOURCE_DIR}")
  SET(abs_top_builddir "${CMAKE_BINARY_DIR}")

  # Create doxyfile.
  CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile @ONLY)

  # Add doxygen build to ALL target.
  ADD_CUSTOM_TARGET(ww3_doc ALL
    ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Generating API Documentation with Doxygen" VERBATIM)

ENDIF(ENABLE_DOCS)
