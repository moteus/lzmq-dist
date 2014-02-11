cmake_minimum_required ( VERSION 2.8 )

project ( lzmq C )
include ( cmake/dist.cmake )
include ( lua )

find_package ( ZMQ REQUIRED )

include_directories ( ${CMAKE_CURRENT_SOURCE_DIR}/lzmq/src )
include_directories ( ${ZMQ_INCLUDE_DIR} )

set ( LUA_LZMQ_SRC
  lzmq/src/lzmq.c     lzmq/src/lzutils.c lzmq/src/poller.c 
  lzmq/src/zcontext.c lzmq/src/zerror.c  lzmq/src/zmsg.c 
  lzmq/src/zpoller.c  lzmq/src/zsocket.c
)

set ( LUA_LZMQ_DEFINE LUAZMQ_USE_SEND_AS_BUF LUAZMQ_USE_TEMP_BUFFERS LUAZMQ_USE_ERR_TYPE_OBJECT )

set ( LUA_LZTIMER_SRC lzmq/src/ztimer.c lzmq/src/lzutils.c )

install_lua_module ( lzmq ${LUA_LZMQ_SRC} LINK ${ZMQ_LIBRARY} )

set_target_properties ( lzmq PROPERTIES COMPILE_DEFINITIONS "${LUA_LZMQ_DEFINE}" )

install_lua_module ( lzmq.loop          lzmq/src/lua/lzmq/loop.lua         )
install_lua_module ( lzmq.poller        lzmq/src/lua/lzmq/poller.lua       )
install_lua_module ( lzmq.threads       lzmq/src/lua/lzmq/threads.lua      )
install_lua_module ( lzmq.ffi           lzmq/src/lua/lzmq/ffi.lua          )
install_lua_module ( lzmq.ffi.api       lzmq/src/lua/lzmq/ffi/api.lua      )
install_lua_module ( lzmq.ffi.error     lzmq/src/lua/lzmq/ffi/error.lua    )
install_lua_module ( lzmq.ffi.loop      lzmq/src/lua/lzmq/ffi/loop.lua     )
install_lua_module ( lzmq.ffi.poller    lzmq/src/lua/lzmq/ffi/poller.lua   )
install_lua_module ( lzmq.ffi.timer     lzmq/src/lua/lzmq/ffi/timer.lua    )
install_lua_module ( lzmq.ffi.threads   lzmq/src/lua/lzmq/ffi/threads.lua  )
install_lua_module ( lzmq.llthreads.ex  lzmq/src/lua/lzmq/llthreads/ex.lua )
install_lua_module ( lzmq.impl.threads  lzmq/src/lua/lzmq/impl/threads.lua )
install_lua_module ( lzmq.impl.loop     lzmq/src/lua/lzmq/impl/loop.lua    )

if(UNIX AND NOT APPLE)
  set ( RT_LIBRARY rt )
else()
  set ( RT_LIBRARY )
endif()

install_lua_module ( lzmq.timer ${LUA_LZTIMER_SRC} LINK ${RT_LIBRARY} )

install_data ( lzmq/README.md lzmq/LICENCE.txt )

install_example ( lzmq/examples/ )

install_test ( lzmq/test/ lzmq/examples/utils.lua )
