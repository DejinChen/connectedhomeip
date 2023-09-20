if(NOT MATTER_SDK_PATH)
    get_filename_component(MATTER_SDK_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../../../.. REALPATH)
endif()

include(${CMAKE_CURRENT_LIST_DIR}/ota-image.cmake)

set(CHIP_REQUIRE_COMPONENTS esp_eth freertos lwip bt mbedtls fatfs app_update console openthread nvs_flash spi_flash)

if(NOT "${IDF_TARGET}" STREQUAL "esp32h2")
    list(APPEND CHIP_REQUIRE_COMPONENTS mdns)
endif()

set(SRC_DIRS_LIST "."
                  "${MATTER_SDK_PATH}/src/access"
                  "${MATTER_SDK_PATH}/src/access/examples"
                  "${MATTER_SDK_PATH}/src/app"
                  "${MATTER_SDK_PATH}/src/app/icd/"
                  "${MATTER_SDK_PATH}/src/app/MessageDef"
                  "${MATTER_SDK_PATH}/src/app/reporting"
                  "${MATTER_SDK_PATH}/src/app/server"
                  "${MATTER_SDK_PATH}/src/ble"
                  "${MATTER_SDK_PATH}/src/credentials"
                  "${MATTER_SDK_PATH}/src/credentials/attestation_verifier"
                  "${MATTER_SDK_PATH}/src/credentials/examples"
                  "${MATTER_SDK_PATH}/src/credentials/tests"
                  "${MATTER_SDK_PATH}/src/crypto"
                  # "${MATTER_SDK_PATH}/src/darwin"
                  # "${MATTER_SDK_PATH}/src/include"
                  "${MATTER_SDK_PATH}/src/inet"
                  "${MATTER_SDK_PATH}/src/lib/address_resolve"
                  "${MATTER_SDK_PATH}/src/lib/asn1"
                  "${MATTER_SDK_PATH}/src/lib/core"
                  "${MATTER_SDK_PATH}/src/lib/dnssd"
                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns"
                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core"
                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/records"
                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/responders"
                  "${MATTER_SDK_PATH}/src/lib/support"
                  "${MATTER_SDK_PATH}/src/lib/support/logging"
                  "${MATTER_SDK_PATH}/src/lib/support/verhoeff"
                  "${MATTER_SDK_PATH}/src/lib/shell"
                  "${MATTER_SDK_PATH}/src/lib/shell/commands"
                  # "${MATTER_SDK_PATH}/src/lwip"
                  "${MATTER_SDK_PATH}/src/messaging"
                  "${MATTER_SDK_PATH}/src/platform"
                  "${MATTER_SDK_PATH}/src/platform/ESP32"
                  "${MATTER_SDK_PATH}/src/platform/ESP32/nimble"
                  # "${MATTER_SDK_PATH}/src/platform/ESP32/bluedroid"
                  "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook"
                  "${MATTER_SDK_PATH}/src/platform/OpenThread/"
                  "${MATTER_SDK_PATH}/src/protocols"
                  "${MATTER_SDK_PATH}/src/protocols/bdx"
                  "${MATTER_SDK_PATH}/src/protocols/echo"
                  "${MATTER_SDK_PATH}/src/protocols/secure_channel"
                  "${MATTER_SDK_PATH}/src/protocols/user_directed_commissioning"
                  # "${MATTER_SDK_PATH}/src/pybindings"
                  "${MATTER_SDK_PATH}/src/setup_payload"
                  "${MATTER_SDK_PATH}/src/system"
                  "${MATTER_SDK_PATH}/src/tracing"
                  "${MATTER_SDK_PATH}/src/transport"
                  "${MATTER_SDK_PATH}/src/transport/raw"
                  "${MATTER_SDK_PATH}/zzz_generated/app-common/app-common/zap-generated")

set(INCLUDE_DIRS_LIST "."
                      "${MATTER_SDK_PATH}/src"
                      "${MATTER_SDK_PATH}/src/access"
                      "${MATTER_SDK_PATH}/src/access/examples"
                      "${MATTER_SDK_PATH}/src/app"
                      "${MATTER_SDK_PATH}/src/app/icd/"
                      "${MATTER_SDK_PATH}/src/app/MessageDef"
                      "${MATTER_SDK_PATH}/src/app/reporting"
                      "${MATTER_SDK_PATH}/src/app/server"
                      "${MATTER_SDK_PATH}/src/ble"
                      "${MATTER_SDK_PATH}/src/controller"
                      "${MATTER_SDK_PATH}/src/credentials"
                      "${MATTER_SDK_PATH}/src/credentials/attestation_verifier"
                      "${MATTER_SDK_PATH}/src/credentials/tests"
                      "${MATTER_SDK_PATH}/src/crypto"
                      # "${MATTER_SDK_PATH}/src/darwin"
                      "${MATTER_SDK_PATH}/src/include"
                      "${MATTER_SDK_PATH}/src/inet"
                      "${MATTER_SDK_PATH}/src/lib/address_resolve"
                      "${MATTER_SDK_PATH}/src/lib/asn1"
                      "${MATTER_SDK_PATH}/src/lib/core"
                      "${MATTER_SDK_PATH}/src/lib/dnssd"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/records"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/responders"
                      "${MATTER_SDK_PATH}/src/lib/support"
                      "${MATTER_SDK_PATH}/src/lib/support/logging"
                      "${MATTER_SDK_PATH}/src/lib/support/verhoeff"
                      "${MATTER_SDK_PATH}/src/lib/shell"
                      "${MATTER_SDK_PATH}/src/lib/shell/commands"
                      # "${MATTER_SDK_PATH}/src/lwip"
                      "${MATTER_SDK_PATH}/src/messaging"
                      "${MATTER_SDK_PATH}/src/platform"
                      "${MATTER_SDK_PATH}/src/platform/ESP32"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/nimble"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/bluedroid"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook"
                      "${MATTER_SDK_PATH}/src/platform/OpenThread/"
                      "${MATTER_SDK_PATH}/src/protocols"
                      "${MATTER_SDK_PATH}/src/protocols/bdx"
                      "${MATTER_SDK_PATH}/src/protocols/echo"
                      "${MATTER_SDK_PATH}/src/protocols/secure_channel"
                      "${MATTER_SDK_PATH}/src/pybindings"
                      "${MATTER_SDK_PATH}/src/setup_payload"
                      "${MATTER_SDK_PATH}/src/system"
                      "${MATTER_SDK_PATH}/src/tracing"
                      "${MATTER_SDK_PATH}/src/transport"
                      "${MATTER_SDK_PATH}/src/transport/raw"
                      # "${MATTER_SDK_PATH}/out/host/gen/include"
                      "${MATTER_SDK_PATH}/third_party/nlfaultinjection/repo/include"
                      "${MATTER_SDK_PATH}/third_party/nlassert/repo/include"
                      "${MATTER_SDK_PATH}/third_party/nlio/repo/include"
                      "${MATTER_SDK_PATH}/zzz_generated/app-common")

set(EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/app/icd/ICDEventManager.cpp"
                      "${MATTER_SDK_PATH}/src/app/icd/ICDManager.cpp"
                      "${MATTER_SDK_PATH}/src/controller/ExamplePersistentStorage.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/attestation_verifier/FileAttestationTrustStore.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/CHIPCert_error_test_vectors.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/CHIPCert_test_vectors.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/CHIPCert_unit_test_vectors.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/FuzzChipCert.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestCertificationDeclaration.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestChipCert.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestCommissionerDUTVectors.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestDeviceAttestationConstruction.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestDeviceAttestationCredentials.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestFabricTable.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestGroupDataProvider.cpp"
                      "${MATTER_SDK_PATH}/src/credentials/tests/TestPersistentStorageOpCertStore.cpp"
                      "${MATTER_SDK_PATH}/src/crypto/CHIPCryptoPALOpenSSL.cpp"
                      "${MATTER_SDK_PATH}/src/crypto/CHIPCryptoPALPSA.cpp"
                      "${MATTER_SDK_PATH}/src/crypto/PSAOperationalKeystore.cpp"
                      "${MATTER_SDK_PATH}/src/crypto/PSASessionKeystore.cpp"
                      "${MATTER_SDK_PATH}/src/inet/InetFaultInjection.cpp"
                      "${MATTER_SDK_PATH}/src/inet/TCPEndPointImplOpenThread.cpp"
                      "${MATTER_SDK_PATH}/src/inet/TCPEndPointImplSockets.cpp"
                      "${MATTER_SDK_PATH}/src/inet/UDPEndPointImplNetworkFramework.cpp"
                      "${MATTER_SDK_PATH}/src/inet/UDPEndPointImplOpenThread.cpp"
                      "${MATTER_SDK_PATH}/src/inet/UDPEndPointImplSockets.cpp"
                      "${MATTER_SDK_PATH}/src/lib/address_resolve/tool.cpp"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/Advertiser_ImplNone.cpp"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/Resolver_ImplNone.cpp"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/AddressPolicy_LibNlImpl.cpp"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/Logging.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/UnitTestRegistration.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/UnitTestUtils.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/JniReferences.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/CHIPFaultInjection.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/CHIPMem-Simple.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/MainLoopAmeba.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/MainLoopCYW30739.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/MainLoopDefault.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/MainLoopMW320.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/MainLoopSilabs.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/MainLoopZephyr.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_ameba.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_asr.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_bouffalolab.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_cc13x2_26x2.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_cc13x4_26x4.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_cyw30739.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_k32w.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_mbed.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_mt793x.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_mw320.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_openiotsdk.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_qpg.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_silabs.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_stdio.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/streamer_zephyr.cpp"
                      "${MATTER_SDK_PATH}/src/lib/shell/commands/NFC.cpp"
                      "${MATTER_SDK_PATH}/src/platform/DeviceSafeQueue.cpp"
                      "${MATTER_SDK_PATH}/src/platform/SyscallStubs.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ConnectivityManagerImpl_Ethernet.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ESP32CHIPCryptoPAL.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ESP32DeviceInfoProvider.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ESP32FactoryDataProvider.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ESP32SecureCertDACProvider.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/NetworkCommissioningDriver_Ethernet.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/WiFiDnssdImpl.cpp"
                      "${MATTER_SDK_PATH}/src/platform/OpenThread/GenericThreadStackManagerImpl_OpenThread_LwIP.cpp"
                      "${MATTER_SDK_PATH}/src/system/SystemFaultInjection.cpp"
                      "${MATTER_SDK_PATH}/src/system/SystemLayerImplSelect.cpp"
                      "${MATTER_SDK_PATH}/src/transport/TraceMessage.cpp")

set(REQUIRES_LIST )

list(APPEND INCLUDE_DIRS_LIST "${CMAKE_CURRENT_BINARY_DIR}")

# ESP-IDF lets user set software version string by two ways:
# 1. Project's CMakeLists.txt file and 2. Config option
# It depends on CONFIG_APP_PROJECT_VER_FROM_CONFIG option
# So, below makes the same provision for software version number
if (CONFIG_APP_PROJECT_VER_FROM_CONFIG)
    set(chip_config_software_version_number ${CONFIG_DEVICE_SOFTWARE_VERSION_NUMBER})
elseif (DEFINED PROJECT_VER_NUMBER)
    set(chip_config_software_version_number ${PROJECT_VER_NUMBER})
else()
    set(chip_config_software_version_number 0)
endif()

if (CONFIG_CHIP_CONFIG_IM_PRETTY_PRINT)
    set(enable_im_pretty_print 1)
else()
    set(enable_im_pretty_print 0)
endif()

# Config the chip log level by IDF menuconfig
if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 1)
    set(chip_error_logging 1)
else()
    set(chip_error_logging 0)
endif()

if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 3)
    set(chip_progress_logging 1)
else()
    set(chip_progress_logging 0)
endif()

if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 4)
    set(chip_detail_logging 1)
else()
    set(chip_detail_logging 0)
endif()

if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 5)
    set(chip_automation_logging 1)
else()
    set(chip_automation_logging 0)
endif()

if(CONFIG_ENABLE_CHIPOBLE)
    set(chip_config_network_layer_ble 1)
else()
    set(chip_config_network_layer_ble 0)
endif()

if(CONFIG_DISABLE_IPV4)
    set(chip_inet_config_enable_ipv4 0)
else()
    set(chip_inet_config_enable_ipv4 1)
endif()

if(CONFIG_DISABLE_READ_CLIENT)
    chip_gn_arg_append("chip_enable_read_client"             "false")
endif()

if(CHIP_CODEGEN_PREGEN_DIR)
    set("chip_code_pre_generated_directory"  "\"${CHIP_CODEGEN_PREGEN_DIR}\"")
endif()

set(chip_config_memory_management_malloc 1)

if (CONFIG_BUILD_CHIP_TESTS)
    set(chip_build_tests 1)
else()
    set(chip_build_tests 0)
endif()

if (CONFIG_IM_PRETTY_PRINT)
    set(enable_im_pretty_print 1)
endif()

if (NOT CONFIG_USE_MINIMAL_MDNS)
    set("chip_mdns" "\"platform\"")
    list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/lib/dnssd/ActiveResolveAttempts.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/Advertiser_ImplMinimalMdns.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/IncrementalResolve.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/MinimalMdnsServer.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/Resolver_ImplMinimalMdns.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/Parser.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/RecordData.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/ResponseSender.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/Server.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/AddressPolicy.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core/QName.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core/QNameString.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core/RecordWriter.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/responders/IP.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/responders/QueryResponder.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/records/IP.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/records/ResourceRecord.cpp"
                                  "${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/AddressPolicy_DefaultImpl.cpp")
else()
    set("chip_mdns" "\"minimal\"")
    list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/lib/dnssd/Discovery_ImplPlatform.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/DnssdImpl.cpp")
endif()

if (CONFIG_ENABLE_CHIP_SHELL)
    set(chip_build_libshell 1)
endif()

if (CONFIG_ENABLE_CHIP_CONTROLLER_BUILD)
    set(chip_build_controller 1)
    list(APPEND SRC_DIRS_LIST "${MATTER_SDK_PATH}/src/controller")
endif()

if (CONFIG_ENABLE_WIFI_STATION OR CONFIG_ENABLE_WIFI_AP)
    set(chip_enable_wifi 1)
else()
    set(chip_enable_wifi 0)
    list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/platform/ESP32/ConnectivityManagerImpl_WiFi.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/NetworkCommissioningDriver.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteHook.c"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteTable.c"
                                  "${MATTER_SDK_PATH}/src/lib/shell/commands/WiFi.cpp")
endif()

if (CONFIG_ENABLE_CHIPOBLE)
    set(chip_enable_chipoble 1)
endif()

if ((CONFIG_BT_ENABLED) AND (CONFIG_ENABLE_CHIPOBLE))
	if (CONFIG_BT_NIMBLE_ENABLED)
        set(chip_bt_nimble_enabled 1)
    else()
        set(chip_bt_bluedroid_enabled 1)
        list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/platform/ESP32/nimble/BLEManagerImpl.cpp")
    endif()
endif()

if ((CONFIG_ENABLE_ESP32_BLE_CONTROLLER))
    set(chip_enable_ble_controller 1)
else()
    list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/platform/ESP32/nimble/ChipDeviceScanner.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/nimble/misc.c"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/nimble/peer.c")
endif()

if (CONFIG_ENABLE_ETHERNET_TELEMETRY)
    set(chip_enable_ethernet 1)
endif()

if (CONFIG_ENABLE_MATTER_OVER_THREAD)
    set(chip_enable_openthread 1)
else()
    set(chip_enable_openthread 0)
    list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/platform/ESP32/OpenthreadLauncher.c"
                                  "${MATTER_SDK_PATH}/src/platform/ESP32/ThreadStackManagerImpl.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/OpenThread/DnssdImpl.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/OpenThread/GenericNetworkCommissioningThreadDriver.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/OpenThread/OpenThreadUtils.cpp"
                                  "${MATTER_SDK_PATH}/src/platform/OpenThread/OpenThreadDnssdImpl.cpp")
endif()

if (CONFIG_OPENTHREAD_FTD)
    set(chip_openthread_ftd 1)
else()
    set(chip_openthread_ftd 0)
endif()

if (CONFIG_ENABLE_OTA_REQUESTOR)
    set(chip_enable_ota_requestor 1)
else()
    set(chip_enable_ota_requestor 0)
    list(APPEND EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/platform/ESP32/OTAImageProcessorImpl.cpp")
endif()

if (CONFIG_ENABLE_ROTATING_DEVICE_ID)
    set(chip_enable_additional_data_advertising 1)
    set(chip_enable_rotating_device_id 1)
else()
    set(chip_enable_additional_data_advertising 0)
    set(chip_enable_rotating_device_id 0)
endif()

if (CONFIG_ENABLE_ROUTE_HOOK)
    set(chip_enable_route_hook 1)
endif()

if (CONFIG_CHIP_ENABLE_EXTERNAL_PLATFORM)
    set("chip_device_platform" "\"external\"")
    if (CONFIG_ENABLE_CHIP_SHELL)
        set("chip_shell_platform" "\"esp32\"")
    endif()
    set("chip_platform_target" "\"//${CONFIG_CHIP_EXTERNAL_PLATFORM_DIR}\"")
endif()

# Set up CHIP project configuration file

if (CONFIG_CHIP_PROJECT_CONFIG)
    get_filename_component(CHIP_PROJECT_CONFIG
        ${CONFIG_CHIP_PROJECT_CONFIG}
        REALPATH
        BASE_DIR ${CMAKE_SOURCE_DIR}
    )
    set(CHIP_PROJECT_CONFIG "<${CHIP_PROJECT_CONFIG}>")
else()
    set(CHIP_PROJECT_CONFIG "")
endif()

if (CONFIG_ENABLE_ESP32_FACTORY_DATA_PROVIDER)
    set(chip_use_transitional_commissionable_data_provider 0)
    set(chip_use_factory_data_provider 1)
else()
    set(chip_use_transitional_commissionable_data_provider 1)
    set(chip_use_factory_data_provider 0)
endif()

if (CONFIG_ENABLE_ESP32_DEVICE_INFO_PROVIDER)
    set(chip_use_device_info_provider 1)
endif()

if (CONFIG_SEC_CERT_DAC_PROVIDER)
    set(chip_use_secure_cert_dac_provider 1)
endif()

if (CONFIG_ENABLE_ESP_INSIGHTS_TRACE)
    chip_gn_arg_append("matter_enable_esp_insights_trace"                    "true")
endif()

if (CONFIG_USE_ESP32_ECDSA_PERIPHERAL)
    set(chip_use_esp32_ecdsa_peripheral 1)
endif()

idf_component_register(SRC_DIRS             ${SRC_DIRS_LIST}
                       EXCLUDE_SRCS         ${EXCLUDE_SRCS_LIST}
                       INCLUDE_DIRS         ${INCLUDE_DIRS_LIST}
                       PRIV_REQUIRES        ${CHIP_REQUIRE_COMPONENTS}
                       REQUIRES             ${REQUIRES_LIST})

target_compile_options(${COMPONENT_LIB} PUBLIC
                       "-DCHIP_ADDRESS_RESOLVE_IMPL_INCLUDE_HEADER=<lib/address_resolve/AddressResolve_DefaultImpl.h>"
                       "-DCHIP_CONFIG_SOFTWARE_VERSION_NUMBER=0")

if (CHIP_PROJECT_CONFIG)
    set("chip_project_config_include" "\"${CHIP_PROJECT_CONFIG}\"")
    set("chip_system_project_config_include" "\"${CHIP_PROJECT_CONFIG}\"")
    # target_compile_options(${COMPONENT_LIB} PUBLIC
    #                        "-DCHIP_PROJECT_CONFIG_INCLUDE=${chip_project_config_include}"
    #                        "-DSYSTEM_PROJECT_CONFIG_INCLUDE=${chip_system_project_config_include}")
endif()

if (CONFIG_ENABLE_OTA_REQUESTOR)
    target_compile_options(${COMPONENT_LIB} PUBLIC "-DCHIP_DEVICE_CONFIG_ENABLE_OTA_REQUESTOR=1")
endif()

if (CONFIG_USE_MINIMAL_MDNS)
    target_compile_options(${COMPONENT_LIB} PUBLIC "-DCHIP_MINMDNS_DEFAULT_POLICY=1"
                                                   "-DCHIP_MINMDNS_USE_EPHEMERAL_UNICAST_PORT=0"
                                                   "-DCHIP_MINMDNS_HIGH_VERBOSITY=0")
endif()

if (NOT CMAKE_BUILD_EARLY_EXPANSION)
    if (CONFIG_COMPILER_OPTIMIZATION_DEFAULT OR CONFIG_COMPILER_OPTIMIZATION_NONE)
        set(is_debug TRUE)
        target_compile_options(${COMPONENT_LIB} PUBLIC "-DLWIP_DEBUG=1")
    else()
        if (NOT CONFIG_COMPILER_OPTIMIZATION_ASSERTIONS_DISABLE)
            message(FATAL_ERROR "CONFIG_COMPILER_OPTIMIZATION_ASSERTIONS_DISABLE shall be set")
        endif()
        set(is_debug FALSE)
    endif()
endif()

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/app/AppBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/app/AppBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/asn1/ASN1OID.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/asn1/ASN1OID.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/ble/BleBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/ble/BleBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/core/CHIPBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/core/CHIPBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/crypto/CryptoBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/crypto/CryptoBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/inet/InetBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/inet/InetBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/lwip/lwip_buildconfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/lwip/lwip_buildconfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/platform/CHIPDeviceBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/platform/CHIPDeviceBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/setup_payload/CHIPAdditionalDataPayloadBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/setup_payload/CHIPAdditionalDataPayloadBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/system/SystemBuildConfig.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/system/SystemBuildConfig.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/matter/tracing/build_config.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/matter/tracing/build_config.h)

configure_file(${CMAKE_CURRENT_LIST_DIR}/include/CHIPVersion.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/CHIPVersion.h)

# Build Matter OTA image
if (CONFIG_CHIP_OTA_IMAGE_BUILD)
    chip_ota_image(chip-ota-image
        INPUT_FILES ${BUILD_DIR}/${CMAKE_PROJECT_NAME}.bin
        OUTPUT_FILE ${BUILD_DIR}/${CMAKE_PROJECT_NAME}-ota.bin
    )
    # Adding dependecy as app target so that this runs after images are ready
    add_dependencies(chip-ota-image app)
endif()
