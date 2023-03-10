cmake_minimum_required(VERSION 3.5)

# quick and dirty cmake
project(imgui CXX)
include(FetchContent)
FetchContent_Declare(glfw GIT_REPOSITORY https://github.com/glfw/glfw)
FetchContent_GetProperties(glfw)

if(NOT glfw_POPULATED)
    FetchContent_Populate(glfw)
    add_subdirectory(${glfw_SOURCE_DIR} ${glfw_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

if(NOT IS_DIRECTORY implot)
    execute_process(COMMAND git clone https://github.com/rohit-kumar-j/implot.git)
endif()

execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/implot/implot.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/implot/implot_demo.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/implot/implot_items.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/implot/implot.h ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/implot/implot_internal.h ${CMAKE_CURRENT_SOURCE_DIR})

execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.h ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.h ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3_loader.h ${CMAKE_CURRENT_SOURCE_DIR})

file(GLOB IMGUI_SOURCES
    implot.cpp
    implot_demo.cpp
    implot_items.cpp

    #
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    imgui_impl_glfw.cpp
    imgui_impl_opengl3.cpp
)

file(GLOB IMGUI_HEADERS
    implot.h
    implot_internal.h

    #
    imconfig.h
    imgui.h
    imgui_internal.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    imgui_impl_glfw.h
    imgui_impl_opengl3.h
    imgui_impl_opengl3_loader.h
)

add_library(imgui STATIC ${IMGUI_SOURCES})
target_include_directories(imgui
    PRIVATE
        ${glfw_INCLUDEDIR}
    PUBLIC
        ${IMGUI_HEADERS}
)
target_link_libraries(imgui
    PRIVATE
        glfw
)

install(TARGETS imgui EXPORT imgui DESTINATION lib)
install(FILES ${IMPLOT_HEADERS} DESTINATION include/imgui)
install(FILES ${IMGUI_HEADERS} DESTINATION include/imgui)