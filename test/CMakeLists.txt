cmake_minimum_required(VERSION 3.2)

project(tests VERSION 0.0.1)

# Build googletest
add_subdirectory(
    "${CMAKE_CURRENT_SOURCE_DIR}/../googletest" 
    "googletest"
)

# Test target
add_executable(test_sort test.cpp)

# Add src to the include directories for the test target
target_include_directories(
    test_sort PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/../src"
    "${CMAKE_CURRENT_SOURCE_DIR}/../lib"
)

# Link test target against gtest libraries
target_link_libraries(
    test_sort PRIVATE
    lib
    gtest gtest_main
)

if (MSVC)
    # @see https://github.com/google/googletest/blob/master/googletest/README.md
    # Visual Studio Dynamic vs Static Runtimes
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

    target_compile_options(test_sort PRIVATE /W4)
else ()
    target_compile_options(
        test_sort PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()
