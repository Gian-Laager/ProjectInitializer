cmake_minimum_required(VERSION 3.17)
project(ProjectInitializer)

set(CMAKE_CXX_STANDARD 20)

set(SRC
		src/main.cpp
)
include_directories(src ${CMAKE_CURRENT_LIST_DIR})


set(PCH src/pch.h)

set(wxWidgets_CONFIGURATION mswu)
find_package(wxWidgets REQUIRED)
include(${wxWidgets_USE_FILE})

add_executable(${PROJECT_NAME} ${SRC})
target_precompile_headers(${PROJECT_NAME} PUBLIC ${PCH})
target_link_libraries(${PROJECT_NAME} ${wxWidgets_LIBRARIES})

set(TEST_SRC
		test/main.cpp
)

set(TEST_PCH test/pch.h)

add_library(${PROJECT_NAME}_LIB SHARED ${SRC})
add_executable(${PROJECT_NAME}_TEST ${TEST_SRC})
target_precompile_headers(${PROJECT_NAME}_TEST PRIVATE ${TEST_PCH})

target_link_directories(${PROJECT_NAME}_TEST PRIVATE deps/googletest/build/lib/)
target_link_libraries(${PROJECT_NAME}_TEST PRIVATE
		${PROJECT_NAME}_LIB
		libgtest.dylib
)

target_include_directories(${PROJECT_NAME}_TEST PRIVATE
	deps/googletest/googletest/include/
)
