cmake_minimum_required (VERSION 3.1)

project(imgui)

set(IMGUI_PATH ${CMAKE_CURRENT_SOURCE_DIR})

find_package(OpenGL REQUIRED)

add_library(imgui.core
   ${IMGUI_PATH}/imgui.cpp
   ${IMGUI_PATH}/imgui_draw.cpp
   ${IMGUI_PATH}/imgui_widgets.cpp
   ${IMGUI_PATH}/imgui_tables.cpp
)
target_include_directories(imgui.core PUBLIC
   PUBLIC
    $<BUILD_INTERFACE:${IMGUI_PATH}>
    $<INSTALL_INTERFACE:imgui.h>
    $<INSTALL_INTERFACE:imgui_internal.h>
)

add_library(imgui.glfw 
    ${IMGUI_PATH}/backends/imgui_impl_glfw.cpp
    ${IMGUI_PATH}/backends/imgui_impl_glfw.h
)
target_include_directories(imgui.glfw PUBLIC
   PUBLIC
    $<BUILD_INTERFACE:${IMGUI_PATH}/backends>
    $<INSTALL_INTERFACE:imgui_impl_glfw.h>
)
target_link_libraries(imgui.glfw PRIVATE imgui.core)
target_link_libraries(imgui.glfw PUBLIC glfw)

add_library(imgui.opengl3
    backends/imgui_impl_opengl3.cpp
    backends/imgui_impl_opengl3.h
)
target_include_directories(imgui.glfw PUBLIC
   PUBLIC
    $<BUILD_INTERFACE:${IMGUI_PATH}/backends>
    $<INSTALL_INTERFACE:imgui_impl_opengl3.h>
)
target_link_libraries(imgui.opengl3 PUBLIC imgui.core)
target_link_libraries(imgui.opengl3 PUBLIC OpenGL::GL)
