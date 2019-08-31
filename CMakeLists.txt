cmake_minimum_required(VERSION 3.6)

PROJECT (tinygltf)

SET(CMAKE_CXX_STANDARD 11)

option(TINYGLTF_BUILD_EXAMPLES "Compile examples" true)

if (TINYGLTF_BUILD_EXAMPLES)
  ADD_EXECUTABLE ( loader_example
    loader_example.cc
  )

  ADD_SUBDIRECTORY ( examples/gltfutil )
  ADD_SUBDIRECTORY ( examples/glview )
  ADD_SUBDIRECTORY ( examples/validator )

  INSTALL ( FILES
    json.hpp
    stb_image.h
    stb_image_write.h
    tiny_gltf.h
    DESTINATION
    include
  )

  INSTALL ( FILES
  cmake/TinyGLTFConfig.cmake
  DESTINATION
  cmake
  )

else ()

  # Target for tinygltf #
  add_library(tinygltf INTERFACE)
  add_library(tinygltf::tinygltf ALIAS tinygltf)

  # Target property for include directories
  target_include_directories(
    tinygltf
    INTERFACE
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>"
    "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
  )

  # A simple project to show tinygltf headers in the IDE
  file(GLOB tinygltf_INCLUDE_HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/*.h" "${CMAKE_CURRENT_SOURCE_DIR}/*.hpp")
  add_custom_target(tinygltf_headers_for_ide SOURCES ${tinygltf_INCLUDE_HEADERS})

endif ()
