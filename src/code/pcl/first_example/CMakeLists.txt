cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(FirstExample)

find_package(PCL 1.2 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable (first_example first_example.cpp)
target_link_libraries (first_example ${PCL_LIBRARIES})
