# Copyright (c) Microsoft Corporation.
# SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception

set(TR1_EXPECTED_RESULTS "${CMAKE_CURRENT_SOURCE_DIR}/expected_results.txt")
set(TR1_TEST_OUTPUT_DIR "${STL_TEST_OUTPUT_DIR}/tr1")
set(TR1_TEST_SUBDIRS_FILE "${CMAKE_CURRENT_SOURCE_DIR}/test.lst")
set(TR1_TEST_SUBDIRS_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")

configure_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/lit.site.cfg.in
  ${CMAKE_CURRENT_BINARY_DIR}/lit.site.cfg)

get_property(STL_LIT_CONFIG_MAP GLOBAL PROPERTY STL_LIT_CONFIG_MAP)
string(APPEND STL_LIT_CONFIG_MAP "map_config(\"${CMAKE_CURRENT_SOURCE_DIR}/lit.cfg\", \"${CMAKE_CURRENT_BINARY_DIR}/lit.site.cfg\")\n")
set_property(GLOBAL PROPERTY STL_LIT_CONFIG_MAP ${STL_LIT_CONFIG_MAP})

if(ENABLE_XUNIT_OUTPUT)
    list(APPEND TR1_ADDITIONAL_LIT_FLAGS "--xunit-xml-output" "${CMAKE_CURRENT_BINARY_DIR}/tr1.test.xml")
endif()

list(APPEND TR1_STL_LIT_COMMAND "${STL_LIT}"
                                "${ADDITIONAL_LIT_FLAGS}"
                                "${TR1_ADDITIONAL_LIT_FLAGS}"
                                "${CMAKE_CURRENT_SOURCE_DIR}")

add_test(NAME tr1 COMMAND ${Python3_EXECUTABLE} ${TR1_STL_LIT_COMMAND} COMMAND_EXPAND_LISTS)
set_tests_properties(tr1 PROPERTIES RUN_SERIAL TRUE)
