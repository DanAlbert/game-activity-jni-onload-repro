cmake_minimum_required(VERSION 3.22.1)
project("GameActivity JNI_OnLoad repro case" VERSION 1.0.0 LANGUAGES C CXX)

find_package(game-activity REQUIRED CONFIG)

add_library(app SHARED app.cpp)
target_link_libraries(app
  PUBLIC
    game-activity::game-activity
    -landroid
    -llog
)
target_link_options(app
  PRIVATE
    -flto
    -Wl,--version-script,${CMAKE_SOURCE_DIR}/libapp.map.txt
)
set_target_properties(app
  PROPERTIES
    LINK_DEPENDS ${CMAKE_SOURCE_DIR}/libapp.map.txt
)
