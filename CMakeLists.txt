cmake_minimum_required(VERSION 3.15)

project(nDmenu C CXX ASM)

find_package(SDL2 REQUIRED)
find_package(SDL2_image CONFIG REQUIRED)
find_package(SDL2_ttf CONFIG REQUIRED)
find_package(SDL2_mixer CONFIG REQUIRED)
find_package(unofficial-smpeg2 CONFIG REQUIRED)
find_package(OpenGL REQUIRED)

list(APPEND HEADERS src/nD_GL.h)
list(APPEND HEADERS src/GLee.h)
list(APPEND SOURCES src/main.c)
list(APPEND SOURCES src/GLee.c)

add_executable(nDmenu WIN32 ${SOURCES} ${HEADERS})
set_target_properties(nDmenu PROPERTIES LINKER_LANGUAGE CXX)

target_compile_definitions(nDmenu PRIVATE $<$<PLATFORM_ID:Windows>:WIN32>)

target_link_libraries(nDmenu PRIVATE
    SDL2::SDL2
    SDL2::SDL2main
    SDL2_image::SDL2_image
    SDL2_ttf::SDL2_ttf
    SDL2_mixer::SDL2_mixer
    unofficial::smpeg2::smpeg2
    OpenGL::GL)

if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.26)
    set(ND_COPY_DIR copy_directory_if_different)
else()
    set(ND_COPY_DIR copy_directory)
endif()

add_custom_command(TARGET nDmenu POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E ${ND_COPY_DIR}
            "${CMAKE_CURRENT_SOURCE_DIR}/data"
            "$<TARGET_FILE_DIR:nDmenu>/data"
    COMMENT "Copying data folder to output directory")