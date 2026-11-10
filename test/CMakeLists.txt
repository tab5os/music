# Host tests for the parts of Music that are plain C: tags, playlists, the library.
#   cmake -S test -B test/build -G Ninja && cmake --build test/build && ctest --test-dir test/build
cmake_minimum_required(VERSION 3.22)
project(music_host_tests C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
add_compile_options(-Wall -Wextra -Werror -Wno-unused-parameter -D_DEFAULT_SOURCE -D_DARWIN_C_SOURCE)

include(FetchContent)
FetchContent_Declare(unity
    GIT_REPOSITORY https://github.com/ThrowTheSwitch/Unity.git
    GIT_TAG v2.6.1
    GIT_SHALLOW TRUE)
FetchContent_MakeAvailable(unity)

set(MUSIC ${CMAKE_CURRENT_LIST_DIR}/..)
enable_testing()
add_executable(test_music test_music.c ${MUSIC}/tags.c ${MUSIC}/m3u.c ${MUSIC}/library.c)
target_include_directories(test_music PRIVATE ${MUSIC})
target_link_libraries(test_music PRIVATE unity)
add_test(NAME test_music COMMAND test_music)
