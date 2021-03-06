cmake_minimum_required(VERSION 2.8.1)
project(UnitTest++)

# get the main sources
file(GLOB SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} src/*.cpp src/*.h)
source_group("" FILES ${SRCS})

# get platform specific sources
if (WIN32)
    set(PLAT_DIR Win32)
else()
    set(PLAT_DIR Posix)
endif(WIN32)
file(GLOB PLAT_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} src/${PLAT_DIR}/*.cpp src/${PLAT_DIR}/*.h)
source_group(${PLAT_DIR} FILES ${PLAT_SRCS})

# create the lib
add_library(UnitTestPP STATIC ${SRCS} ${PLAT_SRCS})
set_target_properties(UnitTestPP PROPERTIES OUTPUT_NAME UnitTest++)
include_directories(src)

# build the test runner
file(GLOB TEST_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} src/tests/*.cpp src/tests/*.h)
source_group( "" FILES ${TEST_SRCS})
add_executable(TestUnitTestPP ${TEST_SRCS})
set_target_properties(TestUnitTestPP PROPERTIES OUTPUT_NAME TestUnitTest++)
target_link_libraries(TestUnitTestPP UnitTestPP)

# turn on testing
enable_testing()
add_custom_target(check COMMAND ${CMAKE_CTEST_COMMAND} -V)

# add the test runner as a test
add_test(NAME TestUnitTestPP COMMAND TestUnitTest++ ${CONFIG_PATH} ${CONFIG_TASKS_PATH} ${SOUND_LOG_PATH})
add_dependencies(check TestUnitTestPP)
