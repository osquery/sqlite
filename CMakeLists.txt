# Copyright (c) 2014-present, Facebook, Inc.
# All rights reserved.
#
# This source code is licensed in accordance with the terms specified in
# the LICENSE file found in the root directory of this source tree.

cmake_minimum_required(VERSION 3.13.3)
project(thirdparty_sqlite)

option(SQLITE_GENERATE_INSTALL_TARGET "When enabled, will cause the project to create the install targets" OFF)

function(sqliteMain)
  set(parameter_list
   "SQLITE_VERSION=3.29.0"
   "SQLITE_MAX_VARIABLE_NUMBER=250000"
  )

  set(option_list
    SQLITE_ENABLE_COLUMN_METADATA
    SQLITE_ENABLE_RTREE
    SQLITE_ENABLE_FTS3
    SQLITE_ENABLE_FTS3_PARENTHESIS
    SQLITE_ENABLE_FTS5
    SQLITE_SECURE_DELETE
    SQLITE_ENABLE_UNLOCK_NOTIFY
    SQLITE_ENABLE_DBSTAT_VTAB
    SQLITE_ENABLE_JSON1
    SQLITE_ENABLE_PREUPDATE_HOOK
    SQLITE_ENABLE_SESSION
    SQLITE_SOUNDEX
  )

  add_library(thirdparty_sqlite
    src/sqlite3.c
  )

  target_compile_definitions(thirdparty_sqlite PRIVATE
    ${parameter_list}
  )

  foreach(option ${option_list})
    target_compile_definitions(thirdparty_sqlite PRIVATE
      "${option}=1"
    )
  endforeach()

  target_include_directories(thirdparty_sqlite INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}/src"
  )

  if(SQLITE_GENERATE_INSTALL_TARGET)
    install(TARGETS thirdparty_sqlite DESTINATION lib)
    install(FILES src/sqlite3ext.h src/sqlite3.h DESTINATION include)
  endif()
endfunction()

sqliteMain()
