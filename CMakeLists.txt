cmake_minimum_required(VERSION 3.14)
project(PKURunningHelper CXX)
SET(CMAKE_EXPORT_COMPILE_COMMANDS ON )
SET (CMAKE_CXX_STANDARD 17)

add_compile_options(
    -Wall -Wextra -Wnon-virtual-dtor -pedantic -Werror # -Wshadow -Wconversion -Wsign-conversion
)

include(FetchContent)

FetchContent_Declare(cpr GIT_REPOSITORY https://github.com/libcpr/cpr.git GIT_TAG 67e12da316754ff3c6c91b50aafb2658438f3c1e) # the commit hash for 1.7.0
FetchContent_MakeAvailable(cpr)

FetchContent_Declare(json
  GIT_REPOSITORY https://github.com/nlohmann/json.git
  GIT_TAG v3.7.3)
FetchContent_GetProperties(json)
if(NOT json_POPULATED)
  FetchContent_Populate(json)
  add_subdirectory(${json_SOURCE_DIR} ${json_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

#https://stackoverflow.com/questions/3201154/automatically-add-all-files-in-a-folder-to-a-target-using-cmake
file(GLOB SRC CONFIGURE_DEPENDS
    "src/*.cpp"
    "include/*.h"
)

add_executable(${PROJECT_NAME} ${SRC})
target_link_libraries(${PROJECT_NAME} 
    PRIVATE 
        cpr::cpr 
        nlohmann_json::nlohmann_json
)