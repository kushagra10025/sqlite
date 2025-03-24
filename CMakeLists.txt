##########
# sqlite #
##########

cmake_minimum_required(VERSION 3.25)

project(sqlite)

set(SQLITE_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/sqlite3.c"
)

add_library(${PROJECT_NAME} STATIC ${SQLITE_SOURCES})
add_library(sqlite::sqlite_static ALIAS ${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} 
	INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}"
)

install(TARGETS ${PROJECT_NAME})
