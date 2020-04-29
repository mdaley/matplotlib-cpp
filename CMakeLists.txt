cmake_minimum_required(VERSION 3.16)
project(matplotlib_cpp)

set(CMAKE_CXX_STANDARD 11)

find_package(Python COMPONENTS Development NumPy)

message("Python_VERSION:${Python_VERSION}")
message("Python_NumPy_VERSION:${Python_NumPy_VERSION}")
message("Python_Development_FOUND:${Python_Development_FOUND}")
message("Python_LIBRARIES:${Python_LIBRARIES}")
message("Python_INCLUDE_DIRS:${Python_INCLUDE_DIRS}")
message("Python_NumPy_INCLUDE_DIRS:${Python_NumPy_INCLUDE_DIRS}")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/examples/build)

add_library(matplotlibcpp INTERFACE)
target_include_directories(matplotlibcpp INTERFACE include ${Python_INCLUDE_DIRS} ${Python_NumPy_INCLUDE_DIRS})
target_link_libraries(matplotlibcpp INTERFACE ${Python_LIBRARIES})

set(targets minimal basic animation bar colorbar fill fill_inbetween imshow
        lines3d modern nonblock quiver subplot subplot2grid surface update xkcd)

set(without_numpy basic)

foreach(target ${targets})
    add_executable(${target} examples/${target}.cpp)
    target_link_libraries(${target} matplotlibcpp)
    if(${target} IN_LIST without_numpy)
        message("NO NUMPY for ${target}")
        target_compile_definitions(${target} PRIVATE "-DWITHOUT_NUMPY")
    endif()
endforeach()
