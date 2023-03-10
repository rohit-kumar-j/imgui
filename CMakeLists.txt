cmake_minimum_required(VERSION 3.5)

# THIS IS QUICL AND DIRTY ONLY FOR GLFW and
project(imgui CXX)

set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_SOURCE_DIR} CACHE STRING " " FORCE)

# set(CMAKE_INSTALL_PREFIX FORCE PATH " " ${CMAKE_CURRENT_SOURCE_DIR})
include(FetchContent)
FetchContent_Declare(glfw GIT_REPOSITORY https://github.com/glfw/glfw)
FetchContent_GetProperties(glfw)

if(NOT glfw_POPULATED)
    FetchContent_Populate(glfw)
    add_subdirectory(${glfw_SOURCE_DIR} ${glfw_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

include(ExternalProject)

# get_filename_component(buildDirRelFilePath "${CURRENT_FOLDER}"
# REALPATH BASE_DIR "${CMAKE_BINARY_DIR}")
set(CURRENT_DIR ${CMAKE_CURRENT_SOURCE_DIR})
ExternalProject_Add(
    implot_utility
    PREFIX implot_utility
    GIT_REPOSITORY https://github.com/rohit-kumar-j/implot.git
    GIT_TAG origin/master
    INSTALL_COMMAND ""
    UPDATE_COMMAND ""
    BUILD_COMMAND ""
    CONFIGURE_COMMAND cd <SOURCE_DIR>
        COMMAND ${CMAKE_COMMAND} -E copy <SOURCE_DIR>/implot.cpp ${CURRENT_DIR}
        COMMAND ${CMAKE_COMMAND} -E copy <SOURCE_DIR>/implot.h ${CURRENT_DIR}
        COMMAND ${CMAKE_COMMAND} -E copy <SOURCE_DIR>/implot_demo.cpp ${CURRENT_DIR}
        COMMAND ${CMAKE_COMMAND} -E copy <SOURCE_DIR>/implot_internal.h ${CURRENT_DIR}
        COMMAND ${CMAKE_COMMAND} -E copy <SOURCE_DIR>/implot_items.cpp ${CURRENT_DIR}
    CONFIGURE_HANDLED_BY_BUILD FALSE
    BUILD_ALWAYS FALSE
    UPDATE_DISCONNECTED TRUE
)
ExternalProject_Get_Property(implot_utility SOURCE_DIR)
message("Source dir of myExtProj = ${SOURCE_DIR}")

set(IMPLOT_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/implot.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/implot_demo.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/implot_items.cpp"
)
set(IMPLOT_HEADERS
    "${CMAKE_CURRENT_SOURCE_DIR}/implot.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/implot_internal.h"
)

execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3_loader.cpp ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.h ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.h ${CMAKE_CURRENT_SOURCE_DIR})
execute_process(COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3_loader.h ${CMAKE_CURRENT_SOURCE_DIR})

file(GLOB IMGUI_SOURCES
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    imgui_impl_glfw.cpp
    imgui_impl_opengl3.cpp
    imgui_impl_opengl3_loader.cpp
)

file(GLOB IMGUI_HEADERS
    imconfig.h
    imgui.h
    imgui_internal.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    imgui_impl_glfw.h
    imgui_impl_opengl3.h
)
add_library(imgui STATIC ${IMGUI_SOURCES})
add_library(imgui_intermediate INTERFACE IMPORTED GLOBAL ${IMPLOT_SOURCES} implot_utility)

# target_include_directories(${_ARGS_LIB_NAME} INTERFACE .)
# target_link_libraries(${_ARGS_LIB_NAME} INTERFACE ./lib)
add_dependencies(imgui imgui_intermediate)
add_dependencies(imgui_intermediate implot_utility)

target_include_directories(imgui
    PRIVATE
        ./
        ${glfw_INCLUDEDIR}
    PUBLIC
        ${IMGUI_HEADERS}
        $<INSTALL_INTERFACE:${IMGUI_HEADERS}/>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/>
)
target_include_directories(imgui_intermediate
    INTERFACE
        ./
        ${IMPLOT_HEADERS}
        $<INSTALL_INTERFACE:${IMPLOT_HEADERS}/>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/>
)

target_link_libraries(imgui
    PRIVATE
        glfw
)

# target_link_libraries(imgui_intermediate
# INTERFACE
# ${CMAKE_INSTALL_LIBDIR}/lib/*.lib
# )
install(
    TARGETS imgui
    EXPORT imgui
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT runtime
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT runtime
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT dev
    PUBLIC_HEADER DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}" COMPONENT dev
)

install(TARGET imgui DESTINATION lib)
install(FILES ${IMPLOT_HEADERS} DESTINATION include)
install(FILES ${IMGUI_HEADERS} DESTINATION include)

# install(TARGETS imgui
# DESTINATION ${CMAKE_INSTALL_LIBDIR}
# )

# # install(FILES ${IMGUI_HEADERS} # source directory
# # DESTINATION external/include/imgui_intermediate# target directory
# # )
