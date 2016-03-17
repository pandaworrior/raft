cmake_minimum_required(VERSION 2.8.12)

project(Raft C)

set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "build type")
string(TOUPPER ${CMAKE_BUILD_TYPE} CMAKE_BUILD_TYPE_UPPER)

if (MSVC)
    set(CompilerFlags
        CMAKE_CXX_FLAGS
        CMAKE_CXX_FLAGS_DEBUG
        CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS
        CMAKE_C_FLAGS_DEBUG
        CMAKE_C_FLAGS_RELEASE
    )

    # Build using /MT option
    foreach(CompilerFlag ${CompilerFlags})
      string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
    endforeach()
endif ()

#user specified option
message("--------------Raft build option--------------")
message("CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE}")
message("---------------------------------------------")

include_directories(include)
set(sources
    src/raft_log.c
    src/raft_node.c
    src/raft_server.c
    src/raft_server_properties.c)

add_library(raft STATIC ${sources})

set_target_properties(raft PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY_${CMAKE_BUILD_TYPE_UPPER} "${CMAKE_CURRENT_BINARY_DIR}/lib"
)

install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib"
    DESTINATION ./
    FILES_MATCHING PATTERN "*.lib" PATTERN "*.dll" PATTERN "*.so*" PATTERN "*.a*"
)