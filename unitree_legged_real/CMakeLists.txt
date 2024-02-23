cmake_minimum_required(VERSION 2.8.3)
project(unitree_legged_real)

set(CMAKE_CXX_FLAGS "-O3 -fPIC -std=c++11")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    geometry_msgs
    unitree_legged_msgs
    unitree_legged_sdk
)

catkin_package()

message("-- CMAKE_SYSTEM_PROCESSOR: ${CMAKE_SYSTEM_PROCESSOR}")
if("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "x86_64.*")
  set(ARCH amd64)
else()
  set(ARCH arm64)
endif()

set(EXTRA_LIBS -pthread ${unitree_legged_sdk_LIBRARIES})

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${unitree_legged_sdk_INCLUDE_DIRS}
)

add_executable(ros_example_walk src/exe/example_walk.cpp)
target_link_libraries(ros_example_walk ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(ros_example_walk ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(ros_example_position src/exe/example_position.cpp)
target_link_libraries(ros_example_position ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(ros_example_position ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(state_sub src/exe/state_sub.cpp)
target_link_libraries(state_sub ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(state_sub ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(ros_udp src/exe/ros_udp.cpp)
target_link_libraries(ros_udp ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(ros_udp ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(control_via_keyboard src/exe/control_via_keyboard.cpp)
target_link_libraries(control_via_keyboard ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(control_via_keyboard ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(twist_sub src/exe/twist_sub.cpp)
target_link_libraries(twist_sub ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(twist_sub ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})



# add Julia interface
# set(JlCxx_DIR /home/biorobotics/Documents/julia_program_files/libcxxwrap-julia/build)
# set(Julia_PREFIX /home/biorobotics/Documents/julia_program_files/julia-1.5.3) 
# set(Julia_EXECUTABLE /home/biorobotics/Documents/julia_program_files/julia-1.5.3/bin/julia)
# find_package(JlCxx)
# get_target_property(JlCxx_location JlCxx::cxxwrap_julia LOCATION)
# get_filename_component(JlCxx_location ${JlCxx_location} DIRECTORY)
# set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib;${JlCxx_location}")
# set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")

# message(STATUS "Found JlCxx at ${JlCxx_location}")

# add_library(julia_a1_interface SHARED src/julia/julia_interface.cpp)

# target_link_libraries(julia_a1_interface JlCxx::cxxwrap_julia ${EXTRA_LIBS})
# this lib file locates in 
# /home/biorobotics/ros_workspaces/unitree_ws/build/unitree_legged_real/lib
# install(TARGETS
#   julia_a1_interface
# LIBRARY DESTINATION lib
# ARCHIVE DESTINATION lib
# RUNTIME DESTINATION lib)
