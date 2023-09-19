#
#   Copyright (c) 2021 Project CHIP Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.
#

# The compile flags written args.gn come from chip.c and chip.cpp.
# Any build specifications (include dirs, compile flags, definitions, etc)
# that is set in the component will be reflected in
# the output args.gn. By default, all components inherit build-level
# specifications stored as idf-build-properties as done in
# tools/cmake/build.cmake.
#
# Adding REQUIRES/PRIV_REQUIRES will also propagate the
# appropriate build specifications from the required component.
idf_build_get_property(idf_path IDF_PATH)

if(NOT CHIP_ROOT)
    get_filename_component(CHIP_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../../../.. REALPATH)
endif()

include(${CMAKE_CURRENT_LIST_DIR}/ota-image.cmake)

set(CHIP_REQUIRE_COMPONENTS esp_eth freertos lwip bt mbedtls fatfs app_update console openthread nvs_flash spi_flash)

if(NOT "${IDF_TARGET}" STREQUAL "esp32h2")
    list(APPEND CHIP_REQUIRE_COMPONENTS mdns)
endif()

if (NOT CMAKE_BUILD_EARLY_EXPANSION)
    if (CONFIG_COMPILER_OPTIMIZATION_DEFAULT OR CONFIG_COMPILER_OPTIMIZATION_NONE)
        set(is_debug TRUE)
    else()
        if (NOT CONFIG_COMPILER_OPTIMIZATION_ASSERTIONS_DISABLE)
            message(FATAL_ERROR "CONFIG_COMPILER_OPTIMIZATION_ASSERTIONS_DISABLE shall be set")
        endif()
        set(is_debug FALSE)
    endif()
endif()

set(SRCS_LIST
                .
                ${MATTER_SDK_PATH}/src/app/AttributeAccessInterface.cpp
                ${MATTER_SDK_PATH}/src/app/AttributePathExpandIterator.cpp
                ${MATTER_SDK_PATH}/src/app/BufferedReadCallback.cpp
                ${MATTER_SDK_PATH}/src/app/CASEClient.cpp
                ${MATTER_SDK_PATH}/src/app/CASESessionManager.cpp
                ${MATTER_SDK_PATH}/src/app/ChunkedWriteCallback.cpp
                ${MATTER_SDK_PATH}/src/app/ClusterStateCache.cpp
                ${MATTER_SDK_PATH}/src/app/CommandHandler.cpp
                ${MATTER_SDK_PATH}/src/app/CommandSender.cpp
                ${MATTER_SDK_PATH}/src/app/DefaultAttributePersistenceProvider.cpp
                ${MATTER_SDK_PATH}/src/app/DeferredAttributePersistenceProvider.cpp
                ${MATTER_SDK_PATH}/src/app/DeviceProxy.cpp
                ${MATTER_SDK_PATH}/src/app/EventManagement.cpp
                ${MATTER_SDK_PATH}/src/app/FailSafeContext.cpp
                ${MATTER_SDK_PATH}/src/app/InteractionModelEngine.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ArrayBuilder.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ArrayParser.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributeDataIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributeDataIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributePathIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributePathIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributeReportIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributeReportIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributeStatusIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/AttributeStatusIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/Builder.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ClusterPathIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/CommandDataIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/CommandPathIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/CommandStatusIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/DataVersionFilterIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/DataVersionFilterIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventDataIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventFilterIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventFilterIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventPathIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventPathIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventReportIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventReportIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/EventStatusIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/InvokeRequestMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/InvokeRequests.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/InvokeResponseIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/InvokeResponseIBs.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/InvokeResponseMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ListBuilder.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ListParser.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/MessageBuilder.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/MessageDefHelper.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/MessageParser.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/Parser.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ReadRequestMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/ReportDataMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/StatusIB.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/StatusResponseMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/StructBuilder.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/StructParser.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/SubscribeRequestMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/SubscribeResponseMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/TimedRequestMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/WriteRequestMessage.cpp
                ${MATTER_SDK_PATH}/src/app/MessageDef/WriteResponseMessage.cpp
                ${MATTER_SDK_PATH}/src/app/OperationalSessionSetup.cpp
                ${MATTER_SDK_PATH}/src/app/ReadClient.cpp
                ${MATTER_SDK_PATH}/src/app/ReadHandler.cpp
                ${MATTER_SDK_PATH}/src/app/RequiredPrivilege.cpp
                ${MATTER_SDK_PATH}/src/app/StatusResponse.cpp
                ${MATTER_SDK_PATH}/src/app/TimedHandler.cpp
                ${MATTER_SDK_PATH}/src/app/TimedRequest.cpp
                ${MATTER_SDK_PATH}/src/app/WriteClient.cpp
                ${MATTER_SDK_PATH}/src/app/WriteHandler.cpp
                ${MATTER_SDK_PATH}/src/app/reporting/Engine.cpp
                ${MATTER_SDK_PATH}/src/app/reporting/ReportSchedulerImpl.cpp
                ${MATTER_SDK_PATH}/src/app/reporting/SynchronizedReportSchedulerImpl.cpp
                ${MATTER_SDK_PATH}/src/app/SimpleSubscriptionResumptionStorage.cpp
                ${MATTER_SDK_PATH}/src/ble/BLEEndPoint.cpp
                ${MATTER_SDK_PATH}/src/ble/BleError.cpp
                ${MATTER_SDK_PATH}/src/ble/BleLayer.cpp
                ${MATTER_SDK_PATH}/src/ble/BleUUID.cpp
                ${MATTER_SDK_PATH}/src/ble/BtpEngine.cpp
                ${MATTER_SDK_PATH}/src/crypto/CHIPCryptoPAL.cpp
                ${MATTER_SDK_PATH}/src/crypto/PersistentStorageOperationalKeystore.cpp
                ${MATTER_SDK_PATH}/src/crypto/RandUtils.cpp
                ${MATTER_SDK_PATH}/src/crypto/RawKeySessionKeystore.cpp
                ${MATTER_SDK_PATH}/src/inet/IPAddress-StringFuncts.cpp
                ${MATTER_SDK_PATH}/src/inet/IPAddress.cpp
                ${MATTER_SDK_PATH}/src/inet/IPPacketInfo.cpp
                ${MATTER_SDK_PATH}/src/inet/IPPrefix.cpp
                ${MATTER_SDK_PATH}/src/inet/InetArgParser.cpp
                ${MATTER_SDK_PATH}/src/inet/InetError.cpp
                ${MATTER_SDK_PATH}/src/inet/InetInterface.cpp
                ${MATTER_SDK_PATH}/src/inet/TCPEndPoint.cpp
                ${MATTER_SDK_PATH}/src/inet/TCPEndPointImplLwIP.cpp
                ${MATTER_SDK_PATH}/src/inet/UDPEndPoint.cpp
                ${MATTER_SDK_PATH}/src/inet/UDPEndPointImplLwIP.cpp
                ${MATTER_SDK_PATH}/src/lib/asn1/ASN1Error.cpp
                ${MATTER_SDK_PATH}/src/lib/asn1/ASN1OID.cpp
                ${MATTER_SDK_PATH}/src/lib/asn1/ASN1Reader.cpp
                ${MATTER_SDK_PATH}/src/lib/asn1/ASN1Time.cpp
                ${MATTER_SDK_PATH}/src/lib/asn1/ASN1Writer.cpp
                ${MATTER_SDK_PATH}/src/lib/core/CHIPError.cpp
                ${MATTER_SDK_PATH}/src/lib/core/CHIPKeyIds.cpp
                ${MATTER_SDK_PATH}/src/lib/core/OTAImageHeader.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVCircularBuffer.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVDebug.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVReader.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVTags.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVUpdater.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVUtilities.cpp
                ${MATTER_SDK_PATH}/src/lib/core/TLVWriter.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/IPAddressSorter.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/ServiceNaming.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/TxtFields.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/ActiveResolveAttempts.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/Advertiser_ImplMinimalMdns.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/IncrementalResolve.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/MinimalMdnsServer.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/Resolver_ImplMinimalMdns.cpp
                ${MATTER_SDK_PATH}/src/lib/support/Base64.cpp
                ${MATTER_SDK_PATH}/src/lib/support/BufferReader.cpp
                ${MATTER_SDK_PATH}/src/lib/support/BufferWriter.cpp
                ${MATTER_SDK_PATH}/src/lib/support/BytesCircularBuffer.cpp
                ${MATTER_SDK_PATH}/src/lib/support/BytesToHex.cpp
                ${MATTER_SDK_PATH}/src/lib/support/CHIPArgParser.cpp
                ${MATTER_SDK_PATH}/src/lib/support/CHIPMem.cpp
                ${MATTER_SDK_PATH}/src/lib/support/CHIPPlatformMemory.cpp
                ${MATTER_SDK_PATH}/src/lib/support/ErrorStr.cpp
                ${MATTER_SDK_PATH}/src/lib/support/FibonacciUtils.cpp
                ${MATTER_SDK_PATH}/src/lib/support/FixedBufferAllocator.cpp
                ${MATTER_SDK_PATH}/src/lib/support/IniEscaping.cpp
                ${MATTER_SDK_PATH}/src/lib/support/PersistentStorageAudit.cpp
                ${MATTER_SDK_PATH}/src/lib/support/Pool.cpp
                ${MATTER_SDK_PATH}/src/lib/support/PrivateHeap.cpp
                ${MATTER_SDK_PATH}/src/lib/support/SerializableIntegerSet.cpp
                ${MATTER_SDK_PATH}/src/lib/support/StringBuilder.cpp
                ${MATTER_SDK_PATH}/src/lib/support/ThreadOperationalDataset.cpp
                ${MATTER_SDK_PATH}/src/lib/support/TimeUtils.cpp
                ${MATTER_SDK_PATH}/src/lib/support/ZclString.cpp
                ${MATTER_SDK_PATH}/src/lib/support/logging/CHIPLogging.cpp
                ${MATTER_SDK_PATH}/src/lib/support/verhoeff/Verhoeff.cpp
                ${MATTER_SDK_PATH}/src/lib/support/verhoeff/Verhoeff10.cpp
                ${MATTER_SDK_PATH}/src/lib/support/CHIPMem-Malloc.cpp
                ${MATTER_SDK_PATH}/src/messaging/ApplicationExchangeDispatch.cpp
                ${MATTER_SDK_PATH}/src/messaging/ErrorCategory.cpp
                ${MATTER_SDK_PATH}/src/messaging/ExchangeContext.cpp
                ${MATTER_SDK_PATH}/src/messaging/ExchangeMessageDispatch.cpp
                ${MATTER_SDK_PATH}/src/messaging/ExchangeMgr.cpp
                ${MATTER_SDK_PATH}/src/messaging/ReliableMessageContext.cpp
                ${MATTER_SDK_PATH}/src/messaging/ReliableMessageMgr.cpp
                ${MATTER_SDK_PATH}/src/messaging/ReliableMessageProtocolConfig.cpp
                ${MATTER_SDK_PATH}/src/platform/CommissionableDataProvider.cpp
                ${MATTER_SDK_PATH}/src/platform/DeviceControlServer.cpp
                ${MATTER_SDK_PATH}/src/platform/DeviceInfoProvider.cpp
                ${MATTER_SDK_PATH}/src/platform/DeviceInstanceInfoProvider.cpp
                ${MATTER_SDK_PATH}/src/platform/DiagnosticDataProvider.cpp
                ${MATTER_SDK_PATH}/src/platform/Entropy.cpp
                ${MATTER_SDK_PATH}/src/platform/GeneralUtils.cpp
                ${MATTER_SDK_PATH}/src/platform/Globals.cpp
                ${MATTER_SDK_PATH}/src/platform/LockTracker.cpp
                ${MATTER_SDK_PATH}/src/platform/PersistedStorage.cpp
                ${MATTER_SDK_PATH}/src/platform/PlatformEventSupport.cpp
                ${MATTER_SDK_PATH}/src/protocols/Protocols.cpp
                ${MATTER_SDK_PATH}/src/protocols/echo/EchoClient.cpp
                ${MATTER_SDK_PATH}/src/protocols/echo/EchoServer.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/MessageCounterManager.cpp
                ${MATTER_SDK_PATH}/src/protocols/user_directed_commissioning/UserDirectedCommissioningClient.cpp
                ${MATTER_SDK_PATH}/src/protocols/user_directed_commissioning/UserDirectedCommissioningServer.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/Base38Decode.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/Base38Encode.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/ManualSetupPayloadGenerator.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/ManualSetupPayloadParser.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/QRCodeSetupPayloadGenerator.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/QRCodeSetupPayloadParser.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/SetupPayload.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/SetupPayloadHelper.cpp
                ${MATTER_SDK_PATH}/src/system/SystemClock.cpp
                ${MATTER_SDK_PATH}/src/system/SystemError.cpp
                ${MATTER_SDK_PATH}/src/system/SystemLayer.cpp
                ${MATTER_SDK_PATH}/src/system/SystemMutex.cpp
                ${MATTER_SDK_PATH}/src/system/SystemPacketBuffer.cpp
                ${MATTER_SDK_PATH}/src/system/SystemStats.cpp
                ${MATTER_SDK_PATH}/src/system/SystemTimer.cpp
                ${MATTER_SDK_PATH}/src/system/TLVPacketBufferBackingStore.cpp
                ${MATTER_SDK_PATH}/src/system/WakeEvent.cpp
                ${MATTER_SDK_PATH}/src/system/SystemLayerImplFreeRTOS.cpp
                ${MATTER_SDK_PATH}/src/transport/CryptoContext.cpp
                ${MATTER_SDK_PATH}/src/transport/GroupPeerMessageCounter.cpp
                ${MATTER_SDK_PATH}/src/transport/SecureMessageCodec.cpp
                ${MATTER_SDK_PATH}/src/transport/SecureSession.cpp
                ${MATTER_SDK_PATH}/src/transport/SecureSessionTable.cpp
                ${MATTER_SDK_PATH}/src/transport/Session.cpp
                ${MATTER_SDK_PATH}/src/transport/SessionHolder.cpp
                ${MATTER_SDK_PATH}/src/transport/SessionManager.cpp
                ${MATTER_SDK_PATH}/src/transport/TransportMgrBase.cpp
                ${MATTER_SDK_PATH}/src/controller/AbstractDnssdDiscoveryController.cpp
                ${MATTER_SDK_PATH}/src/controller/AutoCommissioner.cpp
                ${MATTER_SDK_PATH}/src/controller/CHIPCommissionableNodeController.cpp
                ${MATTER_SDK_PATH}/src/controller/CHIPDeviceController.cpp
                ${MATTER_SDK_PATH}/src/controller/CHIPDeviceControllerFactory.cpp
                ${MATTER_SDK_PATH}/src/controller/CommissioneeDeviceProxy.cpp
                ${MATTER_SDK_PATH}/src/controller/CommissionerDiscoveryController.cpp
                ${MATTER_SDK_PATH}/src/controller/CommissioningDelegate.cpp
                ${MATTER_SDK_PATH}/src/controller/CommissioningWindowOpener.cpp
                ${MATTER_SDK_PATH}/src/controller/CurrentFabricRemover.cpp
                ${MATTER_SDK_PATH}/src/controller/EmptyDataModelHandler.cpp
                ${MATTER_SDK_PATH}/src/controller/ExampleOperationalCredentialsIssuer.cpp
                ${MATTER_SDK_PATH}/src/controller/SetUpCodePairer.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/MainLoopESP32.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/streamer_esp32.cpp
                ${MATTER_SDK_PATH}/src/credentials/attestation_verifier/DacOnlyPartialAttestationVerifier.cpp
                ${MATTER_SDK_PATH}/src/credentials/attestation_verifier/DefaultDeviceAttestationVerifier.cpp
                ${MATTER_SDK_PATH}/src/access/AccessControl.cpp
                ${MATTER_SDK_PATH}/src/access/examples/ExampleAccessControlDelegate.cpp
                ${MATTER_SDK_PATH}/src/access/examples/PermissiveAccessControlDelegate.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/AdditionalDataPayloadGenerator.cpp
                ${MATTER_SDK_PATH}/src/setup_payload/AdditionalDataPayloadParser.cpp
                ${MATTER_SDK_PATH}/src/crypto/CHIPCryptoPALmbedTLS.cpp
                ${MATTER_SDK_PATH}/zzz_generated/app-common/app-common/zap-generated/cluster-objects.cpp
                ${MATTER_SDK_PATH}/src/platform/SingletonConfigurationManager.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/ConfigurationManagerImpl.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/ConnectivityManagerImpl.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/DiagnosticDataProviderImpl.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/ESP32Config.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/ESP32Utils.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/KeyValueStoreManagerImpl.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/Logging.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/LwIPCoreLock.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/PlatformManagerImpl.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/SystemTimeSupport.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/nimble/BLEManagerImpl.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/nimble/ChipDeviceScanner.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/nimble/misc.c
                ${MATTER_SDK_PATH}/src/platform/ESP32/nimble/peer.c
                ${MATTER_SDK_PATH}/src/platform/ESP32/ConnectivityManagerImpl_WiFi.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/NetworkCommissioningDriver.cpp
                ${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteHook.c
                ${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteTable.c
                ${MATTER_SDK_PATH}/src/app/icd/IcdManagementServer.cpp
                ${MATTER_SDK_PATH}/src/app/icd/IcdMonitoringTable.cpp
                ${MATTER_SDK_PATH}/src/protocols/interaction_model/StatusCode.cpp
                ${MATTER_SDK_PATH}/src/lib/address_resolve/AddressResolve.cpp
                ${MATTER_SDK_PATH}/src/lib/address_resolve/AddressResolve_DefaultImpl.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/Parser.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/RecordData.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/ResponseSender.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/Server.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/AddressPolicy.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core/QName.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core/QNameString.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/core/RecordWriter.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/responders/IP.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/responders/QueryResponder.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/records/IP.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/records/ResourceRecord.cpp
                ${MATTER_SDK_PATH}/src/lib/dnssd/minimal_mdns/AddressPolicy_DefaultImpl.cpp
                ${MATTER_SDK_PATH}/src/tracing/registry.cpp
                ${MATTER_SDK_PATH}/src/credentials/CHIPCert.cpp
                ${MATTER_SDK_PATH}/src/credentials/CHIPCertFromX509.cpp
                ${MATTER_SDK_PATH}/src/credentials/CHIPCertToX509.cpp
                ${MATTER_SDK_PATH}/src/credentials/CertificationDeclaration.cpp
                ${MATTER_SDK_PATH}/src/credentials/DeviceAttestationConstructor.cpp
                ${MATTER_SDK_PATH}/src/credentials/DeviceAttestationCredsProvider.cpp
                ${MATTER_SDK_PATH}/src/credentials/FabricTable.cpp
                ${MATTER_SDK_PATH}/src/credentials/GenerateChipX509Cert.cpp
                ${MATTER_SDK_PATH}/src/credentials/GroupDataProviderImpl.cpp
                ${MATTER_SDK_PATH}/src/credentials/LastKnownGoodTime.cpp
                ${MATTER_SDK_PATH}/src/credentials/PersistentStorageOpCertStore.cpp
                ${MATTER_SDK_PATH}/src/credentials/attestation_verifier/DeviceAttestationVerifier.cpp
                ${MATTER_SDK_PATH}/src/credentials/examples/DeviceAttestationCredsExample.cpp
                ${MATTER_SDK_PATH}/src/credentials/examples/ExampleDACs.cpp
                ${MATTER_SDK_PATH}/src/credentials/examples/ExamplePAI.cpp
                ${MATTER_SDK_PATH}/src/credentials/tests/CHIPAttCert_test_vectors.cpp
                ${MATTER_SDK_PATH}/src/transport/raw/MessageHeader.cpp
                ${MATTER_SDK_PATH}/src/transport/raw/TCP.cpp
                ${MATTER_SDK_PATH}/src/transport/raw/UDP.cpp
                ${MATTER_SDK_PATH}/src/transport/raw/BLE.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/CASEDestinationId.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/CASEServer.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/CASESession.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/DefaultSessionResumptionStorage.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/PASESession.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/PairingSession.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/SessionEstablishmentExchangeDispatch.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/SimpleSessionResumptionStorage.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/StatusReport.cpp
                ${MATTER_SDK_PATH}/src/protocols/secure_channel/UnsolicitedStatusHandler.cpp
                ${MATTER_SDK_PATH}/src/protocols/bdx/BdxMessages.cpp
                ${MATTER_SDK_PATH}/src/protocols/bdx/BdxTransferSession.cpp
                ${MATTER_SDK_PATH}/src/protocols/bdx/BdxUri.cpp
                ${MATTER_SDK_PATH}/src/protocols/bdx/TransferFacilitator.cpp
                ${MATTER_SDK_PATH}/src/app/server/AclStorage.cpp
                ${MATTER_SDK_PATH}/src/app/server/CommissioningWindowManager.cpp
                ${MATTER_SDK_PATH}/src/app/server/DefaultAclStorage.cpp
                ${MATTER_SDK_PATH}/src/app/server/Dnssd.cpp
                ${MATTER_SDK_PATH}/src/app/server/EchoHandler.cpp
                ${MATTER_SDK_PATH}/src/app/server/OnboardingCodesUtil.cpp
                ${MATTER_SDK_PATH}/src/app/server/Server.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/Engine.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/streamer.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Base64.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Help.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Meta.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Config.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Device.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/OnboardingCodes.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/WiFi.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/BLE.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Dns.cpp
                ${MATTER_SDK_PATH}/src/lib/shell/commands/Ota.cpp
                                
                # "${MATTER_SDK_PATH}/zzz_generated/app-common"
                # "${MATTER_SDK_PATH}/third_party/nlfaultinjection/repo/include"
                # "${MATTER_SDK_PATH}/src"
)

set(SRC_DIRS_LIST
                    "."
                    # "${MATTER_SDK_PATH}/third_party/nlfaultinjection/repo/src"
                    # "${MATTER_SDK_PATH}/src/lib/core"
                    # "${MATTER_SDK_PATH}/src/credentials"
                    # "${MATTER_SDK_PATH}/src/access/examples"
                    # "${MATTER_SDK_PATH}/src/messaging"
                    # "${MATTER_SDK_PATH}/src/transport"
                    # "${MATTER_SDK_PATH}/src/tracing"
                    # "${MATTER_SDK_PATH}/src/platform/ESP32"
                    # "${MATTER_SDK_PATH}/src/platform"
                    # "${MATTER_SDK_PATH}/src/lib/dnssd"
                    # "${MATTER_SDK_PATH}/src/setup_payload"
                    # "${MATTER_SDK_PATH}/src/crypto"
                    # # "${MATTER_SDK_PATH}/src/ble"
                    # # "${MATTER_SDK_PATH}/src/inet"
                    # "${MATTER_SDK_PATH}/src/lib/core"
                    # "${MATTER_SDK_PATH}/src/lib/support/logging"
                    # "${MATTER_SDK_PATH}/src/lib/support"
                    # "${MATTER_SDK_PATH}/src/system"
                    # "${MATTER_SDK_PATH}/src/platform/ESP32/nimble"
                    # # "${MATTER_SDK_PATH}/src/platform/ESP32/bluedroid"
                    # "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook"
                    # "${MATTER_SDK_PATH}/src/platform/OpenThread/"
                    "${MATTER_SDK_PATH}/src/app"
                    "${MATTER_SDK_PATH}/src/ble"
                    "${MATTER_SDK_PATH}/src/crypto"

)

set(INCLUDE_DIRS_LIST
                        "."
                        "${MATTER_SDK_PATH}/zzz_generated/app-common"
                        "${MATTER_SDK_PATH}/third_party/nlfaultinjection/repo/include"
                        "${MATTER_SDK_PATH}/out/host/gen/include"
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
)

set(EXCLUDE_SRCS_LIST "${MATTER_SDK_PATH}/src/platform/ESP32/ConnectivityManagerImpl_Ethernet.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/NetworkCommissioningDriver_Ethernet.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ConnectivityManagerImpl_WiFi.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/WiFiDnssdImpl.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/WiFiDnssdImpl.h"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/NetworkCommissioningDriver.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/NetworkCommissioningDriver.h"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/ESP32CHIPCryptoPAL.cpp"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteHook.c"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteHook.h"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteTable.c"
                      "${MATTER_SDK_PATH}/src/platform/ESP32/route_hook/ESP32RouteTable.h"
                      "${MATTER_SDK_PATH}/src/platform/OpenThread/GenericThreadStackManagerImpl_OpenThread_LwIP.cpp"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/Advertiser_ImplMinimalMdns.cpp"
                      "${MATTER_SDK_PATH}/src/lib/dnssd/Advertiser_ImplNone.cpp"
                      # "${MATTER_SDK_PATH}/src/platform/OpenThreadDnssdImpl.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/JniReferences.cpp"
                      "${MATTER_SDK_PATH}/src/lib/support/CHIPFaultInjection.cpp"
                      )

set(PTIV_INCLUDE_DIRS_LIST "${MATTER_SDK_PATH}/src")
set(REQUIRES_LIST )

# list(APPEND INCLUDE_DIRS_LIST "zap_common")

# SRCS                 ${SRCS_LIST}
# PRIV_INCLUDE_DIRS    ${PTIV_INCLUDE_DIRS_LIST}

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/app/AppBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/app/AppBuildConfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/asn1/ASN1OID.h.in
                ${CMAKE_CURRENT_LIST_DIR}/asn1/ASN1OID.h)
        
configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/ble/BleBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/ble/BleBuildConfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/core/CHIPBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/core/CHIPBuildConfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/crypto/CryptoBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/crypto/CryptoBuildConfig.h)
                    
configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/inet/InetBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/inet/InetBuildConfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/lwip/lwip_buildconfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/lwip/lwip_buildconfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/platform/CHIPDeviceBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/platform/CHIPDeviceBuildConfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/setup_payload/CHIPAdditionalDataPayloadBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/setup_payload/CHIPAdditionalDataPayloadBuildConfig.h)
                    
configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/system/SystemBuildConfig.h.in
                ${CMAKE_CURRENT_LIST_DIR}/system/SystemBuildConfig.h)

configure_file(
                ${CMAKE_CURRENT_LIST_DIR}/matter/tracing/build_config.h.in
                ${CMAKE_CURRENT_LIST_DIR}/matter/tracing/build_config.h)


list(APPEND INCLUDE_DIRS_LIST "${MATTER_SDK_PATH}/config/esp32/components/chip/")
list(APPEND INCLUDE_DIRS_LIST "${MATTER_SDK_PATH}/third_party/nlassert/repo/include"
                              "${MATTER_SDK_PATH}/third_party/nlio/repo/include"
                              "${MATTER_SDK_PATH}/third_party/boringssl/repo/src/include") 

idf_component_register(
                       SRCS                 ${SRCS_LIST}
                    #    SRC_DIRS             ${SRC_DIRS_LIST}
                    #   EXCLUDE_SRCS         ${EXCLUDE_SRCS_LIST}
                       INCLUDE_DIRS         ${INCLUDE_DIRS_LIST}
                       REQUIRES             ${CHIP_REQUIRE_COMPONENTS})
                    #    PRIV_REQUIRES        ${CHIP_REQUIRE_COMPONENTS}
                    #    REQUIRES             ${REQUIRES_LIST})

add_definitions(
                "-DCHIP_ADDRESS_RESOLVE_IMPL_INCLUDE_HEADER=<lib/address_resolve/AddressResolve_DefaultImpl.h>"
                "-DCHIP_CONFIG_SOFTWARE_VERSION_NUMBER=0"
                "-DCHIP_MINMDNS_DEFAULT_POLICY=1")


# Prepare initial args file (lacking compile flags)
# This will be saved as args.gn.in
set(chip_gn_args "import(\"//args.gni\")\n")

macro(chip_gn_arg_append arg val)
    string(APPEND chip_gn_args "${arg} = ${val}\n")
endmacro()

macro(chip_gn_arg_bool arg boolean)
    if (${boolean})
        string(APPEND chip_gn_args "${arg} = true\n")
    else()
        string(APPEND chip_gn_args "${arg} = false\n")
    endif()
endmacro()

# ESP-IDF lets user set software version string by two ways:
# 1. Project's CMakeLists.txt file and 2. Config option
# It depends on CONFIG_APP_PROJECT_VER_FROM_CONFIG option
# So, below makes the same provision for software version number
if (CONFIG_APP_PROJECT_VER_FROM_CONFIG)
    chip_gn_arg_append("chip_config_software_version_number" ${CONFIG_DEVICE_SOFTWARE_VERSION_NUMBER})
elseif (DEFINED PROJECT_VER_NUMBER)
    chip_gn_arg_append("chip_config_software_version_number" ${PROJECT_VER_NUMBER})
else()
    chip_gn_arg_append("chip_config_software_version_number" 0)
endif()

chip_gn_arg_append("esp32_ar"              "\"${CMAKE_AR}\"")
chip_gn_arg_append("esp32_cc"              "\"${CMAKE_C_COMPILER}\"")
chip_gn_arg_append("esp32_cxx"             "\"${CMAKE_CXX_COMPILER}\"")
chip_gn_arg_append("esp32_cpu"             "\"esp32\"")
chip_gn_arg_bool("is_debug"                ${is_debug})

if (CONFIG_CHIP_CONFIG_IM_PRETTY_PRINT)
    chip_gn_arg_bool("enable_im_pretty_print" "true")
endif()

# Config the chip log level by IDF menuconfig
if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 1)
    chip_gn_arg_bool  ("chip_error_logging" "true")
else()
    chip_gn_arg_bool  ("chip_error_logging" "false")
endif()

if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 3)
    chip_gn_arg_bool  ("chip_progress_logging" "true")
else()
    chip_gn_arg_bool  ("chip_progress_logging" "false")
endif()

if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 4)
    chip_gn_arg_bool  ("chip_detail_logging" "true")
else()
    chip_gn_arg_bool  ("chip_detail_logging" "false")
endif()

if (CONFIG_LOG_DEFAULT_LEVEL GREATER_EQUAL 5)
    chip_gn_arg_bool  ("chip_automation_logging" "true")
else()
    chip_gn_arg_bool  ("chip_automation_logging" "false")
endif()

if(CONFIG_ENABLE_CHIPOBLE)
chip_gn_arg_append("chip_config_network_layer_ble"           "true")
else()
chip_gn_arg_append("chip_config_network_layer_ble"           "false")
endif()

if(CONFIG_DISABLE_IPV4)
    chip_gn_arg_append("chip_inet_config_enable_ipv4"        "false")
endif()

if(CONFIG_DISABLE_READ_CLIENT)
    chip_gn_arg_append("chip_enable_read_client"             "false")
endif()

if(CHIP_CODEGEN_PREGEN_DIR)
    chip_gn_arg_append("chip_code_pre_generated_directory"  "\"${CHIP_CODEGEN_PREGEN_DIR}\"")
endif()

if(CONFIG_ENABLE_ICD_SERVER)
    chip_gn_arg_append("chip_enable_icd_server"               "true")
endif()

if(CONFIG_ENABLE_PW_RPC)
    string(APPEND chip_gn_args "import(\"//build_overrides/pigweed.gni\")\n")
    chip_gn_arg_append("remove_default_configs"             "[\"//third_party/connectedhomeip/third_party/pigweed/repo/pw_build:toolchain_cpp_standard\"]")
    chip_gn_arg_append("chip_build_pw_rpc_lib"              "true")
    chip_gn_arg_append("pw_log_BACKEND"                     "\"//third_party/connectedhomeip/third_party/pigweed/repo/pw_log_basic\"")
    chip_gn_arg_append("pw_assert_BACKEND"                  "\"//third_party/connectedhomeip/third_party/pigweed/repo/pw_assert_log:check_backend\"")
    chip_gn_arg_append("pw_sys_io_BACKEND"                  "\"//third_party/connectedhomeip/examples/platform/esp32/pw_sys_io:pw_sys_io_esp32\"")
    chip_gn_arg_append("dir_pw_third_party_nanopb"          "\"//third_party/connectedhomeip/third_party/nanopb/repo\"")
    chip_gn_arg_append("pw_build_LINK_DEPS"                 "[\"\$dir_pw_assert:impl\", \"\$dir_pw_log:impl\"]")
    chip_gn_arg_append("pw_rpc_CONFIG"                      "\"//third_party/connectedhomeip/third_party/pigweed/repo/pw_rpc:disable_global_mutex\"")
endif()

if (CONFIG_BUILD_CHIP_TESTS)
    chip_gn_arg_bool("chip_build_tests"     "true")
endif()

if (CONFIG_IM_PRETTY_PRINT)
    chip_gn_arg_bool("enable_im_pretty_print"	"true")
endif()

if (NOT CONFIG_USE_MINIMAL_MDNS)
    chip_gn_arg_append("chip_mdns"                          "\"platform\"")
else()
    chip_gn_arg_append("chip_mdns"                          "\"minimal\"")
endif()

if (CONFIG_ENABLE_CHIP_SHELL)
    chip_gn_arg_append("chip_build_libshell"                "true")
endif()

if (CONFIG_ENABLE_CHIP_CONTROLLER_BUILD)
    chip_gn_arg_append("chip_build_controller"              "true")
endif()

if (CONFIG_ENABLE_WIFI_STATION OR CONFIG_ENABLE_WIFI_AP)
    chip_gn_arg_append("chip_enable_wifi"                       "true")
else()
    chip_gn_arg_append("chip_enable_wifi"                       "false")
endif()

if (CONFIG_ENABLE_CHIPOBLE)
    chip_gn_arg_append("chip_enable_chipoble"                "true")
endif()

if ((CONFIG_BT_ENABLED) AND (CONFIG_ENABLE_CHIPOBLE))
	if (CONFIG_BT_NIMBLE_ENABLED)
          chip_gn_arg_append("chip_bt_nimble_enabled"                "true")
        else()
          chip_gn_arg_append("chip_bt_bluedroid_enabled"                "true")
        endif()
endif()

if (CONFIG_ENABLE_ESP32_BLE_CONTROLLER)
    chip_gn_arg_append("chip_enable_ble_controller"             "true")
endif()

if (CONFIG_ENABLE_ETHERNET_TELEMETRY)
    chip_gn_arg_append("chip_enable_ethernet"                   "true")
endif()

if (CONFIG_ENABLE_MATTER_OVER_THREAD)
    chip_gn_arg_append("chip_enable_openthread"                 "true")
else()
    chip_gn_arg_append("chip_enable_openthread"                 "false")
endif()

if (CONFIG_OPENTHREAD_FTD)
    chip_gn_arg_append("chip_openthread_ftd"                    "true")
else()
    chip_gn_arg_append("chip_openthread_ftd"                    "false")
endif()

if (CONFIG_ENABLE_OTA_REQUESTOR)
    chip_gn_arg_append("chip_enable_ota_requestor"                 "true")
endif()

if (CONFIG_ENABLE_ROTATING_DEVICE_ID)
    chip_gn_arg_append("chip_enable_additional_data_advertising"   "true")
    chip_gn_arg_append("chip_enable_rotating_device_id"            "true")
endif()

if (CONFIG_ENABLE_ROUTE_HOOK)
    chip_gn_arg_append("chip_enable_route_hook"   "true")
endif()

if (CONFIG_CHIP_ENABLE_EXTERNAL_PLATFORM)
    chip_gn_arg_append("chip_device_platform"   "\"external\"")
    if (CONFIG_ENABLE_CHIP_SHELL)
        chip_gn_arg_append("chip_shell_platform"    "\"esp32\"")
    endif()
    chip_gn_arg_append("chip_platform_target"   "\"//${CONFIG_CHIP_EXTERNAL_PLATFORM_DIR}\"")
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

if (CHIP_PROJECT_CONFIG)
    chip_gn_arg_append("chip_project_config_include" "\"${CHIP_PROJECT_CONFIG}\"")
    chip_gn_arg_append("chip_system_project_config_include" "\"${CHIP_PROJECT_CONFIG}\"")
endif()

if (CONFIG_ENABLE_ESP32_FACTORY_DATA_PROVIDER)
    chip_gn_arg_append("chip_use_transitional_commissionable_data_provider" "false")
    chip_gn_arg_append("chip_use_factory_data_provider"                     "true")
endif()

if (CONFIG_ENABLE_ESP32_DEVICE_INFO_PROVIDER)
    chip_gn_arg_append("chip_use_device_info_provider"                     "true")
endif()

if (CONFIG_SEC_CERT_DAC_PROVIDER)
    chip_gn_arg_append("chip_use_secure_cert_dac_provider"                 "true")
endif()

if (CONFIG_ENABLE_ESP_INSIGHTS_TRACE)
    chip_gn_arg_append("matter_enable_esp_insights_trace"                    "true")
endif()

if (CONFIG_USE_ESP32_ECDSA_PERIPHERAL)
    chip_gn_arg_append("chip_use_esp32_ecdsa_peripheral" "true")
endif()

set(args_gn_input "${CMAKE_CURRENT_BINARY_DIR}/args.gn.in")
file(GENERATE OUTPUT "${args_gn_input}" CONTENT "${chip_gn_args}")

# This generates the final args.in file to be fed to
# CHIP build using GN build system. The necessary compile_commands.json
# and args.gn.in input file should exist by the time this is invoked,
# since these requirements ar generated at the parent project's
# GENERATE phase.
idf_build_get_property(idf_ver IDF_VER)
set(filter_out "-DHAVE_CONFIG_H" "-DIDF_VER=\\\"${idf_ver}\\\"" )

# CMake adds config include dir relative to build directory.
# Make sure full path is given to CHIP build.
list(APPEND filter_out "-Iconfig")
idf_build_get_property(config_dir CONFIG_DIR)
target_compile_options(${COMPONENT_LIB} PRIVATE "-I${config_dir}")

if(filter_out)
    set(filter_arg "--filter-out=${filter_out}")
endif()

set(args_gn "${CMAKE_CURRENT_BINARY_DIR}/args.gn")
# add_custom_command(OUTPUT "${args_gn}"
#                    COMMAND ${python} 
#                            ${CMAKE_CURRENT_LIST_DIR}/create_args_gn.py 
#                            "${CMAKE_BINARY_DIR}"
#                            "${idf_path}"
#                            "${CMAKE_CURRENT_LIST_DIR}/chip.c"
#                            "${CMAKE_CURRENT_LIST_DIR}/chip.cpp"
#                            "${args_gn_input}"
#                            "${args_gn}"
#                            "${filter_arg}"
#                    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
#                    DEPENDS "${CMAKE_BINARY_DIR}/compile_commands.json"
#                    VERBATIM)

# add_custom_target(args_gn DEPENDS "${args_gn}")

# CHIP build as an external project.
find_program(GN_EXECUTABLE gn)
if (${GN_EXECUTABLE} STREQUAL GN_EXECUTABLE-NOTFOUND)
    message(FATAL_ERROR "The 'gn' command was not found. Make sure you have GN installed."
                        "Or have followed necessary build preparations stated in BUILDING.md.")
endif()

set(GN_ROOT_TARGET ${CHIP_ROOT}/config/esp32)

# set(chip_libraries "${CMAKE_CURRENT_BINARY_DIR}/lib/libCHIP.a")
set(chip_libraries )

if(CONFIG_ENABLE_PW_RPC)
    # list(APPEND chip_libraries "${CMAKE_CURRENT_BINARY_DIR}/lib/libPwRpc.a")
endif()

# When using the pregenerated files, there is a edge case where an error appears for
# undeclared argument chip_code_pre_generated_directory. To get around with it we are
# disabling the --fail-on-unused-args flag.
# For more, see: https://github.com/project-chip/connectedhomeip/issues/27636
if (CHIP_CODEGEN_PREGEN_DIR)
    set(GN_CONFIGURE_COMMAND ${GN_EXECUTABLE} --root=${GN_ROOT_TARGET} gen --check ${CMAKE_CURRENT_BINARY_DIR})
else ()
    set(GN_CONFIGURE_COMMAND ${GN_EXECUTABLE} --root=${GN_ROOT_TARGET} gen --check --fail-on-unused-args ${CMAKE_CURRENT_BINARY_DIR})
endif ()

# externalproject_add(
#     chip_gn
#     SOURCE_DIR              ${CHIP_ROOT}
#     BINARY_DIR              ${CMAKE_CURRENT_BINARY_DIR}
#     CONFIGURE_COMMAND       ${GN_CONFIGURE_COMMAND}
#     BUILD_COMMAND           ninja "esp32"
#     INSTALL_COMMAND         ""
#     BUILD_BYPRODUCTS        ${chip_libraries}
#     WORKING_DIRECTORY       ${CMAKE_CURRENT_LIST_DIR}
#     DEPENDS                 args_gn
#     BUILD_ALWAYS            1
# )

idf_component_get_property(freertos_dir freertos COMPONENT_DIR)

# ESP-IDF components usually need 'freertos/<header.h>', while
# CHIP might include do 'header.h'.
# In IDF V5.0, this path has been moved to
# '${freertos_component}/FreeRTOS-Kernel/include/freertos'
target_include_directories(${COMPONENT_LIB} PRIVATE
    "${freertos_dir}/include/freertos"
    "${freertos_dir}/FreeRTOS-Kernel/include/freertos")

target_include_directories(${COMPONENT_LIB} INTERFACE
    "${CHIP_ROOT}/src/platform/ESP32"
    "${CHIP_ROOT}/src/platform/OpenThread"
    "${CHIP_ROOT}/src/include"
    "${CHIP_ROOT}/src/lib"
    "${CHIP_ROOT}/src"
    "${CHIP_ROOT}/zzz_generated/app-common"
    "${CHIP_ROOT}/examples/platform/esp32"
    "${CHIP_ROOT}/third_party/nlassert/repo/include"
    "${CHIP_ROOT}/third_party/nlio/repo/include"
    "${CMAKE_CURRENT_BINARY_DIR}/src/include"
    "${CMAKE_CURRENT_BINARY_DIR}/include"
    "${CMAKE_CURRENT_BINARY_DIR}/gen/include"
    "${CHIP_ROOT}/config/esp32/${CONFIG_CHIP_EXTERNAL_PLATFORM_DIR}"
    "${CHIP_ROOT}/config/esp32/${CONFIG_CHIP_EXTERNAL_PLATFORM_DIR}/../../"
)

if (CONFIG_ENABLE_ESP_INSIGHTS_TRACE)
    target_include_directories(${COMPONENT_LIB} INTERFACE "${CHIP_ROOT}/src/tracing/esp32_trace/include")
endif()

idf_component_get_property(mbedtls_lib mbedtls COMPONENT_LIB)

idf_build_get_property(idf_target IDF_TARGET)
set(target_name "${idf_target}")

if(CONFIG_BT_ENABLED)
    idf_component_get_property(bt_lib bt COMPONENT_LIB)
    if((target_name STREQUAL "esp32h2") OR (target_name STREQUAL "esp32c2") OR (target_name STREQUAL "esp32c6"))
        idf_component_get_property(bt_dir bt COMPONENT_DIR)
        list(APPEND chip_libraries $<TARGET_FILE:${bt_lib}>)
        list(APPEND chip_libraries "${bt_dir}/controller/lib_${target_name}/${target_name}-bt-lib/libble_app.a")
    else()
        list(APPEND chip_libraries $<TARGET_FILE:${bt_lib}> -lbtdm_app)
    endif()
endif()

if (CONFIG_ENABLE_CHIP_SHELL)
    idf_component_get_property(console_lib console COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${console_lib}>)
endif()

if(CONFIG_OPENTHREAD_ENABLED)
    idf_component_get_property(openthread_lib openthread COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${openthread_lib}>)
    if (CONFIG_IEEE802154_ENABLED)
        idf_component_get_property(ieee802154_lib ieee802154 COMPONENT_LIB)
        list(APPEND chip_libraries $<TARGET_FILE:${ieee802154_lib}>)
    endif()
endif()

if((NOT CONFIG_USE_MINIMAL_MDNS) AND (CONFIG_ENABLE_WIFI_STATION OR CONFIG_ENABLE_WIFI_AP))
    idf_build_get_property(build_components BUILD_COMPONENTS)
    # For IDF v5.x, the mdns component was moved to idf_managed_components.
    # We should use 'espressif__mdns' for 'idf_component_get_property'.
    if("espressif__mdns" IN_LIST build_components)
        idf_component_get_property(mdns_lib espressif__mdns COMPONENT_LIB)
    elseif("mdns" IN_LIST build_components)
        idf_component_get_property(mdns_lib mdns COMPONENT_LIB)
    endif()
    list(APPEND chip_libraries $<TARGET_FILE:${mdns_lib}>)
endif()

if (CONFIG_ENABLE_ENCRYPTED_OTA)
    idf_component_get_property(esp_encrypted_img_lib espressif__esp_encrypted_img COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${esp_encrypted_img_lib}>)
endif()

idf_component_get_property(main_lib main COMPONENT_LIB)
list(APPEND chip_libraries $<TARGET_FILE:${main_lib}>)

if (CONFIG_SEC_CERT_DAC_PROVIDER)
    idf_component_get_property(esp32_secure_cert_mgr_lib espressif__esp_secure_cert_mgr COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${esp32_secure_cert_mgr_lib}>)
endif()

if (CONFIG_ENABLE_ESP_INSIGHTS_TRACE)
    idf_component_get_property(esp_insights_lib espressif__esp_insights COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${esp_insights_lib}>)
endif()

idf_component_get_property(lwip_lib lwip COMPONENT_LIB)
list(APPEND chip_libraries $<TARGET_FILE:${lwip_lib}>)


if (CONFIG_ESP32_WIFI_ENABLED)
    idf_component_get_property(esp_wifi_lib esp_wifi COMPONENT_LIB)
    idf_component_get_property(esp_wifi_dir esp_wifi COMPONENT_DIR)
    list(APPEND chip_libraries $<TARGET_FILE:${esp_wifi_lib}>)
    if (CONFIG_IDF_TARGET_ESP32C2)
        set(blobs core net80211 pp)
    else()
        set(blobs core mesh net80211 pp)
    endif()

    foreach(blob ${blobs})
    list(APPEND chip_libraries "${esp_wifi_dir}/lib/${target_name}/lib${blob}.a")
    endforeach()

    idf_component_get_property(wpa_supplicant_lib wpa_supplicant COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${wpa_supplicant_lib}>)
endif()

if (CONFIG_ETH_ENABLED)
    idf_component_get_property(esp_eth_lib esp_eth COMPONENT_LIB)
    list(APPEND chip_libraries $<TARGET_FILE:${esp_eth_lib}>)
endif()

idf_component_get_property(esp_netif_lib esp_netif COMPONENT_LIB)
list(APPEND chip_libraries $<TARGET_FILE:${esp_netif_lib}>)

idf_component_get_property(esp_hw_support_lib esp_hw_support COMPONENT_LIB)
list(APPEND chip_libraries $<TARGET_FILE:${esp_hw_support_lib}>)

idf_component_get_property(esp_phy_lib esp_phy COMPONENT_LIB)
idf_component_get_property(esp_phy_dir esp_phy COMPONENT_DIR)
list(APPEND chip_libraries $<TARGET_FILE:${esp_phy_lib}>)

if (CONFIG_IDF_TARGET_ESP32)
    set(phy_blobs phy rtc)
elseif (CONFIG_IDF_TARGET_ESP32S2)
    set(phy_blobs phy)
else()
    set(phy_blobs phy btbb)
endif()
foreach(phy_blob ${phy_blobs})
    list(APPEND chip_libraries "${esp_phy_dir}/lib/${target_name}/lib${phy_blob}.a")
endforeach()

set(components_to_link esp_event hal esp_system soc efuse vfs driver esp_coex freertos)
idf_build_get_property(build_components BUILD_COMPONENTS)
foreach(component ${components_to_link})
    # Some of the components are not present in IDF v4.x
    # So, Check if the component is in the list of build components
    if("${component}" IN_LIST build_components)
        idf_component_get_property(lib_name ${component} COMPONENT_LIB)
        list(APPEND chip_libraries $<TARGET_FILE:${lib_name}>)
    endif()
endforeach()

target_link_libraries(${COMPONENT_LIB} INTERFACE -Wl,--start-group
                                                ${chip_libraries}
                                                $<TARGET_FILE:mbedcrypto> $<TARGET_FILE:mbedx509>
                                                $<TARGET_FILE:${mbedtls_lib}>
                                                -Wl,--end-group)

# Make the component dependent on our CHIP build
# add_dependencies(${COMPONENT_LIB} chip_gn)

if(CONFIG_ENABLE_PW_RPC)
    set(WRAP_FUNCTIONS esp_log_write esp_log_writev)
    foreach(func ${WRAP_FUNCTIONS})
        target_link_libraries(${COMPONENT_LIB} INTERFACE "-Wl,--wrap=${func}")
    endforeach()
endif()

# Build Matter OTA image
if (CONFIG_CHIP_OTA_IMAGE_BUILD)
    chip_ota_image(chip-ota-image
        INPUT_FILES ${BUILD_DIR}/${CMAKE_PROJECT_NAME}.bin
        OUTPUT_FILE ${BUILD_DIR}/${CMAKE_PROJECT_NAME}-ota.bin
    )
    # Adding dependecy as app target so that this runs after images are ready
    add_dependencies(chip-ota-image app)
endif()
