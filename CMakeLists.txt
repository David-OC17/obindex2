cmake_minimum_required(VERSION 3.5)
project(obindex2 LANGUAGES CXX)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)

add_subdirectory(lib)

install(TARGETS obindex2
  EXPORT export_obindex2
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY lib/include/obindex2/
  DESTINATION include/obindex2
)

ament_export_targets(export_obindex2 HAS_LIBRARY_TARGET)
ament_export_include_directories(include)
ament_export_libraries(obindex2)
ament_export_dependencies(rclcpp)

ament_package()
