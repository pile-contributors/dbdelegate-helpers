# ============================================================================
#
# This file hosts all tests.
#
# If INCLUDE_PILE_SOURCES_DIRECTLY is ON then DBDELEGATE_SOURCES and
# DBDELEGATE_HEADERS are defined we add them to the executable. DBDELEGATE_LIBRARY
# is empty.
#
# On the other hand if INCLUDE_PILE_SOURCES_DIRECTLY is
# OFF then DBDELEGATE_SOURCES and DBDELEGATE_HEADERS are empty and we link
# against DBDELEGATE_LIBRARY that is now the name of the target that
# builds the library.
#
# ============================================================================

find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

if (NOT INCLUDE_PILE_SOURCES_DIRECTLY)
    set (DBDELEGATE_LIBRARY DbDelegate)
endif()

set (SOURCES
    ${DBDELEGATE_SOURCES}
    ${DBDELEGATE_HEADERS}
    ${DBMODEL_SOURCES}
    ${DBMODEL_HEADERS}
    ${DBSTRUCT_SOURCES}
    ${DBSTRUCT_HEADERS})

add_executable(test_basic
    ${SOURCES}
    basic.cc)

if (UNIX)
    find_package(Threads REQUIRED)
    list(APPEND GTEST_BOTH_LIBRARIES ${CMAKE_THREAD_LIBS_INIT})
    add_definitions(-fPIC)
endif()

target_link_libraries(test_basic
    ${GTEST_BOTH_LIBRARIES}
    ${DBDELEGATE_LIBRARY})
qt5_use_modules(test_basic Widgets Sql)

add_test(
    NAME test_basic
    COMMAND $<TARGET_FILE:test_basic>)


add_executable(test_gui_basic
    ${SOURCES}
    gui-basic.cc)
target_link_libraries(test_gui_basic
    ${GTEST_LIBRARIES}
    ${DBDELEGATE_LIBRARY})
qt5_use_modules(test_gui_basic Widgets Sql)
