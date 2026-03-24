cmake_minimum_required(VERSION 3.22)
project(mobman_moveit_config)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)

# Install launch files
if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/launch")
  install(
    DIRECTORY launch
    DESTINATION share/${PROJECT_NAME}
    PATTERN "setup_assistant.launch" EXCLUDE)
endif()

# Install config files
install(DIRECTORY config DESTINATION share/${PROJECT_NAME})
install(FILES .setup_assistant DESTINATION share/${PROJECT_NAME})

# Install Python scripts
install(
  PROGRAMS
    scripts/test_mobman_moveit.py
  DESTINATION lib/${PROJECT_NAME}
  RENAME test_mobman_moveit
)

ament_package()
