#
# Quickstart compile tests don't require any flags
#

# TODO run amalgamate first!

function(add_quickstart_test TEST_NAME)
  # Second argument is C++ standard name
  if (${ARGV1})
    if (MSVC)
      set(QS_STDCPPFLAG /std:${ARGV1})
    else()
      set(QS_STDCPPFLAG -Werror -std=${ARGV1})
    endif()
  else()
    if(MSVC)
      set(QS_STDCPPFLAG "")
    else()
      set(QS_STDCPPFLAG -Werror)
    endif()
  endif()

  add_test(
    NAME ${TEST_NAME}
    COMMAND ${CMAKE_CXX_COMPILER} ${QS_STDCPPFLAG} -I${PROJECT_SOURCE_DIR}/include -I${PROJECT_SOURCE_DIR}/src ${PROJECT_SOURCE_DIR}/src/simdjson.cpp quickstart.cpp
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/examples/quickstart
  )
  set_property(
    TEST ${TEST_NAME}
    APPEND PROPERTY DEPENDS simdjson-source ${PROJECT_SOURCE_DIR}/examples/quickstart/quickstart.cpp
  )
endfunction(add_quickstart_test)

add_quickstart_test(quickstart)
add_quickstart_test(quickstart11 c++11)
add_quickstart_test(quickstart14 c++14)
add_quickstart_test(quickstart17 c++17)
set_property(
  TEST quickstart quickstart11
  APPEND PROPERTY LABELS quicktests
)
set_property(
  TEST quickstart14 quickstart17
  APPEND PROPERTY LABELS quicktests
)
