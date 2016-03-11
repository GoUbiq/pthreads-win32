cmake_minimum_required (VERSION 2.6)
project (pthread-win32)

add_definitions(-D_TIMESPEC_DEFINED)

set(variables
	CMAKE_C_FLAGS_DEBUG
	CMAKE_C_FLAGS_MINSIZEREL
	CMAKE_C_FLAGS_RELEASE
	CMAKE_C_FLAGS_RELWITHDEBINFO
	CMAKE_CXX_FLAGS_DEBUG
	CMAKE_CXX_FLAGS_MINSIZEREL
	CMAKE_CXX_FLAGS_RELEASE
	CMAKE_CXX_FLAGS_RELWITHDEBINFO
)
foreach(variable ${variables})
	if(${variable} MATCHES "/MD")
		string(REGEX REPLACE "/MD" "/MT" ${variable} "${${variable}}")
	endif()
endforeach()

SET (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /SAFESEH:NO")
SET (CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /SAFESEH:NO")
SET (CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} /SAFESEH:NO")

set(pthreadVC2_FILES
	pthread.c
	implement.h
	pthread.h
	sched.h
	semaphore.h
)

add_library(pthreadVC2 SHARED
	${pthreadVC2_FILES})
target_include_directories(pthreadVC2
	PUBLIC .)
target_link_libraries(pthreadVC2)