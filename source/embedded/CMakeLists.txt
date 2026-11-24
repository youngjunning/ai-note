##
# @file CMakeLists.txt
# @brief Embedded application top-level CMake — mirrors the layout
#        produced by `tos.py new project`. Lives under
#        `<project>/source/embedded/`; build with
#        `cd source/embedded && tos.py build`.
#/

# APP_PATH
set(APP_PATH ${CMAKE_CURRENT_LIST_DIR})

# APP_NAME
get_filename_component(APP_NAME ${APP_PATH} NAME)

# APP_SRC
# Recurse so later subdirectories (for example src/ui/) are compiled without
# requiring a CMake edit. CONFIGURE_DEPENDS re-runs Glob on rebuild.
file(GLOB_RECURSE APP_SRC CONFIGURE_DEPENDS ${APP_PATH}/src/*.c ${APP_PATH}/src/*.cc ${APP_PATH}/src/*.cpp)

# APP_INC
set(APP_INC ${APP_PATH}/include)

# APP_OPTIONS
set(APP_OPTIONS "")  # -W

########################################
# Target Configure
########################################
add_library(${EXAMPLE_LIB})
message(STATUS "EXAMPLE_LIB:${APP_PATH}")

target_sources(${EXAMPLE_LIB}
    PRIVATE
        ${APP_SRC}
    )

target_include_directories(${EXAMPLE_LIB}
    PRIVATE
        ${APP_INC}
    )

target_compile_options(${EXAMPLE_LIB}
    PRIVATE
        ${APP_OPTIONS}
    )
