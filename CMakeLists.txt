cmake_minimum_required(VERSION 3.2)

project(helloworld VERSION 0.0.$ENV{PROJECT_VERSION_PATCH})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

configure_file(version.h.in version.h @ONLY)

#Library Helloworld
add_library(helloworld lib.cpp)
set_target_properties(helloworld PROPERTIES
	INCLUDE_DIRECTORIES	${CMAKE_CURRENT_BINARY_DIR}
)

#Program Helloworld_cli
add_executable(helloworld_cli main.cpp)
set_target_properties(helloworld_cli PROPERTIES
	INCLUDE_DIRECTORIES	${CMAKE_CURRENT_BINARY_DIR}
)
target_link_libraries(helloworld_cli 
	helloworld
)

#Test
add_executable(test_version test.cpp)
target_include_directories(test_version PUBLIC ${Boost_INCLUDE_DIR} ${Boost_INCLUDE_DIR} )

target_link_libraries(test_version 
	${Boost_LIBRARIES}
	helloworld
)

#Дополнительный свойста компилятора для всех проектов
set_target_properties(helloworld_cli helloworld test_version PROPERTIES
	CXX_STANDARD 14
	CXX_STANDARD_REQUIRED ON
	COMPILE_OPTIONS -Wall
)

install(TARGETS helloworld_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKEGE_CONTACT aquila-2004@yandex.ru)

include(CPack)

enable_testing()

add_test(test_version test_version)