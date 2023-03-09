cmake_minimum_required(VERSION 3.5)

# THIS IS QUICL AND DIRTY ONLY FOR GLFW and OPRNGL3

project(imgui CXX)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")
include(BuildExternalProject)

#BuildExternalProject(
#    NAME glfw
#    LIB_NAME glfw_target
#    GIT_REPO https://github.com/glfw/glfw.git
#    GIT_TAG 3.3.8
#    INSTALL_PATH ${PROJECT_SOURCE_DIR}/external
#    BUILD_TYPE Release
#    ARCHITECTURE x64
#)

FetchContent_Declare(glfw GIT_REPOSITORY https://github.com/glfw/glfw) 
FetchContent_MakeAvailable(glfw)

file(GLOB IMGUI_SOURCES
    imconfig.h
    imgui.cpp
    imgui.h
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    backends/imgui_impl_glfw.h
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.h
    backends/imgui_impl_opengl3.cpp
    backends/imgui_impl_opengl3_loader.cpp
)

add_library(imgui STATIC ${IMGUI_SOURCES})

target_include_directories(imgui 
  PUBLIC 
    ./
    ${glfw_INCLUDE_DIRS}
)

target_link_libraries(imgui
    PRIVATE
    glfw
)
