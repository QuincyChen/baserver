PROJECT(ProxyServer)

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
SET(CMAKE_BUILD_TYPE DEBUG)

#定义工程的顶级路径
SET(PROJDIR ${CMAKE_CURRENT_SOURCE_DIR}/../..)

#source files
AUX_SOURCE_DIRECTORY(${PROJDIR}/examples/proxy/server MAIN_SRCS)

#设置头文件的引用路径
INCLUDE_DIRECTORIES(
    ${PROJDIR}
)

add_executable(ProxyServer ${MAIN_SRCS})

IF (WIN32)
	MESSAGE(STATUS "Current OS is windows")
	add_definitions(-D_WIN32_WINNT=0x0501)
	
	#请在环境变量中定义BOOST_HOME
	INCLUDE_DIRECTORIES(
        $ENV{BOOST_HOME}
    )
	LINK_DIRECTORIES(
        $ENV{BOOST_HOME}/stage/lib
    )
	TARGET_LINK_LIBRARIES(ProxyServer 
        libboost_program_options-vc100-mt-gd-1_58
        libboost_system-vc100-mt-gd-1_58
        libboost_thread-vc100-mt-gd-1_58
	)		
ELSEIF (APPLE)
	MESSAGE(STATUS "Current OS is Apple systens.")
    TARGET_LINK_LIBRARIES(ProxyServer 
        boost_program_options-mt 
        boost_system-mt 
        boost_thread-mt)
ELSEIF (UNIX)
	MESSAGE(STATUS "Current OS is UNIX-like OS's. Including Linux and CygWin")
    TARGET_LINK_LIBRARIES(ProxyServer 
        boost_program_options 
        boost_system 
        boost_threadt
        pthread)
ENDIF ()