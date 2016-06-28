cmake_minimum_required(VERSION 2.8.3)

project(rcl_examples)

if(NOT WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra")
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rcl REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(rmw REQUIRED)
find_package(rmw_implementation_cmake REQUIRED)

function(custom_executable)

	add_executable(get_node_names${target_suffix}
 		src/get_node_names.cpp
	)
	ament_target_dependencies(get_node_names${target_suffix}
	  "rclcpp${target_suffix}"
	  "rcl${target_suffix}"
	)

	install(TARGETS get_node_names${target_suffix}
		  DESTINATION bin)
endfunction()

macro(targets)
  if(NOT "${target_suffix} " STREQUAL " ")
    get_rclcpp_information("${rmw_implementation}" "rclcpp${target_suffix}")
  endif()

  # Examples of Publish/Subscribe with Topics
  custom_executable()

endmacro()

call_for_each_rmw_implementation(targets GENERATE_DEFAULT)

ament_package()
