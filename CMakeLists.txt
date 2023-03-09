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
include(FetchContent)
FetchContent_Declare(glfw GIT_REPOSITORY https://github.com/glfw/glfw) 
FetchContent_MakeAvailable(glfw)

file(GLOB IMGUI_SOURCES
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
    backends/imgui_impl_opengl3_loader.cpp
)

file(GLOB IMGUI_HEADERS
    imconfig.h
    imgui.h
    imgui_internal.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    backends/imgui_impl_glfw.h
    backends/imgui_impl_opengl3.h
)

add_library(imgui STATIC ${IMGUI_SOURCES})

target_include_directories(imgui 
    PRIVATE 
        ./
        ${glfw_INCLUDEDIR}
    PUBLIC
        ${IMGUI_HEADERS}
)

target_link_libraries(imgui
    PRIVATE
        glfw
)

install(DIRECTORY . DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET ${IMGUI_BINARY_DIR}/src/imguiConfig.cmake"
        "${IMGUI_BINARY_DIR}/src/imguiConfigVersion.cmake"
        DESTINATION "${IMGUI_CONFIG_PATH}")
