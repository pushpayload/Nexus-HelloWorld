# CMakeList.txt : CMake project for Nexus-HelloWorld, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

# Set the linux compiler to gcc posix
if (CMAKE_SYSTEM_NAME STREQUAL "Linux")
  set(CMAKE_SYSTEM_NAME Windows)
  set(CMAKE_C_COMPILER x86_64-w64-mingw32-gcc-posix)
  set(CMAKE_CXX_COMPILER x86_64-w64-mingw32-g++-posix)
  set(CMAKE_RC_COMPILER x86_64-w64-mingw32-windres)
endif()

project ("Nexus-HelloWorld")

# Specify C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Output directory settings
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Set DLL output name
set(OUTPUT_NAME "Nexus-HelloWorld")

# Add ImGui source files
set(IMGUI_SOURCES
    src/imgui/imgui.cpp
    src/imgui/imgui_demo.cpp
    src/imgui/imgui_draw.cpp
    src/imgui/imgui_tables.cpp
    src/imgui/imgui_widgets.cpp
)

# Add source files
set(SOURCES
    src/entry.cpp
    src/shared.cpp
    src/settings.cpp
    ${IMGUI_SOURCES}  # Add ImGui sources to the build
)

# Add header files
set(HEADERS
  src/shared.h
  src/settings.h
  src/imgui/imgui.h
  src/mumble/Mumble.h
  src/nexus/Nexus.h
  src/nlohmann/json.hpp
  src/version.h
)

# Create version.h before build (windows powershell, linux bash)
if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
  add_custom_command(
    OUTPUT ${CMAKE_BINARY_DIR}/src/version.h
    COMMAND powershell.exe -ExecutionPolicy Bypass -File ${CMAKE_SOURCE_DIR}/scripts/generate_version.ps1
    DEPENDS ${CMAKE_SOURCE_DIR}/scripts/generate_version.ps1
  )
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
  add_custom_command(
    OUTPUT ${CMAKE_BINARY_DIR}/src/version.h
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_SOURCE_DIR}/scripts/generate_version.sh
    DEPENDS ${CMAKE_SOURCE_DIR}/scripts/generate_version.sh
  )
endif()

# Add library target
add_library(${PROJECT_NAME} SHARED 
    ${SOURCES} 
    ${HEADERS}
    ${RESOURCES}
    ${CMAKE_BINARY_DIR}/src/version.h
)

# Set output name
set_target_properties(${PROJECT_NAME} PROPERTIES 
    PREFIX ""
    SUFFIX ".dll"
    OUTPUT_NAME ${OUTPUT_NAME}
)

# Add include directories
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_SOURCE_DIR}/src/imgui
    ${CMAKE_SOURCE_DIR}/src/mumble
    ${CMAKE_SOURCE_DIR}/src/nexus
    ${CMAKE_SOURCE_DIR}/src/nlohmann
)