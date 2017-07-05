cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB testcoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    testcoin/external
    testcoin/include
    testcoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("testcoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  testcoin/external/miniupnpc/connecthostport.c
  testcoin/external/miniupnpc/igd_desc_parse.c
  testcoin/external/miniupnpc/minisoap.c
  testcoin/external/miniupnpc/miniupnpc.c
  testcoin/external/miniupnpc/miniwget.c
  testcoin/external/miniupnpc/minixml.c
  testcoin/external/miniupnpc/portlistingparse.c
  testcoin/external/miniupnpc/receivedata.c
  testcoin/external/miniupnpc/upnpcommands.c
  testcoin/external/miniupnpc/upnpreplyparse.c
  testcoin/src/Common/Base58.cpp
  testcoin/src/Common/CommandLine.cpp
  testcoin/src/Common/Util.cpp
  testcoin/src/Common/StringTools.cpp
  testcoin/src/Common/JsonValue.cpp
  testcoin/src/Common/ConsoleTools.cpp
  testcoin/src/Common/MemoryInputStream.cpp
  testcoin/src/Common/PathTools.cpp
  testcoin/src/Common/StdInputStream.cpp
  testcoin/src/Common/StdOutputStream.cpp
  testcoin/src/Common/StreamTools.cpp
  testcoin/src/Common/StringOutputStream.cpp
  testcoin/src/Common/StringView.cpp
  testcoin/src/Common/VectorOutputStream.cpp
  testcoin/src/crypto/blake256.c
  testcoin/src/crypto/chacha8.c
  testcoin/src/crypto/crypto-ops-data.c
  testcoin/src/crypto/crypto-ops.c
  testcoin/src/crypto/crypto.cpp
  testcoin/src/crypto/groestl.c
  testcoin/src/crypto/hash-extra-blake.c
  testcoin/src/crypto/hash-extra-groestl.c
  testcoin/src/crypto/hash-extra-jh.c
  testcoin/src/crypto/hash-extra-skein.c
  testcoin/src/crypto/hash.c
  testcoin/src/crypto/jh.c
  testcoin/src/crypto/keccak.c
  testcoin/src/crypto/oaes_lib.c
  testcoin/src/crypto/random.c
  testcoin/src/crypto/skein.c
  testcoin/src/crypto/slow-hash.c
  testcoin/src/crypto/slow-hash.cpp
  testcoin/src/crypto/tree-hash.c
  testcoin/src/CryptoNoteCore/BlockchainIndices.cpp
  testcoin/src/CryptoNoteCore/BlockchainMessages.cpp
  testcoin/src/CryptoNoteCore/BlockIndex.cpp
  testcoin/src/CryptoNoteCore/CoreConfig.cpp
  testcoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  testcoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  testcoin/src/CryptoNoteCore/Currency.cpp
  testcoin/src/CryptoNoteCore/MinerConfig.cpp
  testcoin/src/CryptoNoteCore/Transaction.cpp
  testcoin/src/CryptoNoteCore/Account.cpp
  testcoin/src/CryptoNoteCore/Blockchain.cpp
  testcoin/src/CryptoNoteCore/Checkpoints.cpp
  testcoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  testcoin/src/CryptoNoteCore/Core.cpp
  testcoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  testcoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  testcoin/src/CryptoNoteCore/Difficulty.cpp
  testcoin/src/CryptoNoteCore/IBlock.cpp
  testcoin/src/CryptoNoteCore/Miner.cpp
  testcoin/src/CryptoNoteCore/TransactionExtra.cpp
  testcoin/src/CryptoNoteCore/TransactionPool.cpp
  testcoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  testcoin/src/CryptoNoteCore/TransactionUtils.cpp
  testcoin/src/InProcessNode/InProcessNode.cpp
  testcoin/src/InProcessNode/InProcessNodeErrors.cpp
  testcoin/src/NodeRpcProxy/NodeErrors.cpp
  testcoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  testcoin/src/P2p/NetNodeConfig.cpp
  testcoin/src/Serialization/BinaryInputStreamSerializer.cpp
  testcoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  testcoin/src/Serialization/JsonInputValueSerializer.cpp
  testcoin/src/Serialization/JsonOutputStreamSerializer.cpp
  testcoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  testcoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  testcoin/src/Serialization/SerializationOverloads.cpp
  testcoin/src/Transfers/BlockchainSynchronizer.cpp
  testcoin/src/Transfers/SynchronizationState.cpp
  testcoin/src/Transfers/TransfersConsumer.cpp
  testcoin/src/Transfers/TransfersContainer.cpp
  testcoin/src/Transfers/TransfersSubscription.cpp
  testcoin/src/Transfers/TransfersSynchronizer.cpp
  testcoin/src/Wallet/LegacyKeysImporter.cpp
  testcoin/src/Wallet/WalletAsyncContextCounter.cpp
  testcoin/src/Wallet/WalletErrors.cpp
  testcoin/src/WalletLegacy/KeysStorage.cpp
  testcoin/src/WalletLegacy/WalletLegacy.cpp
  testcoin/src/WalletLegacy/WalletHelper.cpp
  testcoin/src/WalletLegacy/WalletLegacySerializer.cpp
  testcoin/src/WalletLegacy/WalletLegacySerialization.cpp
  testcoin/src/WalletLegacy/WalletTransactionSender.cpp
  testcoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  testcoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  testcoin/src/System/ContextGroup.cpp
  testcoin/src/System/Event.cpp
  testcoin/src/System/EventLock.cpp
  testcoin/src/System/InterruptedException.cpp
  testcoin/src/System/Ipv4Address.cpp
  testcoin/src/System/TcpStream.cpp
  testcoin/src/HTTP/HttpRequest.cpp
  testcoin/src/HTTP/HttpParser.cpp
  testcoin/src/HTTP/HttpParserErrorCodes.cpp
  testcoin/src/HTTP/HttpResponse.cpp
  testcoin/src/Rpc/HttpClient.cpp
  testcoin/src/Rpc/JsonRpc.cpp
  testcoin/src/P2p/NetNode.cpp
  testcoin/src/P2p/LevinProtocol.cpp
  testcoin/src/P2p/NetNodeConfig.cpp
  testcoin/src/P2p/PeerListManager.cpp
  testcoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  testcoin/src/Logging/ILogger.cpp
  testcoin/src/Logging/LoggerGroup.cpp
  testcoin/src/Logging/CommonLogger.cpp
  testcoin/src/Logging/LoggerManager.cpp
  testcoin/src/Logging/FileLogger.cpp
  testcoin/src/Logging/StreamLogger.cpp
  testcoin/src/Logging/ConsoleLogger.cpp
  testcoin/src/Logging/LoggerMessage.cpp
  testcoin/src/Logging/LoggerRef.cpp
  testcoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  testcoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  testcoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(testcoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} testcoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(testcoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES testcoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
