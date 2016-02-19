cmake_minimum_required(VERSION 2.8)
include(cmake/base.cmake)
set(PROJECT_NAME ltdl)
set(PROJECT_DESCRIPTION "A cmake friendly ltdl for Windows platform")
set(PROJECT_URL "")
project(${PROJECTdd_NAME} CXX)
setup_project(FALSE)

if(WIN32)
  add_library(ltdl SHARED src/ltdl.c src/ltdl.h)
  # Generally import ltdl symbols
  add_definitions(-DLIBLTDL_DLL_IMPORT)
  set_target_properties(ltdl PROPERTIES COMPILE_FLAGS "-DDLL_EXPORT
  -DHAVE_STDLIB_H=1 -DHAVE_ERRNO_H=1 -DHAVE_MEMORY_H=1 -DHAVE_CTYPE_H=1
  -DHAVE_STRING_H=1 -DHAVE_STDIO_H=1 -DHAVE_ASSERT_H=1 -D__WINDOWS__
  -D__WIN32__")
  install(FILES src/ltdl.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
  install(TARGETS ltdl
                    RUNTIME DESTINATION bin
                    LIBRARY DESTINATION lib
                    ARCHIVE DESTINATION lib)
  pkg_config_append_cflags(-DLIBLTDL_DLL_IMPORT)
  pkg_config_append_libs(ltdl)
else()
  MESSAGE(FATAL_ERROR "Fly, you fools!")
endif()

setup_project_finalize()
