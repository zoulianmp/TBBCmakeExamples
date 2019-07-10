# Copyright (c) 2019 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.



project(parallel_reduce)


#----------------------------------------------------------------------------
# Locate sources and headers for this project
# NB: headers are included so they will show up in IDEs
#
##file(GLOB sources ${PROJECT_SOURCE_DIR}/*.cpp)
##file(GLOB headers ${PROJECT_SOURCE_DIR}/*.h)

INCLUDE_DIRECTORIES(${TBB_INCLUDE_DIRS})

set (sourcename  convex_hull_bench)

#set (sourcename convex_hull_sample)

 
 

add_executable(${sourcename}  ${sourcename}.cpp convex_hull.h)
target_link_libraries(${sourcename}  ${TBB_LIBRARIES})   

 
# find_package will search for available TBBConfig using variables CMAKE_PREFIX_PATH and TBB_DIR.
#find_package(TBB REQUIRED tbb)

#message(" fabDESTINATION DESTINATION   ${PROJECT_BINARY_DIR}/Debug ")



file(COPY  ${TBB_DLLS} DESTINATION   "${PROJECT_BINARY_DIR}/Debug")

