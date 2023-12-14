cmake_minimum_required(VERSION 3.5)
project(TestSuite)

# Set compilers
set(HOST_COMPILER clang++)
set(NVCC nvcc -ccbin ${HOST_COMPILER})
set(CMAKE_CXX_COMPILER ${HOST_COMPILER})

# Compiler flags
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -w")

# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Executable names
set(EXE "test_suite")
set(CONTAINER_EXE "container_suite")
set(OPERATIONS_EXE "operations_suite")
set(TENSOR_EXE "tensor_suite")
set(TRAITS_EXE "traits_suite")

# Source files
set(SOURCE_FILES
    container_tests.cpp
    operations_tests.cpp
    tensor_tests.cpp
    traits_tests.cpp
    tests.cpp
)

# Build executables
add_executable(${EXE} ${SOURCE_FILES})
add_executable(${CONTAINER_EXE} ${SOURCE_FILES})
add_executable(${OPERATIONS_EXE} ${SOURCE_FILES})
add_executable(${TENSOR_EXE} ${SOURCE_FILES})
add_executable(${TRAITS_EXE} ${SOURCE_FILES})

# Link libraries
set(LINK_LIBRARIES -lboost_unit_test_framework)

# Link executables with libraries
target_link_libraries(${EXE} ${LINK_LIBRARIES})
target_link_libraries(${CONTAINER_EXE} ${LINK_LIBRARIES})
target_link_libraries(${OPERATIONS_EXE} ${LINK_LIBRARIES})
target_link_libraries(${TENSOR_EXE} ${LINK_LIBRARIES})
target_link_libraries(${TRAITS_EXE} ${LINK_LIBRARIES})

# Compiler flags for specific targets
set_target_properties(${EXE} PROPERTIES COMPILE_FLAGS "${CMAKE_CXX_FLAGS} -DSTAND_ALONE")
set_target_properties(${CONTAINER_EXE} PROPERTIES COMPILE_FLAGS "${CMAKE_CXX_FLAGS} -DSTAND_ALONE")
set_target_properties(${OPERATIONS_EXE} PROPERTIES COMPILE_FLAGS "${CMAKE_CXX_FLAGS} -DSTAND_ALONE")
set_target_properties(${TENSOR_EXE} PROPERTIES COMPILE_FLAGS "${CMAKE_CXX_FLAGS} -DSTAND_ALONE")
set_target_properties(${TRAITS_EXE} PROPERTIES COMPILE_FLAGS "${CMAKE_CXX_FLAGS} -DSTAND_ALONE")

# Clean target
add_custom_target(clean
    COMMAND rm -rf *.o ${EXE} ${CONTAINER_EXE} ${OPERATIONS_EXE} ${TENSOR_EXE} ${TRAITS_EXE}
    COMMENT "Cleaning project"
)
