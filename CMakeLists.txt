cmake_minimum_required(VERSION 3.5)

# THIS IS QUICL AND DIRTY ONLY FOR GLFW and OPRNGL3

project(imgui CXX)

file(GLOB_RECURSE IMGUI_SOURCES
  ${PROJECT_SOURCE_DIR}/*.cpp
  ${PROJECT_SOURCE_DIR}/*.h
  ${PROJECT_SOURCE_DIR}/backends/imgui_impl_glfw.h
  ${PROJECT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
  ${PROJECT_SOURCE_DIR}/backends/imgui_impl_opengl3.h
  ${PROJECT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
  ${PROJECT_SOURCE_DIR}/backends/imgui_impl_opengl3_loader.cpp
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
