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

# Add source files
set(SOURCES
  Nexus-HelloWorld.cpp
)

# Add header files
set(HEADERS
  Nexus-HelloWorld.h
)

# Add source to this project's executable.
add_executable (Nexus-HelloWorld ${SOURCES} ${HEADERS})

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET Nexus-HelloWorld PROPERTY CXX_STANDARD 20)
endif()

# TODO: Add tests and install targets if needed.
