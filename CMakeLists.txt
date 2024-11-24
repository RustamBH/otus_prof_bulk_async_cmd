cmake_minimum_required(VERSION 3.5)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})

project(bulk VERSION ${PROJECT_VESRION})

#--------------------------------------------------------------------------------------------------
# variables
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})

if(WIN32)
	set(LIB_NAME libasync)
	set(BIN_NAME async)
elseif(UNIX)
	set(LIB_NAME async)
	set(BIN_NAME async_bin)
endif()



#--------------------------------------------------------------------------------------------------
# project
project(${LIB_NAME})

if (MSVC)
	# enable VS-Solution filters support
	set_property(GLOBAL PROPERTY USE_FOLDERS ON)
endif (MSVC)


#--------------------------------------------------------------------------------------------------
# builds
add_library(${LIB_NAME} STATIC
	${SRC_DIR}/CmdHandler.cpp
	${SRC_DIR}/SingletonGetCmd.cpp
	${SRC_DIR}/LogerAsync.cpp
	${SRC_DIR}/async.cpp
)

add_executable(${BIN_NAME}
	${SRC_DIR}/async.cpp
	${SRC_DIR}/main.cpp
)


# properties
set_target_properties(${LIB_NAME} ${BIN_NAME}
	PROPERTIES
	CXX_STANDARD 20
	CXX_STANDARD_REQUIRED ON
)

#--------------------------------------------------------------------------------------------------
# compiler settings
if (MSVC)
	
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")

	add_compile_options("/std:c++latest")
	
	target_compile_options(${LIB_NAME} PRIVATE
		/W4
	)
	target_compile_options(${BIN_NAME} PRIVATE
		/W4
	)
else ()
	target_compile_options(${LIB_NAME} PRIVATE
		-std=c++20 -Wall -Wextra -pedantic -Werror
	)
	target_compile_options(${BIN_NAME} PRIVATE
		-std=c++20 -Wall -Wextra -pedantic -Werror
	)
endif (MSVC)


#--------------------------------------------------------------------------------------------------
# deb pkg
if (UNIX)
	install(TARGETS ${LIB_NAME} RUNTIME DESTINATION bin)

	set(CPACK_GENERATOR DEB)

	set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
	set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
	set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")	
		
	set(CPACK_PACKAGE_CONTACT example@example.com)

	include(CPack)
endif (UNIX)


