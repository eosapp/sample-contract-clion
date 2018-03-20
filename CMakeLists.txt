cmake_minimum_required(VERSION 3.9)
project(hello CXX)

set(target currency)
set(EOSIO_INSTALL_DIR $ENV{HOME}/eos/build/install)
set(ABIGEN ${EOSIO_INSTALL_DIR}/bin/abi_gen)

file(GLOB SOURCE_FILES "src/*.cpp")

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/src/currency.hpp" "${CMAKE_CURRENT_BINARY_DIR}" COPYONLY)
add_wast_target(${target} "${EOSIO_INSTALL_DIR}/include" ${CMAKE_CURRENT_BINARY_DIR})
add_custom_target(assembly ALL DEPENDS ${target} ${CMAKE_CURRENT_BINARY_DIR}/${target}.abi)
add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/${target}.abi
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/src/${target}.hpp
        COMMAND ${ABIGEN} -extra-arg=-c -extra-arg=--std=c++14 -extra-arg=--target=wasm32
        -extra-arg=-I${EOSIO_INSTALL_DIR}/include  -extra-arg=-I${CMAKE_CURRENT_BINARY_DIR}
        -extra-arg=-fparse-all-comments -destination-file=${CMAKE_CURRENT_BINARY_DIR}/${target}.abi -verbose=1 -context=${CMAKE_CURRENT_BINARY_DIR} ${target}.hpp --
        COMMENT "Generated ${CMAKE_CURRENT_BINARY_DIR}/${target}.abi")


