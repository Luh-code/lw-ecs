cmake_minimum_required(VERSION 3.1)
project(lw-ecs VERSION 1.0 LANGUAGES C CXX)
include(CTest)

# Ensures that we do an out of source build

MACRO(MACRO_ENSURE_OUT_OF_SOURCE_BUILD MSG)
     STRING(COMPARE EQUAL "${CMAKE_SOURCE_DIR}"
     "${CMAKE_BINARY_DIR}" insource)
     GET_FILENAME_COMPONENT(PARENTDIR ${CMAKE_SOURCE_DIR} PATH)
     STRING(COMPARE EQUAL "${CMAKE_SOURCE_DIR}"
     "${PARENTDIR}" insourcesubdir)
    IF(insource OR insourcesubdir)
        MESSAGE(FATAL_ERROR "${MSG}")
    ENDIF(insource OR insourcesubdir)
ENDMACRO(MACRO_ENSURE_OUT_OF_SOURCE_BUILD)

MACRO_ENSURE_OUT_OF_SOURCE_BUILD(
"${CMAKE_PROJECT_NAME} requires an out of source build.")

set(CMAKE_BUILD_TYPE Debug)

set(CMAKE_C_COMILER "gcc-12.2")
set(CMAKE_CXX_COMILER "g++-12.2")

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_EXTENSIONS OFF)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CURRENT_BINARY_DIR ../bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ../bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ../bin/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ../bin/lib)

file(GLOB umbraECSSrcFiles
  ${PROJECT_SOURCE_DIR}/src/*.h
  ${PROJECT_SOURCE_DIR}/src/**/*.h
  ${PROJECT_SOURCE_DIR}/src/*.hpp
  ${PROJECT_SOURCE_DIR}/src/**/*.hpp
  ${PROJECT_SOURCE_DIR}/src/*.c
  ${PROJECT_SOURCE_DIR}/src/**/*.c
  ${PROJECT_SOURCE_DIR}/src/*.cpp
  ${PROJECT_SOURCE_DIR}/src/**/*.cpp
)

set(ABSL_PROPAGATE_CXX_STD ON)
add_subdirectory(${PROJECT_SOURCE_DIR}/deps/abseil)

set(UMBRA_ECS_REQUIRED_INCLUDE_DIRS
  ABSL_COMMON_INCLUDE_DIRS
)
set(UMBRA_ECS_REQUIRED_LINK_LIBRARIES
  absl::flat_hash_set
  absl::container_common
)

# add_library(lw_ecs ${umbraSrcFiles})
# set_target_properties(lw_ecs PROPERTIES LINKER_LANGUAGE CXX)
# target_include_directories(lw_ecs PUBLIC
#   ABSL_COMMON_INCLUDE_DIRS
# )
# target_link_libraries(lw_ecs PUBLIC
#   absl::flat_hash_set
#   absl::container_common
# )

enable_testing()
#add_subdirectory(${PROJECT_SOURCE_DIR}/tests)
