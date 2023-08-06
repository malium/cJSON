
set(CJSON_OUTPUT_NAME "cJSON")

if(WIN32)
	string(APPEND CJSON_OUTPUT_NAME "Win")
else()
	string(APPEND CJSON_OUTPUT_NAME "Lnx")
endif()

set(release FALSE)

if(NOT ${CMAKE_BUILD_TYPE} STREQUAL "Debug")
	#string(APPEND CJSON_OUTPUT_NAME "_Release")
	set(release TRUE)
else()
	#string(APPEND CJSON_OUTPUT_NAME "_Debug")
endif()
string(APPEND CJSON_OUTPUT_NAME "_" $<CONFIG>)

add_library(cJSON SHARED "cJSON.c" "cJSON_Utils.c" "cJSON.h" "cJSON_Utils.h")
set_target_properties(cJSON PROPERTIES OUTPUT_NAME ${CJSON_OUTPUT_NAME})

set_target_properties(cJSON PROPERTIES
						RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/bin
						RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/bin
						RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${PROJECT_SOURCE_DIR}/bin
						RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${PROJECT_SOURCE_DIR}/bin
						OUTPUT_NAME ${CJSON_OUTPUT_NAME}
						LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin
						LIBRARY_OUTPUT_NAME ${CJSON_OUTPUT_NAME}
						C_STANDARD 11)

target_compile_definitions(cJSON PRIVATE $<CONFIG>)

if(MSVC)
	set(MSVC_COMPILE_OPTIONS "/W4;/sdl;/Gm-;/EHsc;/GS;/Zc:inline;/Zc:forScope;/Zc:wchar_t;/permissive-;/TC")
	if(release EQUAL TRUE)
		string(APPEND MSVC_COMPILE_OPTIONS ";/Oi;/Ot;/Gy;/fp:fast")
	endif()
	target_compile_options(cJSON PRIVATE ${MSVC_COMPILE_OPTIONS})
endif()
