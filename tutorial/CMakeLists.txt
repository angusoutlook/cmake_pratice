cmake_minimum_required (VERSION 2.6)
project (Tutorial)
include (CTest)

enable_testing()  

option (USE_MYMATH "Use tutorial provided math implemetation" ON)

set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)

configure_file (
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  )

include_directories ("${PROJECT_BINARY_DIR}")

include (CheckFunctionExists)
check_function_exists (log HAVE_LOG)
check_function_exists (exp HAVE_EXP)

if (USE_MYMATH)
  include_directories( "${PROJECT_SOURCE_DIR}/MathFunctions")
  add_subdirectory (MathFunctions)
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)

add_executable (Tutorial tutorial.cxx)
target_link_libraries (Tutorial ${EXTRA_LIBS})

install (TARGETS Tutorial DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h" DESTINATION include)

#add_test (TutorialUsage Tutorial)
#set_tests_properties (TutorialUsage
#  PROPERTIES
#  PASS_REGULAR_EXPRESSION "Usage:.*number"
#  )

#macro (do_test arg result)
#  add_test (TutorialComp${arg} Tutorial ${arg})
#  set_tests_properties (TutorialCom${arg}
#    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
#    )
#endmacro (do_test)

#do_test (4 "4 is 2")
#do_test (9 "9 is 3")
#do_test (5 "5 is 2.236")
#do_test (7 "7 is 2.645")
#do_test (25 "25 is 5")
#do_test (-25 "-25 is 0")
#do_test (0.0001 "0.0001 is 0.01")

# build a CPack driven installer package
include (InstallRequiredSystemLibraries)
set (CPACK_RESOURCE_FILE_LICENSE  
     "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
set (CPACK_PACKAGE_VERSION_MAJOR "${Tutorial_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${Tutorial_VERSION_MINOR}")
include (CPack)

