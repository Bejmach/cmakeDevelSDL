cmake_minimum_required(VERSION 3.10)
project(SDLTest)

set(SOURCE src/main.cpp)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Adjust the path to your SDL2 development release
set(SDL2_DIR "external/SDL2/x86_64-mingw")

# Include the SDL2 headers
include_directories(${SDL2_DIR}/include)

# Link against the SDL2 library
add_executable(SDLTest ${SOURCE})

# Find and link SDL2
find_library(SDL2_LIBRARY NAMES SDL2 PATHS ${CMAKE_SOURCE_DIR}/${SDL2_DIR}/lib)
find_library(SDL2MAIN_LIBRARY NAMES SDL2main PATHS ${CMAKE_SOURCE_DIR}/${SDL2_DIR}/lib)
find_library(SDL2_NET_LIBRARY NAMES SDL2_net PATHS ${CMAKE_SOURCE_DIR}/${SDL2_DIR}/lib)

if(NOT SDL2_LIBRARY OR NOT SDL2MAIN_LIBRARY OR NOT SDL2_NET_LIBRARY)
    message(FATAL_ERROR "SDL2 libraries not found!")
endif()

target_link_libraries(SDLTest ${SDL2_NET_LIBRARY} ${SDL2MAIN_LIBRARY} ${SDL2_LIBRARY})

# Copy SDL2.dll to the build directory
add_custom_command(TARGET SDLTest POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/${SDL2_DIR}/bin/SDL2.dll ${CMAKE_BINARY_DIR}
	COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/${SDL2_DIR}/bin/SDL2_net.dll ${CMAKE_BINARY_DIR}
)
