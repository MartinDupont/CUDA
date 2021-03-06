cmake_minimum_required(VERSION 3.16)

project( HistogramTest LANGUAGES CUDA CXX )

set( CMAKE_CXX_STANDARD 17 )
set( CMAKE_CXX_STANDARD_REQUIRED TRUE )

set( CMAKE_CUDA_STANDARD 14 )
set( CMAKE_CUDA_STANDARD_REQUIRED TRUE )

if( NOT SupportingFiles )
    add_subdirectory(
        "${CMAKE_CURRENT_SOURCE_DIR}/../../Libraries/SupportingFiles"
        "${CMAKE_CURRENT_SOURCE_DIR}/../../Libraries/SupportingFiles/build")
endif( NOT SupportingFiles )

if( NOT Histogram )
    add_subdirectory(
        "${CMAKE_CURRENT_SOURCE_DIR}/../../Libraries/Histogram"
        "${CMAKE_CURRENT_SOURCE_DIR}/../../Libraries/Histogram/build")
endif( NOT Histogram )

add_executable( histogram_test main.cpp )

target_link_libraries( histogram_test PRIVATE lib_supporting_files )
target_link_libraries( histogram_test PRIVATE lib_supporting_files_cuda )
target_link_libraries( histogram_test PRIVATE lib_histogram )

# set custom linker (nvcc)
set( CMAKE_LINKER "/usr/bin/nvcc" )
# build the executable linker with the custom linker
set( CMAKE_CXX_LINK_EXECUTABLE "<CMAKE_LINKER>  <FLAGS> <CMAKE_CXX_LINK_FLAGS> <LINK_FLAGS> <OBJECTS>  -o <TARGET> <LINK_LIBRARIES>" )
# originally the linker is build as follows
# set( CMAKE_CXX_LINK_EXECUTABLE "<CMAKE_CXX_COMPILER>  <FLAGS> <CMAKE_CXX_LINK_FLAGS> <LINK_FLAGS> <OBJECTS>  -o <TARGET> <LINK_LIBRARIES>" )
