# Minimal CMake project for building a static library under Windows.

cmake_minimum_required (VERSION 2.8)
if(NOT(${CMAKE_MAJOR_VERSION} LESS 3))
    cmake_policy(SET CMP0042 NEW)
endif()
project (yaml C)

set (YAML_VERSION_MAJOR 0)
set (YAML_VERSION_MINOR 1)
set (YAML_VERSION_PATCH 6)
set (YAML_VERSION_STRING "${YAML_VERSION_MAJOR}.${YAML_VERSION_MINOR}.${YAML_VERSION_PATCH}")

file (GLOB SRC src/*.c)

include_directories (include win32)
add_definitions (-DHAVE_CONFIG_H -DYAML_DECLARE_STATIC
-DYAML_VERSION_MAJOR=${YAML_VERSION_MAJOR}
-DYAML_VERSION_MINOR=${YAML_VERSION_MINOR}
-DYAML_VERSION_PATCH=${YAML_VERSION_PATCH}
-DYAML_VERSION_STRING="${YAML_VERSION_STRING}")
add_library (yaml SHARED ${SRC})
add_library (yamlstatic STATIC ${SRC})

if(UNIX)
    # .a vs .so can be used to differentiate the libraries:
    set_target_properties(yaml yamlstatic PROPERTIES OUTPUT_NAME yaml)
endif()
