cmake_minimum_required(VERSION 3.10)
project(particleman)

set(PMAN_LIB particleman)

file(GLOB_RECURSE PMAN_SRC
	"particleman/particlefx_source/*.cpp"
	"public/interface.cpp"
)

if(NOT MSVC)
	add_definitions(-D_LINUX)
	add_definitions(-Dstricmp=strcasecmp -Dstrnicmp=strncasecmp -D_snprintf=snprintf -D_vsnprintf=vsnprintf)
	add_compile_options(-Wno-switch -Wno-implicit-exception-spec-mismatch -Wno-writable-strings)
endif()

include_directories(
	"common"
	"engine"
	"public"
	"public/steam"
	"public/tier1"
	"particleman/pm_shared"
	"particleman/particlefx_header"
)

add_library(${PMAN_LIB} MODULE ${PMAN_SRC})

install(TARGETS ${PMAN_LIB}
	DESTINATION "${GAME_DIR}/${CLIENT_INSTALL_DIR}/"
	PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE
		GROUP_READ GROUP_EXECUTE
		WORLD_READ WORLD_EXECUTE)

# Install PDB file on Windows
if(WIN32 AND MSVC)
	install(FILES $<TARGET_PDB_FILE:${PMAN_LIB}>
		DESTINATION "${GAME_DIR}/${CLIENT_INSTALL_DIR}/" OPTIONAL)
endif()