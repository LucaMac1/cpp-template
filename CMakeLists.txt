cmake_minimum_required(VERSION 3.13)
project(MyProject)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Enable testing
enable_testing()

# Define options for sanitizers and coverage
option(ENABLE_ASAN "Enable AddressSanitizer" OFF)
option(ENABLE_UBSAN "Enable Undefined Behavior Sanitizer" OFF)
option(ENABLE_MSAN "Enable MemorySanitizer" OFF)
option(ENABLE_COVERAGE "Enable Code Coverage" OFF)

# Common compilation flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -g -fno-omit-frame-pointer")

# Sanitizer configurations
if (ENABLE_ASAN)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address -DADDRESS_SANITIZER")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address")
endif()

if (ENABLE_UBSAN)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=undefined")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=undefined")
endif()

if (ENABLE_MSAN)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=memory")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=memory")
endif()

if (ENABLE_COVERAGE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fprofile-arcs -ftest-coverage")
endif()

# Define source files and executable
file(GLOB_RECURSE SRC_FILES src/*.cpp)
add_executable(MyExecutable ${SRC_FILES})

# Link required libraries (modify as needed)
target_link_libraries(MyExecutable PRIVATE pthread)

# Add unit tests (modify if using another framework like Catch2)
add_executable(MyTests tests/test_main.cpp)
target_link_libraries(MyTests PRIVATE pthread)

# Register tests with CTest
add_test(NAME MyTests COMMAND MyTests)