cmake_minimum_required(VERSION 2.8)
PROJECT(PluginManager)


set(HeadFileDirList "")  
#�ݹ��������Ŀ��ָ���ļ����µ�����.h�ļ����ļ�·��;  
file(GLOB SRCH_LIST_CInterface "CInterface/*.h")
file(GLOB SRCH_LIST_cJSON "CJsonHelper/cJSON/*.h")
file(GLOB SRCH_LIST_CJsonHelper "CJsonHelper/*.h")
file(GLOB SRCH_LIST_Common "Common/*.h")
file(GLOB SRCH_LIST_DataProcess "DataProcess/*.h")
file(GLOB SRCH_LIST_DynamicLoad "DynamicLoad/*.h")
file(GLOB SRCH_LIST_Global "Global/*.h")
file(GLOB SRCH_LIST_Thread "Thread/*.h")
file(GLOB SRCH_LIST_ZMQService "ZMQService/*.h")
set(SRCH_LIST 	${SRCH_LIST_CInterface} 
				${SRCH_LIST_cJSON} 
				${SRCH_LIST_CJsonHelper} 
				${SRCH_LIST_Common} 
				${SRCH_LIST_DataProcess}
				${SRCH_LIST_DynamicLoad}
				${SRCH_LIST_Global}
				${SRCH_LIST_Thread}
				${SRCH_LIST_ZMQService}) 
foreach(HeadFile ${SRCH_LIST})  
    #ȥ���ļ���,��ȡ·��;
    string(REGEX REPLACE "(.*)/.*\\.h$" "\\1" HeadFileDir ${HeadFile})
    #����·��;
    include_directories(${HeadFileDir})
    #ƴ��·��,Ȼ������CMAKE_INCLUDE_PATH����ʹ��find_path��������Ŀ¼;
    set(HeadFileDirList "${HeadFileDirList}${HeadFileDir} ")
endforeach()

#�����ⲿͷ�ļ�;
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ThirdParty/include)


#����cpp�ļ�;
SET(SRC_C 	${CMAKE_CURRENT_SOURCE_DIR}/CJsonHelper/cJSON/cJSON.c
			${CMAKE_CURRENT_SOURCE_DIR}/CJsonHelper/cJSON/cJSON_Utils.c)
SET(SRCS ${CMAKE_CURRENT_SOURCE_DIR}/PluginManager_all.cpp)


MESSAGE(STATUS "this is BINARY dir: " ${PluginManager_BINARY_DIR})
MESSAGE(STATUS "this is SOURCE dir: " ${PluginManager_SOURCE_DIR})
MESSAGE(STATUS "this is CURRENT dir: " ${CMAKE_CURRENT_SOURCE_DIR})


IF(UNIX)
ADD_DEFINITIONS(-std=c++11)
ADD_DEFINITIONS(-DLINUX -g -O0)
SET(CMAKE_CXX_COMPILER g++)
SET(CMAKE_CXX_FLAGS -Wall)
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/bin/linux/Release64)

LINK_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/bin/linux/Release64)

ADD_EXECUTABLE(PluginManager ${SRC_C} ${SRCS})

TARGET_LINK_LIBRARIES(PluginManager pthread log4cplus-1.2 zmq dl)

ENDIF(UNIX)

