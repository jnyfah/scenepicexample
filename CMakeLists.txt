cmake_minimum_required( VERSION 3.15 )

project(scenepicexample)

include(FetchContent)


set(nodejs_URL https://nodejs.org/dist/v16.14.2/node-v16.14.2-win-x64.zip)

# Download and unpack nodeJS binary distribution.
FetchContent_Declare(nodejs
    URL ${nodejs_URL}
)

FetchContent_MakeAvailable(nodejs)

FetchContent_Declare(scenepic
    GIT_REPOSITORY https://github.com/microsoft/scenepic.git
    GIT_TAG v1.0.10
)
FetchContent_GetProperties(scenepic)
if(NOT scenepic_POPULATED)
    FetchContent_Populate(scenepic)
    message(STATUS "scenepic_SOURCE_DIR: ${scenepic_SOURCE_DIR}")
    
    # Run npm install and npm run build on the cloned project    
    execute_process(
        COMMAND ${nodejs_SOURCE_DIR}/npm.cmd install
        WORKING_DIRECTORY ${scenepic_SOURCE_DIR}
        RESULT_VARIABLE scenepic_npm_RESULT
        OUTPUT_VARIABLE scenepic_npm_STDOUT
        ERROR_VARIABLE scenepic_npm_STDERR
    )
    message(STATUS "scenepic_npm_RESULT: ${scenepic_npm_RESULT}")
    message(STATUS "scenepic_npm_STDOUT: ${scenepic_npm_STDOUT}")
    message(STATUS "scenepic_npm_STDERR: ${scenepic_npm_STDERR}")
    if(NOT scenepic_npm_RESULT EQUAL 0)
    message(FATAL_ERROR "npm install failed (see output).")
    endif()

    execute_process(
        COMMAND ${nodejs_SOURCE_DIR}/npm.cmd run build
        WORKING_DIRECTORY ${scenepic_SOURCE_DIR}
        RESULT_VARIABLE scenepic_npm_RESULT
        OUTPUT_VARIABLE scenepic_npm_STDOUT
        ERROR_VARIABLE scenepic_npm_STDERR
    )
    message(STATUS "scenepic_npm_RESULT: ${scenepic_npm_RESULT}")
    message(STATUS "scenepic_npm_STDOUT: ${scenepic_npm_STDOUT}")
    message(STATUS "scenepic_npm_STDERR: ${scenepic_npm_STDERR}")
    if(NOT scenepic_npm_RESULT EQUAL 0)
        message(FATAL_ERROR "npm build failed (see output).")
    endif()

    # Once the prerequisites are satisfied just add_subdirectory as usual
    add_subdirectory(${scenepic_SOURCE_DIR} ${scenepic_BINARY_DIR})
endif()

set(FETCHCONTENT_UPDATES_DISCONNECTED ON CACHE BOOL "")
