cmake_minimum_required(VERSION 3.22)

# Setup project
project(LwLibPROJECT)

if(NOT PROJECT_IS_TOP_LEVEL)
    add_subdirectory(lwow)
else()
    add_executable(${PROJECT_NAME})
    target_sources(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/dev/main.c

        # Port files
        ${CMAKE_CURRENT_LIST_DIR}/lwow/src/system/lwow_sys_win32.c
        ${CMAKE_CURRENT_LIST_DIR}/lwow/src/system/lwow_ll_win32.c
    )
    target_include_directories(${PROJECT_NAME} PUBLIC
        ${CMAKE_CURRENT_LIST_DIR}
        ${CMAKE_CURRENT_LIST_DIR}/dev
    )

    # Add subdir with lwow and link to the project
    set(LWOW_OPTS_FILE ${CMAKE_CURRENT_LIST_DIR}/dev/lwow_opts.h)
    add_subdirectory(lwow)
    target_link_libraries(${PROJECT_NAME} lwow)
    target_link_libraries(${PROJECT_NAME} lwow_devices)
    add_subdirectory(snippets)
    target_link_libraries(${PROJECT_NAME} lwow_snippets)

    # Add compile options to the library, which will propagate options to executable through public link
    target_compile_definitions(lwow PUBLIC WIN32 _DEBUG CONSOLE LWOW_DEV)
    target_compile_options(lwow PUBLIC -Wall -Wextra -Wpedantic)
endif()
