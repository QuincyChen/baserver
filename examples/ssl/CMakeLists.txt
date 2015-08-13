PROJECT(Ssl)

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
SET(CMAKE_BUILD_TYPE DEBUG)

#定义工程的顶级路径
SET(PROJDIR ${CMAKE_CURRENT_SOURCE_DIR}/../..)

#source files
AUX_SOURCE_DIRECTORY(${PROJDIR}/examples/ssl/server SERVER_SRCS)
AUX_SOURCE_DIRECTORY(${PROJDIR}/examples/ssl/client CLIENT_SRCS)

#设置头文件的引用路径
INCLUDE_DIRECTORIES(
    ${PROJDIR}
)

#SSL Server
add_executable(SslServer ${SERVER_SRCS})

#SSL Client    
add_executable(SslClient ${CLIENT_SRCS})
 
IF (WIN32)
	MESSAGE(STATUS "Current OS is windows")
	add_definitions(-D_WIN32_WINNT=0x0501)
	
	#请在环境变量中定义BOOST_HOME,SSL_HOME
	INCLUDE_DIRECTORIES(
        $ENV{BOOST_HOME}
        $ENV{SSL_HOME}/include
    )
	LINK_DIRECTORIES(
        $ENV{BOOST_HOME}/stage/lib
        $ENV{SSL_HOME}/lib
    )
	TARGET_LINK_LIBRARIES(SslServer 
        libboost_program_options-vc100-mt-gd-1_58
        libboost_system-vc100-mt-gd-1_58
        libboost_thread-vc100-mt-gd-1_58
        ssl crypto
	)
	TARGET_LINK_LIBRARIES(SslClient 
        libboost_program_options-vc100-mt-gd-1_58
        libboost_system-vc100-mt-gd-1_58
        libboost_thread-vc100-mt-gd-1_58
        ssl crypto
	)	
ELSEIF (APPLE)
	MESSAGE(STATUS "Current OS is Apple systens.")
    TARGET_LINK_LIBRARIES(SslServer 
        boost_program_options-mt 
        boost_system-mt 
        boost_thread-mt
        ssl crypto)
    TARGET_LINK_LIBRARIES(SslClient 
        boost_program_options-mt 
        boost_system-mt 
        boost_thread-mt
        ssl crypto)
ELSEIF (UNIX)
	MESSAGE(STATUS "Current OS is UNIX-like OS's. Including Linux and CygWin")
    TARGET_LINK_LIBRARIES(SslServer 
        boost_program_options
        boost_system
        boost_thread
        ssl crypto pthread)
    TARGET_LINK_LIBRARIES(SslClient 
        boost_program_options
        boost_system
        boost_thread
        ssl crypto pthread)
ENDIF ()