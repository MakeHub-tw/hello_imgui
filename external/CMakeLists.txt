if (HELLOIMGUI_USE_QT)
    if(NOT IS_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/qtimgui)
        message(FATAL_ERROR "Please download qtimgui by running ${HELLOIMGUI_BASEPATH}/tools/qtimgui_download.py")
    endif()

    # qtimgui includes imgui as a submodule by default
    # we prefer to use our own version (in external/imgui)
    set(QTIMGUI_BUILD_IMGUI OFF CACHE BOOL "disable qtimgui included imgui" FORCE)
    add_subdirectory(qtimgui)
endif()

# glad OpenGL loader
if (HELLOIMGUI_USE_GLAD)
    include(${CMAKE_CURRENT_LIST_DIR}/OpenGL_Loaders/glad.cmake)
endif()

# imgui is built by default
# You can use your own (but make sure that a target named imgui is available
if (HELLOIMGUI_BUILD_IMGUI)
    set(imgui_dir ${CMAKE_CURRENT_LIST_DIR}/imgui)
    add_library(imgui
            ${imgui_dir}/imgui_demo.cpp
            ${imgui_dir}/imgui_draw.cpp
            ${imgui_dir}/imgui_internal.h
            ${imgui_dir}/imgui_widgets.cpp
            ${imgui_dir}/imgui.cpp
            ${imgui_dir}/imgui.h
            )
    target_include_directories(imgui PUBLIC ${imgui_dir})
endif()

if (HELLOIMGUI_USE_SDL_OPENGL3)
    # On desktop (linux, windows, osx) sdl is provided via vcpkg
    # On iOS, the build is done externally via tools/ios/sdl_compile_ios.sh
    if (ANDROID)
        add_subdirectory(SDL)
    endif()
endif()
