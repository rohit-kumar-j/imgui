cmake_minimum_required(VERSION 3.5)

# THIS IS QUICL AND DIRTY ONLY FOR GLFW and
project(imgui CXX)

include(FetchContent)
FetchContent_Declare(glfw GIT_REPOSITORY https://github.com/glfw/glfw)

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
        ${glfw_INCLUDEDIR}
    PUBLIC
        ./
        ${IMGUI_HEADERS}
)

target_link_libraries(imgui
    PRIVATE
        glfw
)

install(TARGETS imgui
        DESTINATION lib
)

install(FILES ${IMGUI_HEADERS} # source directory
    DESTINATION include/imgui # target directory
)
