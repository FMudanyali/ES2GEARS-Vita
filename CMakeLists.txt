cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{DOLCESDK})
	set(CMAKE_TOOLCHAIN_FILE "$ENV{DOLCESDK}/share/dolce.toolchain.cmake" CACHE PATH "toolchain file")
  else()
	message(FATAL_ERROR "Please define DOLCESDK to point to your SDK path!")
  endif()
endif()

project(ES2GEARS)
set(DOLCE_APP_NAME "ES2GEARS")
set(DOLCE_TITLEID  "ES2GEARS0")
set(DOLCE_VERSION  "01.00")
include("$ENV{DOLCESDK}/share/dolce.cmake" REQUIRED)
set(DOLCE_ELF_CREATE_FLAGS  "${DOLCE_ELF_CREATE_FLAGS}  -h 4194304")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu11 -o3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -o3")

link_directories(
	${CMAKE_CURRENT_BINARY_DIR}
)

add_executable(${PROJECT_NAME}
	es2gears.c
)
target_link_libraries(${PROJECT_NAME}
	freeglut-gles
	m
	pib
  	SceThreadmgr_stub
  	SceRtcUser_stub
  	SceCtrl_stub
  	SceGxm_stub
  	SceSysmem_stub
  	SceThreadmgr_stub
  	SceHid_stub
  	SceDisplay_stub
  	SceDisplayUser_stub
  	SceCommonDialog_stub
  	SceSysmodule_stub
  	SceTouch_stub
  	ScePower_stub
)
dolce_create_self(${PROJECT_NAME}.self ${PROJECT_NAME} UNSAFE)
dolce_create_vpk(${PROJECT_NAME}.vpk ${DOLCE_TITLEID} ${PROJECT_NAME}.self
  VERSION ${DOLCE_VERSION}
  NAME ${DOLCE_APP_NAME}
  FILE sce_sys/icon0.png sce_sys/icon0.png
  FILE sce_sys/livearea/contents/bg.png sce_sys/livearea/contents/bg.png
  FILE sce_sys/livearea/contents/startup.png sce_sys/livearea/contents/startup.png
  FILE sce_sys/livearea/contents/template.xml sce_sys/livearea/contents/template.xml
)

