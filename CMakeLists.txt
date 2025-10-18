cmake_minimum_required(VERSION 3.5)
project(obindex2 LANGUAGES CXX)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)

add_subdirectory(lib)

ament_export_include_directories(lib/include)
ament_export_libraries(obindex2)
ament_export_dependencies(rclcpp)

ament_package()
