
gnc_add_swig_guile_command (swig-bar-c
    SWIG_BAR_C swig-bar.c
    ${CMAKE_CURRENT_SOURCE_DIR}/bar.i bar.h
)

ADD_LIBRARY(bar EXCLUDE_FROM_ALL bar.c bar.h)
ADD_LIBRARY(gncmodbar EXCLUDE_FROM_ALL gnc-mod-bar.c)
SET_SOURCE_FILES_PROPERTIES (gnc-mod-bar.c PROPERTY OBJECT_DEPENDS ${SWIG_BAR_C})

TARGET_LINK_LIBRARIES(gncmodbar bar gnc-module)
TARGET_INCLUDE_DIRECTORIES(gncmodbar PRIVATE
  ${CMAKE_BINARY_DIR}/common
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_SOURCE_DIR}/common
  ${CMAKE_SOURCE_DIR}/libgnucash/gnc-module
  ${GLIB2_INCLUDE_DIRS}
  ${GUILE_INCLUDE_DIRS}
)

SET_TARGET_PROPERTIES(bar gncmodbar PROPERTIES
LIBRARY_OUTPUT_DIRECTORY ${LIBDIR_BUILD}/gnucash/test
ARCHIVE_OUTPUT_DIRECTORY ${LIBDIR_BUILD}/gnucash/test
RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

SET_DIST_LIST(mod_bar_DIST CMakeLists.txt bar.c gnucash/bar.scm bar.h bar.i gnc-mod-bar.c)
