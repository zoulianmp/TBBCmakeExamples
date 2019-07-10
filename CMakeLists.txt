


project(TBBExamples)

SET(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake_modules" ${CMAKE_MODULE_PATH})

#message("CMAKE_MODULE_PATH: ${CMAKE_MODULE_PATH}")

include(${CMAKE_SOURCE_DIR}/cmake_modules/ManualSetTBB.cmake)

#include(ManualSetTBB.cmake)

#FIND_PACKAGE(TBB 4.4 REQUIRED
             # COMPONENTS # [tbbmalloc] [tbb_preview]
             # [OPTIONAL_COMPONENTS [tbbmalloc] [tbb_preview]]
#			 ) 

message("TBB_LIBRARIES in main : ${TBB_LIBRARIES} ")

add_subdirectory(GettingStarted)

add_subdirectory(parallel_reduce)
add_subdirectory(parallel_do)
add_subdirectory(pipeline)

add_subdirectory(task)
add_subdirectory(task_group)
