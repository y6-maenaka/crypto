cmake_minimum_required( VERSION 3.2 )

project( OPENSSL_WRAPPER CXX )

find_package( PkgConfig REQUIRED )
find_package( OPENSSL REQUIRED )

if( OPENSSL_FOUND )
  include_directories( ${OPENSSL_INCLUDE_DIR} )
  message( STATUS "Using OpenSSL ${OPENSSL_VERSION}" )
  message( STATUS "Openssl IncludeDir :: ${OPENSSL_INCLUDE_DIR}" )
else()
  message("(Failure) Found OPENSSL")
endif()




add_library( W_RSA_MANAGER SHARED
			  ./w_rsa/rsa_manager.cpp
)
target_link_libraries( W_RSA_MANAGER PUBLIC
						${OPENSSL_LIBRARIES}
)

add_library( W_EVP_PKEY SHARED
			  ./w_evp_pkey/evp_pkey.cpp
)
target_link_libraries( W_EVP_PKEY PUBLIC
						${OPENSSL_LIBRARIES}
)



add_executable(runner runner.cpp)
target_link_libraries( runner PRIVATE 
						W_RSA_MANAGER
						W_EVP_PKEY
)



add_definitions( -std=c++20 -w )
