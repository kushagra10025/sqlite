##########
# sqlite #
##########

cmake_minimum_required(VERSION 3.25)

project(sqlite3 LANGUAGES C)

# Custom Options - Tools
option(SQLITE3_BUILD_SHELL "Enable sqlite3 shell build!" OFF)

set(SQLITE_STATIC_LIB_NAME "${PROJECT_NAME}_static")

if(${SQLITE3_BUILD_SHELL})
    set(SQLITE_SHELL_NAME "${PROJECT_NAME}")
endif()

set(SQLITE_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/sqlite3.c"
)

if(${SQLITE3_BUILD_SHELL})
    set(SQLITE_SHELL_SOURCES
	    "${SQLITE_SOURCES}"
	    "${CMAKE_CURRENT_SOURCE_DIR}/shell.c"
    )
endif()

# Create sqlite3 Static Library
add_library(${SQLITE_STATIC_LIB_NAME} STATIC ${SQLITE_SOURCES})
add_library(sqlite3::sqlite3_static ALIAS ${SQLITE_STATIC_LIB_NAME})

# Create sqlite3 Executable
if(${SQLITE3_BUILD_SHELL})
    add_executable(${SQLITE_SHELL_NAME} ${SQLITE_SHELL_SOURCES})
endif()

# Specify include directories for the static library
target_include_directories(${SQLITE_STATIC_LIB_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Link sqlite3_static to the shell executable
if(${SQLITE3_BUILD_SHELL})
    target_link_libraries(${SQLITE_SHELL_NAME} ${SQLITE_STATIC_LIB_NAME})
endif()

# Installation
install(TARGETS ${SQLITE_STATIC_LIB_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

if(${SQLITE3_BUILD_SHELL})
    install(TARGETS ${SQLITE_SHELL_NAME}
        RUNTIME DESTINATION bin
    )
endif()
