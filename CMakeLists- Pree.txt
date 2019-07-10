cmake_minimum_required(VERSION 2.8.12.2)
project(hellotbb)



# We want to use C++ lambdas.
# To avoid confusing compilation errors, we request C++11 explicitly on g++ and clang++

IF ( CMAKE_COMPILER_IS_GNUCXX )
  SET(CMAKE_CXX_FLAGS "-std=c++11 -Wall")
  #SET(CMAKE_CXX_FLAGS_DISTRIBUTION "-O3")
ENDIF()

# IF ( "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" )
IF ( CMAKE_CXX_COMPILER_ID STREQUAL "Clang" )
  SET(CMAKE_CXX_FLAGS "-std=c++11 -Wall")
ENDIF()


## Enable debug symbols by default
#IF(CMAKE_BUILD_TYPE STREQUAL "")
#  set(CMAKE_BUILD_TYPE Debug)
#ENDIF()
## (you can also set it on the command line: -DCMAKE_BUILD_TYPE=Release)

## Set version information in a config.h file
#SET(myproject_VERSION_MAJOR 1)
#SET(myproject_VERSION_MINOR 0)
#configure_file(
#  "${PROJECT_SOURCE_DIR}/config.h.in"
#  "${PROJECT_BINARY_DIR}/config.h"
#  )
#INCLUDE_DIRECTORIES("${PROJECT_BINARY_DIR}")

## Define sources and executable

ADD_EXECUTABLE(hellotbb hellotbb.cpp)



SET(Boost_USE_MULTITHREAD OFF)   ## from http://stackoverflow.com/q/7855174
## see www.cmake.org/cmake/help/git-master/module/FindBoost.html

## These two lines may be useful if CMake is unable to find Boost:
# SET(Boost_DEBUG ON)
# SET(Boost_DETAILED_FAILURE_MSG ON)

IF( WIN32  AND  (NOT CYGWIN) )
  SET(Boost_USE_STATIC_LIBS ON) ## there's also Boost_USE_STATIC_RUNTIME, which does.... something else?
  MESSAGE( "Note: attempting to use static linking for Boost" )
ENDIF()

SET(Boost_MIN_VERSION "1.54.0")
FIND_PACKAGE(Boost ${Boost_MIN_VERSION} REQUIRED) #  COMPONENTS  program_options)
#SET(Boost_EXACT_VERSION "1.55.0")
#FIND_PACKAGE(Boost ${Boost_EXACT_VERSION} EXACT REQUIRED  COMPONENTS  program_options)
  ## if this fails, CMake will print an error message and then bail
## Things like static_assert and type_traits aren't 'components'.
## Only libraries with binaries are 'components':
## system filesystem program_options


SET(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake_modules" ${CMAKE_MODULE_PATH})
FIND_PACKAGE(TBB 4.4 REQUIRED
             # COMPONENTS # [tbbmalloc] [tbb_preview]
             # [OPTIONAL_COMPONENTS [tbbmalloc] [tbb_preview]]
			 ) 
INCLUDE_DIRECTORIES(${TBB_INCLUDE_DIRS})
# No good: TARGET_LINK_LIBRARIES(hellotbb  ${TBB_LIBRARIES}) ## remember it's a DLL: we still have to worry about that
TARGET_LINK_LIBRARIES(hellotbb  ${TBB_tbb_LIBRARY_RELEASE}) ## tbb.dll
TARGET_LINK_LIBRARIES(hellotbb  ${TBB_tbb_LIBRARY_DEBUG})   ## tbb_debug.dll

## Detect and add libpq
#FIND_PACKAGE(PostgreSQL REQUIRED ) # pq) ## no way to just ask for libpq?????
#if(PostgreSQL_FOUND)
#  INCLUDE_DIRECTORIES(${PostgreSQL_INCLUDE_DIRS})
#  TARGET_LINK_LIBRARIES(hellotbb  ${PostgreSQL_LIBRARIES}) ###### TODO can we pare this down? no need to go overboard just for libpq
#endif()


## Install target
INSTALL(TARGETS hellotbb  DESTINATION bin)


## CPack packaging
INCLUDE(InstallRequiredSystemLibraries)
# set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/COPYING")
SET(CPACK_PACKAGE_VERSION_MAJOR "${myproject_VERSION_MAJOR}")
SET(CPACK_PACKAGE_VERSION_MINOR "${myproject_VERSION_MINOR}")
INCLUDE(CPack)

