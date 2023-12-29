cmake_minimum_required(VERSION 3.9)
project(WaterSurfaceWavelets)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/modules)
set(CMAKE_TOOLCHAIN_FILE ${PROJECT_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake)
message(STATUS "CMAKE_TOOLCHAIN_FILE: ${CMAKE_TOOLCHAIN_FILE}")
include(${CMAKE_TOOLCHAIN_FILE})

# cmath on MSVC needs this to define M_PI etc.
add_compile_definitions(_USE_MATH_DEFINES)

find_package(Eigen3 CONFIG REQUIRED)

# skip OpenMP on MSVC as it doesn't support OpenMP 3.0 unsigned loops
if (!MSVC)
  find_package(OpenMP)
  if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
  endif()
endif()

link_libraries(Eigen3::Eigen)
add_subdirectory(src)
