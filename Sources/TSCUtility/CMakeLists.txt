# This source file is part of the Swift.org open source project
#
# Copyright (c) 2014 - 2020 Apple Inc. and the Swift project authors
# Licensed under Apache License v2.0 with Runtime Library Exception
#
# See http://swift.org/LICENSE.txt for license information
# See http://swift.org/CONTRIBUTORS.txt for Swift project authors

add_library(TSCUtility
  Archiver.swift
  ArgumentParser.swift
  ArgumentParserShellCompletion.swift
  Bits.swift
  Bitstream.swift
  BitstreamReader.swift
  BitstreamWriter.swift
  BuildFlags.swift
  CollectionExtensions.swift
  Diagnostics.swift
  FSWatch.swift
  FloatingPointExtensions.swift
  Git.swift
  IndexStore.swift
  InterruptHandler.swift
  JSONMessageStreamingParser.swift
  misc.swift
  Netrc.swift
  OSLog.swift
  PkgConfig.swift
  Platform.swift
  PolymorphicCodable.swift
  ProgressAnimation.swift
  SerializedDiagnostics.swift
  SimplePersistence.swift
  StringExtensions.swift
  StringMangling.swift
  Triple.swift
  URL.swift
  Verbosity.swift
  Version.swift
  Versioning.swift
  dlopen.swift
  misc.swift
)
target_link_libraries(TSCUtility PUBLIC
  TSCBasic)
target_link_libraries(TSCUtility PRIVATE
  TSCclibc
  ${CMAKE_DL_LIBS}
  Threads::Threads)

if(NOT CMAKE_SYSTEM_NAME STREQUAL Darwin)
  if(CMAKE_SYSTEM_NAME STREQUAL OpenBSD OR CMAKE_SYSTEM_NAME STREQUAL FreeBSD)
    target_link_directories(TSCUtility PRIVATE /usr/local/lib)
  endif()
  if(Foundation_FOUND)
    target_link_libraries(TSCUtility PUBLIC
      FoundationNetworking)
  endif()
endif()
# NOTE(compnerd) workaround for CMake not setting up include flags yet
set_target_properties(TSCUtility PROPERTIES
  INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_Swift_MODULE_DIRECTORY})

install(TARGETS TSCUtility
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)

set_property(GLOBAL APPEND PROPERTY TSC_EXPORTS TSCUtility)
