cmake_minimum_required(VERSION 3.16)
project(boost_test)

#Boost 라이브러리 설치 확인
find_package(Boost)

# Search OpenSSL
find_package(PkgConfig REQUIRED)
pkg_search_module(OPENSSL REQUIRED openssl)

if( OPENSSL_FOUND )
    include_directories(${OPENSSL_INCLUDE_DIRS})
    message(STATUS "Using OpenSSL ${OPENSSL_VERSION}")
else()
    # Error; with REQUIRED, pkg_search_module() will throw an error by it's own
endif()

if(Boost_FOUND)
    # include 패스 설정
    include_directories(${Boost_INCLUDE_DIRS}
			/usr/local/opt/openssl@3/include)
    LINK_DIRECTORIES(${Boost_LIBRARY_DIRS}
		      /usr/local/opt/openssl@3/lib)
endif()

set(CMAKE_CXX_STANDARD 14)
SET(USED_LIBS ${Boost_SYSTEM_LIBRARY} ${Boost_THREAD_LIBRARY} ${Boost_REGEX_LIBRARY}  ${OPENSSL_LIBRARIES})

add_executable(boost_test main.cpp)
add_executable(client client.cpp)
add_executable(server server.cpp)

target_link_libraries(server
    ${Boost_LIBRARIES}
   ${OPENSSL_LIBRARIES}
) 

target_link_libraries(client
    ${Boost_LIBRARIES}
   ${OPENSSL_LIBRARIES}
) 
