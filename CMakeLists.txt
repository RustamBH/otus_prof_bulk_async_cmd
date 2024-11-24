cmake_minimum_required(VERSION 3.5)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})

#--------------------------------------------------------------------------------------------------
# variables
set(LIB_NAME async)
set(BIN_NAME async_bin)
#--------------------------------------------------------------------------------------------------
# project
project(${LIB_NAME} VERSION ${PROJECT_VESRION})
#--------------------------------------------------------------------------------------------------
# builds
add_library(${LIB_NAME} STATIC
	async.cpp
	async.h
	CmdHandler.cpp
	CmdHandler.h
 	LogerAsync.cpp
	 LogerAsync.h
	SingletonGetCmd.cpp
	SingletonGetCmd.h
)

add_executable(${BIN_NAME}
	async.cpp
	async.h
	main.cpp
)
#--------------------------------------------------------------------------------------------------
# properties
set_target_properties(${LIB_NAME} ${BIN_NAME}
	PROPERTIES
	CXX_STANDARD 20
	CXX_STANDARD_REQUIRED ON
)
#--------------------------------------------------------------------------------------------------
# link libraries
target_link_libraries(
    ${BIN_NAME} PRIVATE
    ${LIB_NAME}
)

#--------------------------------------------------------------------------------------------------
# deb pkg
install(TARGETS ${LIB_NAME} RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")	
	
set(CPACK_PACKAGE_CONTACT example@example.com)

include(CPack)
