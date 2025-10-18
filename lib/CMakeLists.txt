cmake_minimum_required(VERSION 3.10)
project(obindex2_lib LANGUAGES CXX)

option(ENABLE_TESTING "Enable testing" ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Dependencies
find_package(OpenCV REQUIRED)
find_package(Boost REQUIRED COMPONENTS system filesystem)
find_package(OpenMP)

if(NOT Boost_FOUND)
  message(FATAL_ERROR "Boost not found")
endif()

include_directories(
  include
  /usr/include/catch2
  ${OpenCV_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

# Library
add_library(obindex2 SHARED
  src/binary_descriptor.cc
  src/binary_tree_node.cc
  src/binary_tree.cc
  src/binary_index.cc
)

target_include_directories(obindex2
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(obindex2
  PUBLIC
  ${OpenCV_LIBRARIES}
  ${Boost_LIBRARIES}
)

if(OpenMP_CXX_FOUND)
  target_link_libraries(obindex2 PUBLIC OpenMP::OpenMP_CXX)
  target_compile_options(obindex2 PUBLIC ${OpenMP_CXX_FLAGS})
endif()

# Testing with Catch2
if(ENABLE_TESTING)
  enable_testing()

  add_executable(test_bdesc tests/test_bdesc.cc)
  target_link_libraries(test_bdesc PRIVATE obindex2)
  add_test(NAME test_bdesc COMMAND test_bdesc)

  add_executable(test_btree tests/test_btree.cc)
  target_link_libraries(test_btree PRIVATE obindex2)
  add_test(NAME test_btree COMMAND test_btree)

  add_executable(test_bindex tests/test_bindex.cc)
  target_link_libraries(test_bindex PRIVATE obindex2)
  add_test(NAME test_bindex COMMAND test_bindex)

  add_executable(test_search tests/test_search.cc)
  target_link_libraries(test_search PRIVATE obindex2)
  add_test(NAME test_search COMMAND test_search)
endif()
