IF(CMAKE_BUILD_TYPE MATCHES Debug)
  message("Debug build, linking against WinFab.Ktl.Linux.debug")
  include_directories(BEFORE "${EXTERNAL_DEPS_DIR}/WinFab.Ktl.Linux.debug/inc")
ELSE()
  message("Optimized build, linking against WinFab.Ktl.Linux.retail")
  include_directories(BEFORE "${EXTERNAL_DEPS_DIR}/WinFab.Ktl.Linux.retail/inc")
ENDIF()
include_directories(BEFORE "../linux_include")
include_directories(BEFORE "${EXTERNAL_DEPS_DIR}/WinFab.Ktl.Linux.retail/inc")
include_directories(BEFORE "../linux_include")
include_directories(BEFORE "../include")
include_directories(BEFORE "../config")
include_directories("Helpers")
include_directories("precomp")

add_compile_options(-rdynamic)

add_definitions(-DBOOST_TEST_ENABLED)

add_executable(${exe_DnsServiceTest}
../../../../test/BoostUnitTest/btest.cpp
DnsCacheTests.cpp
DnsParserTests.cpp
DnsServiceTests.cpp
Helpers/ComFabricPropertyValueResult.cpp
Helpers/ComFabricStatelessServicePartition2.cpp
Helpers/ComPropertyManager.cpp
Helpers/ComServiceManager.cpp
Helpers/ComServicePartitionResult.cpp
Helpers/DnsHelper.cpp
Helpers/DnsServiceSynchronizer.cpp
Helpers/FabricData.cpp
Helpers/NullTracer.cpp
Helpers/Runtime.cpp
precomp/stdafx.cpp
)

add_precompiled_header(${exe_DnsServiceTest} precomp/stdafx.h)

set_target_properties(${exe_DnsServiceTest} PROPERTIES 
    RUNTIME_OUTPUT_DIRECTORY ${TEST_OUTPUT_DIR}) 

find_library(KtlfullDbg NAMES ktlfull PATHS "${EXTERNAL_DEPS_DIR}/WinFab.Ktl.Linux.debug" NO_DEFAULT_PATH)
find_library(KtlfullOpt NAMES ktlfull PATHS "${EXTERNAL_DEPS_DIR}/WinFab.Ktl.Linux.retail" NO_DEFAULT_PATH)
    
target_link_libraries(${exe_DnsServiceTest}
  ${lib_DnsService}
  ${lib_DnsServiceConfig}
  ${lib_Common}
  ${lib_ServiceModel}
  ${lib_Serialization}
  ${lib_FabricCommon}
  ${lib_FabricClient}
  ${lib_FabricResources}
  ${lib_FabricRuntime}
  debug ${KtlfullDbg} optimized ${KtlfullOpt}
  ${BoostTest2}
  ${Cxx}
  ${CxxABI}
  z
  ssl
  crypto
  minizip
  z
  m
  rt
  pthread
  c
  dl
  xml2
  uuid
  unwind
  unwind-x86_64
  icuuc
)
