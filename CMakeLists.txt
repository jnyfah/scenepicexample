cmake_minimum_required(VERSION 3.15)

project(scenepicexample)

set(nodejs_URL https://nodejs.org/dist/v16.14.2/node-v16.14.2-win-x64.zip)

include(FetchContent)

# Download and unpack nodeJS binary distribution.
FetchContent_Declare(nodejs
    URL ${nodejs_URL}
)

FetchContent_Declare(scenepic
    GIT_REPOSITORY "https://github.com/microsoft/scenepic"
   GIT_TAG "ced3439e9bc22ea1b17df78090535db76f9c362d"
)


FetchContent_MakeAvailable(nodejs)

# Install modules.
execute_process(COMMAND ${nodejs_SOURCE_DIR}/npm.cmd install WORKING_DIRECTORY ${FETCHCONTENT_BASE_DIR}/scenepic-src)

# Add nodeJS directory to path.
set(ENV{PATH} "${nodejs_SOURCE_DIR};$ENV{PATH}")

# Build core library.
execute_process(COMMAND ${nodejs_SOURCE_DIR}/npm.cmd run build WORKING_DIRECTORY ${FETCHCONTENT_BASE_DIR}/scenepic-src)

FetchContent_MakeAvailable(scenepic)

#projd_SOURCE_DIR

add_executable(${PROJECT_NAME} src/main.cpp)
target_link_libraries(${PROJECT_NAME} nodejs scenepic)