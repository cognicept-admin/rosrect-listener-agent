cmake_minimum_required(VERSION 3.0.2)
project(error_resolution_diagnoser)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)
## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslib
  rosgraph_msgs
  std_msgs
)
set(cpprestsdk_DIR /usr/lib/${CMAKE_LIBRARY_ARCHITECTURE}/cmake/)
find_package(OpenSSL REQUIRED)
find_package(cpprestsdk REQUIRED)
## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)
## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()
################################################
## Declare ROS messages, services and actions ##
################################################
## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend tag for "message_generation"
##   * add a build_depend and a exec_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependency has been pulled in
##     but can be declared for certainty nonetheless:
##     * add a exec_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)
## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )
## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )
## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )
## Generate added messages and services with any dependencies listed here
#generate_messages(
#  DEPENDENCIES
#  std_msgs  # Or other packages containing msgs
#  rosgraph_msgs
#)
################################################
## Declare ROS dynamic reconfigure parameters ##
################################################
## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a exec_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed
## Generate dynamic reconfigure parameters in the 'cfg' folder
# generate_dynamic_reconfigure_options(
#   cfg/DynReconf1.cfg
#   cfg/DynReconf2.cfg
# )
###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES error_resolution_diagnoser_lib
  CATKIN_DEPENDS roscpp std_msgs rosgraph_msgs
#  DEPENDS system_lib
)
###########
## Build ##
###########
## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)
## Declare a C++ library
# add_library(${PROJECT_NAME}
#   src/${PROJECT_NAME}/error_resolution_diagnoser.cpp
# )
## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
add_library(error_resolution_diagnoser_lib src/backend_api.cpp src/robot_event.cpp src/state_manager.cpp)
target_link_libraries(error_resolution_diagnoser_lib ${catkin_LIBRARIES} cpprestsdk::cpprest)
add_executable(error_resolution_diagnoser src/listener_agent.cpp )
target_link_libraries(error_resolution_diagnoser
  ${catkin_LIBRARIES}
  error_resolution_diagnoser_lib
  cpprestsdk::cpprest
)
add_dependencies(error_resolution_diagnoser ${error_resolution_diagnoser_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
## Rename C++ executable without prefix
## The above recommended prefix causes long target names, the following renames the
## target back to the shorter version for ease of user use
## e.g. "rosrun someones_pkg node" instead of "rosrun someones_pkg someones_pkg_node"
# set_target_properties(${PROJECT_NAME}_node PROPERTIES OUTPUT_NAME node PREFIX "")
## Add cmake target dependencies of the executable
## same as for the library above
#############
## Install ##
#############
# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html
## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )
## Mark executables for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_executables.html
# install(TARGETS ${PROJECT_NAME}_node
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )
## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
# install(TARGETS ${PROJECT_NAME}
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
# )
## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )
## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )
#############
## Testing ##
#############
## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_error_resolution_diagnoser.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()
## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
if(CATKIN_ENABLE_TESTING AND ENABLE_COVERAGE_TESTING)
  find_package(code_coverage REQUIRED)
  # Add compiler flags for coverage instrumentation before defining any targets
  APPEND_COVERAGE_COMPILER_FLAGS()
endif()
if(CATKIN_ENABLE_TESTING)
  find_package(rostest REQUIRED)
  
  # Unit tests for utility classes as just gtest
  catkin_add_gtest(backend_test_node test/utests_backendapi.cpp)
  catkin_add_gtest(robotevent_test_node test/utests_robotevent.cpp)
  catkin_add_gtest(statemanager_test_node test/utests_statemanager.cpp)

  # Integration test for listener agent node as rostest
  add_rostest_gtest(listeneragent_test_node_ros test/listener_integration_test_ros.test test/itest_listeneragent_ros.cpp)
  add_rostest_gtest(listeneragent_test_node_db test/listener_integration_test_db.test test/itest_listeneragent_db.cpp)
  add_rostest_gtest(listeneragent_test_node_telemetry test/listener_integration_test_telemetry.test test/itest_listeneragent_telemetry.cpp)
  add_rostest_gtest(listeneragent_test_node_diagnostics test/listener_integration_test_diagnostics.test test/itest_listeneragent_diagnostics.cpp)

  target_link_libraries(backend_test_node ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)
  target_link_libraries(robotevent_test_node ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)
  target_link_libraries(statemanager_test_node ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)
  target_link_libraries(listeneragent_test_node_ros ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)
  target_link_libraries(listeneragent_test_node_db ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)
  target_link_libraries(listeneragent_test_node_telemetry ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)
  target_link_libraries(listeneragent_test_node_diagnostics ${catkin_LIBRARIES} error_resolution_diagnoser_lib cpprestsdk::cpprest)

  if(ENABLE_COVERAGE_TESTING)
    set(COVERAGE_EXCLUDES "*/${PROJECT_NAME}/test*" "*/${PROJECT_NAME}/include/${PROJECT_NAME}/*.h")
    add_code_coverage(
      NAME ${PROJECT_NAME}_coverage_report
      # DEPENDENCIES tests
    )
  endif()
endif()