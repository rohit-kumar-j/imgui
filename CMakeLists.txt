cmake_minimum_required(VERSION 3.5)

# THIS IS QUICL AND DIRTY ONLY FOR GLFW and OPRNGL3

project(imgui CXX)

file(GLOB_RECURSE IMGUI_SOURCES
  ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/*.h
  ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.h
  ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.h
  ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3_loader.cpp
)

include_directories(./)

add_library(imgui_target ${IMGUI_SOURCES})

target_sources(imgui_target 
  PUBLIC 
    ${IMGUI_SOURCES}
)

target_include_directories(imgui_target 
  PUBLIC 
    ./
    backends
)

target_link_libraries(imgui_target 
    PRIVATE
      glfw
)
