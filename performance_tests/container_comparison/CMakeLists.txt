cmake_minimum_required(VERSION 3.5)
project(ContainerProject)

# Set compiler
set(CMAKE_CXX_COMPILER g++)
set(CMAKE_CXX_STANDARD 11)

# Compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -w -O3")

# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Executable names
set(EXE_SCONT "scontainer")
set(EXE_DCONT "dcontainer")
set(EXE_CONT "container")

# Source files
set(SOURCE_FILES
    scontainer.cpp
    dcontainer.cpp
    container.cpp
)

# Build executables
add_executable(${EXE_SCONT} ${SOURCE_FILES})
add_executable(${EXE_DCONT} ${SOURCE_FILES})
add_executable(${EXE_CONT} ${SOURCE_FILES})

# Additional compiler flags for scont_asm and dcont_asm targets
set(CMAKE_ASM_COMPILER "as")
set(CMAKE_ASM_FLAGS "-adhln")

# Custom targets for asm files
add_custom_target(scont_asm
    COMMAND ${CMAKE_CXX_COMPILER} ${CMAKE_CXX_FLAGS} -S -g -fverbose-asm -masm=intel -O3 -o scontainer.s scontainer.cpp
    COMMAND as -adhln scontainer.s > scontainer.lst
    COMMENT "Building and running scont_asm"
    DEPENDS ${EXE_SCONT}
)

add_custom_target(dcont_asm
    COMMAND ${CMAKE_CXX_COMPILER} ${CMAKE_CXX_FLAGS} -S -g -fverbose-asm -masm=intel -O3 -o dcontainer.s dcontainer.cpp
    COMMAND as -adhln dcontainer.s > dcontainer.lst
    COMMENT "Building and running dcont_asm"
    DEPENDS ${EXE_DCONT}
)

# Clean target
add_custom_target(clean
    COMMAND rm -rf *.o *.s *.lst ${EXE_SCONT} ${EXE_DCONT} ${EXE_CONT}
    COMMENT "Cleaning project"
)
