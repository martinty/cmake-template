cmake_minimum_required(VERSION 3.10)
project(${CMAKE_BUILD_TYPE}-program)

# -----------------------------------------------------------------------------------------
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    set(CMAKE_CXX_FLAGS "/std:c++17 -TP /W4 /GR \
                        /DEBUG /INCREMENTAL /nologo /EHsc")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /Od /Ob0 -DWIN32 /MDd /Zi /RTC1")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /O2 /Ob2 -DNDEBUG -DWIN32_LEAN_AND_MEAN /MD")

    set(tdt4102Path "C:/Program Files/tdt4102")
    include_directories("${tdt4102Path}/Graph_lib/include/" "${tdt4102Path}/FLTK-1.4/")

    file(GLOB SRC *.cpp)
    file(GLOB FLTK_LIB "${tdt4102Path}/FLTK-1.4/lib/${CMAKE_BUILD_TYPE}/*.lib")
    file(GLOB GRAPH_LIB "${tdt4102Path}/Graph_lib/lib/${CMAKE_BUILD_TYPE}/*.lib")
    set(WIN_LIB "wsock32.lib comctl32.lib kernel32.lib user32.lib gdi32.lib \
                    winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib \
                    oleaut32.lib uuid.lib odbc32.lib odbccp32.lib")
    
    set(EXECUTABLE_OUTPUT_PATH ./${CMAKE_BUILD_TYPE})
    add_executable(${PROJECT_NAME} ${SRC})
    target_link_libraries(${PROJECT_NAME} ${GRAPH_LIB} ${FLTK_LIB} ${WIN_LIB})
# -----------------------------------------------------------------------------------------
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
    set(FLTK_DIR /usr/local/lib)
    find_package(FLTK REQUIRED NO_MODULE)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -Wall -Wno-overloaded-virtual")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -ggdb -Og -DDEBUG")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3 -DNDEBUG")

    set(GRAPH_DIR "/usr/local/include/Graph_lib")
    include_directories(${GRAPH_DIR} ${FLTK_INCLUDE_DIRS})

    file(GLOB SRC *.cpp)
    set(LDFLAGS "-lfltk_images -ljpeg")
    set(GRAPH_LIB "/usr/local/lib/Graph_lib")
    set(GRAPH_LIB_LINK -L${GRAPH_LIB} -lGraph_libd)

    set(EXECUTABLE_OUTPUT_PATH ./${CMAKE_BUILD_TYPE})
    add_executable(${PROJECT_NAME} ${SRC})
    target_link_libraries(${PROJECT_NAME} ${GRAPH_LIB_LINK} ${LDFLAGS} fltk)
# -----------------------------------------------------------------------------------------
elseif(CMAKE_SYSTEM_NAME MATCHES "Darwin")
    message(STATUS "CMake not implemented on Mac!")
endif()
# -----------------------------------------------------------------------------------------

#message(STATUS "CXX Compiler executable: ${CMAKE_CXX_COMPILER}")
#message(STATUS "WIN_LIB: ${WIN_LIB}")
#message(STATUS "FLTK_LIB: ${FLTK_LIB}")
#message(STATUS "GRAPH_LIB: ${GRAPH_LIB}")
#message(STATUS "SRC: ${SRC}")
#message(STATUS "Debug flags: ${CMAKE_CXX_FLAGS_DEBUG}")
#message(STATUS "Release flags: ${CMAKE_CXX_FLAGS_RELEASE}")
#message(STATUS "CXX flags: ${CMAKE_CXX_FLAGS}")