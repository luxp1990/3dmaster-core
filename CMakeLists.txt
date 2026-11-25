cmake_minimum_required(VERSION 3.20)
project(3dmaster_core LANGUAGES CXX)

option(3DMASTER_CORE_DISABLE_OCCT "Build 3dmaster_core without OpenCASCADE (mesh-only)" OFF)

set(CMAKE_AUTOMOC ON)

# Allow standalone configure without overriding parent shell Qt6/OCCT discovery
if(NOT TARGET Qt6::Core)
    find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets OpenGL OpenGLWidgets)
endif()

add_library(3dmaster_core STATIC
    src/master_widget.cpp
    src/master_sidebar.cpp
    src/model_loader_worker.cpp
    include/i18n.h
    include/model_data.h
    include/master_widget.h
    include/master_sidebar.h
    include/model_loader_worker.h
)

target_compile_features(3dmaster_core PUBLIC cxx_std_17)

target_include_directories(3dmaster_core PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

if(3DMASTER_CORE_DISABLE_OCCT)
    target_compile_definitions(3dmaster_core PUBLIC DISABLE_OCCT_FOR_TESTING)
else()
    if(DEFINED OCCT_INCLUDE_DIR AND EXISTS "${OCCT_INCLUDE_DIR}")
        target_include_directories(3dmaster_core PRIVATE "${OCCT_INCLUDE_DIR}")
    elseif(DEFINED OCCT_DIR AND EXISTS "${OCCT_DIR}/inc")
        target_include_directories(3dmaster_core PRIVATE "${OCCT_DIR}/inc")
    endif()
endif()

target_link_libraries(3dmaster_core PUBLIC
    Qt6::Core
    Qt6::Gui
    Qt6::Widgets
    Qt6::OpenGL
    Qt6::OpenGLWidgets
    $<$<TARGET_EXISTS:Qt6::CorePrivate>:Qt6::CorePrivate>
    $<$<TARGET_EXISTS:Qt6::GuiPrivate>:Qt6::GuiPrivate>
)

if(MSVC)
    target_compile_options(3dmaster_core PRIVATE /utf-8)
endif()

if(WIN32)
    target_compile_definitions(3dmaster_core PRIVATE
        NOMINMAX
        WIN32_LEAN_AND_MEAN
    )
endif()
